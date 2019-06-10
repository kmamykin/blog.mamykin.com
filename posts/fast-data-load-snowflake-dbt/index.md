---

permalink: /posts/fast-data-load-snowflake-dbt/
title: "How to incrementally load data into Snowflake with dbt"
author: "Kliment Mamykin"
date: "2019-05-23"
image: ''
tags:
  - analytics
  - data engineering
  - snowflake
  - dbt
---

## TLDR

Most of [dbt](https://getdbt.com) docs and tutorials assume the data is already loaded to Redshift or Snowflake (e.g. by services like [StitchData](https://www.stitchdata.com/) or [Fivetran](https://fivetran.com/)) and accessible with a simple select statement from a table in another database/schema.

In this article I walk though a method to efficiently load data from S3 to Snowflake in the first place, and how to integrate this method with dbt using a custom materialization macro.

## Introduction

Recently I have been exploring how to efficiently load terrabytes of raw data stored in S3 into our new Snowflake account with [dbt](https://getdbt.com).

At the same time our reporting team adopted dbt for all our data loading and transformation needs. dbt is an open-source tool to express data transformations in plain SQL and has worked out great for us as we transitioned dozens of very complicated spaghetti SQL locked in our BI tools into a source controlled, tested, auto-deployed codebase.

## Snowflake stage object basics

Snowflake has a specific database object called [stage](https://docs.snowflake.net/manuals/user-guide-data-load.html) which is responsible for accessing files available for loading. Since we are discussing loading files from S3, we will be referring to an [external S3 stage](https://docs.snowflake.net/manuals/user-guide/data-load-s3-create-stage.html), which encapsulates an S3 location, credentials, encryption key, and file format to access the files. For example creating an external S3 stage looks like this:

```sql
create or replace stage my_s3_stage url='s3://mybucket/raw_files/'
  credentials=(aws_key_id='...' aws_secret_key='...')
  file_format = (type = PARQUET);
```

Once the stage is created, we can [list files](https://docs.snowflake.net/manuals/sql-reference/sql/list.html) in the location the stage is pointing at

```sql
list @my_s3_stage/path
```

and immediately query the content of the files with a simple select query

```sql
select * from @my_s3_stage
```

So far that is all we need to load our first model with dbt.

## First attempt: dbt table materialization

A simple dbt model materialized from an external stage could look like this:

```sql
{{
  config(materialized='table')
}}

select
  $1:field_one::int as field_one,
  $1:field_two::string as field_two
from @my_s3_stage
```

After running a quick test with `dbt run` we can confirm the model was indeed created with the data loaded from the stage. For small size table that is all we need!

But if the data we are loading is of any significant size, this materialization immediately shows its shortcomings. After the first initial load any consequent `dbt run` will re-load the whole table. Why? Because dbt's `table` materialization uses CTAS (create table as select) statement, which can be verified by looking at the generated `target/run/<project name>/<model>.sql` file. For the model above it will look like this:

```sql
create table my_model as (
  select
    $1:field_one::int as field_one,
    $1:field_two::string as field_two
  from @my_s3_stage
)
```

For a large raw table what we really want is to detect and load only new files that were created at the stage S3 location.

## Second attempt: dbt incremental materialization

Lets assume the raw data has `event_time` field which we could use to incrementally load only missing records, and lets re-write our model like this:

```sql
{{
  config(materialized='incremental')
}}

select
  $1:field_one::int as field_one,
  $1:field_two::string as field_two,
  $1:event_time::timestamp as event_time
from @my_s3_stage
{% if is_incremental() %}
  -- this filter will only be applied on an incremental run
  where event_time > (select max(event_time) from {{ this }})
{% endif %}
```

After running this model a second time, you will see that the incremental load takes about the same time as the original full load. Why is that? Because a Snowflake stage is not a table, and `select ... from @my_s3_stage where event_time > ...` will still read every raw file inside the stage, parse it, and then filter it based on `event_time`. That is still a lot of i/o and processing time, especially considering that Snowflake charges for compute time.

Can we do better? We could try something smart with using partitions in S3 (and [partitioning you data](https://aws.amazon.com/blogs/big-data/top-10-performance-tuning-tips-for-amazon-athena/) [is always a good practice](https://docs.snowflake.net/manuals/user-guide/data-load-considerations-stage.html)), or try to explicitly detect new files using `list @my_s3_stage` and loading only those files `... from @my_s3_stage/path/to/new/file.parquet`. But let me save you a few hours - lets learn about Snowflake's [COPY INTO](https://docs.snowflake.net/manuals/sql-reference/sql/copy-into-table.html) command.

## Snowflake's COPY INTO table

What's not immediately apparent after reading documentation on [COPY INTO](https://docs.snowflake.net/manuals/sql-reference/sql/copy-into-table.html) command is that it is [idempotent](https://en.wikipedia.org/wiki/Idempotence), meaning given the same set of staged files it can be run multiple times with the same result - every file will be loaded only once. If no new files were staged, `COPY INTO` will be a noop, and if new files were staged - only those files will be loaded and the content appended to the table. `COPY INTO` automatically keeps metadata on the target table about every file that was loaded into it. [There are limitations to this](https://docs.snowflake.net/manuals/user-guide/data-load-considerations-load.html#loading-older-files), specifically that the load metadata expired after 64 days. If already loaded file was modified resulting a new checksum, `COPY INTO` will load that file again, but will not delete the records loaded from the first version of the file (pre modification). Bottom line - `COPY INTO` will work like a charm if you only append new files to the stage location and run it at least one in every 64 day period.

## Third attempt: custom materialization using COPY INTO

Luckily dbt allows [creating custom materializations](https://docs.getdbt.com/docs/creating-new-materializations) just for cases like this.

Here is how the model file would look like:

```sql
{{
  config(
    materialized='from_external_stage',
    stage_url = 's3://bucket/path/to/model/raw/data/'
  )
}}

select
  $1:field_one::int as field_one,
  $1:field_two::string as field_two
from {{ external_stage() }}
```

And the materialization macro in `macros/from_external_stage_materialization.sql`:

```sql
{% macro external_stage(path='') %}
    @__STAGE_TOKEN__{{path}}
{% endmacro %}

{% macro ensure_external_stage(stage_name, s3_url, file_format, temporary=False) %}
    {{ log('Making external stage: ' ~ [stage_name, s3_url, file_format, temporary] | join(', ')) }}
    create or replace stage {{ 'temporary' if temporary }} {{ stage_name }}
        url='{{ s3_url }}'
        credentials=(aws_key_id='{{ env_var("SNOWFLAKE_AWS_ACCESS_KEY_ID") }}' aws_secret_key='{{ env_var("SNOWFLAKE_AWS_SECRET_ACCESS_KEY") }}')
        file_format = {{ file_format }};
{% endmacro %}

{% materialization from_external_stage, adapter='snowflake' -%}
    {%- set identifier = model['alias'] -%}
    {%- set stage_name = namespace_stage_name(config.get('stage_name', default=identifier ~ '_stage')) -%}
    {%- set stage_url = config.require('stage_url') -%}
    {%- set stage_file_format = config.get('stage_file_format', default='(type = PARQUET)') -%}
    {%- call statement() -%}
        {{ ensure_external_stage(stage_name, stage_url, stage_file_format, temporary=False) }}
    {%- endcall -%}

    {%- set old_relation = adapter.get_relation(schema=schema, identifier=identifier) -%}
    {%- set target_relation = api.Relation.create(schema=schema, identifier=identifier, type='table') -%}

    {%- set full_refresh_mode = (flags.FULL_REFRESH == True) -%}
    {%- set exists_as_table = (old_relation is not none and old_relation.is_table) -%}
    {%- set should_drop = (full_refresh_mode or not exists_as_table) -%}

    -- setup
    {% if old_relation is none -%}
        -- noop
    {%- elif should_drop -%}
        {{ adapter.drop_relation(old_relation) }}
        {%- set old_relation = none -%}
    {%- endif %}

    {{ run_hooks(pre_hooks, inside_transaction=False) }}

    -- `BEGIN` happens here:
    {{ run_hooks(pre_hooks, inside_transaction=True) }}

    -- build model
    {% if full_refresh_mode or old_relation is none -%}
        {#
            -- Create an empty table with columns as specified in sql.
            -- We append a unique invocation_id to ensure no files are actually loaded, and an empty row set is returned,
            -- which serves as a template to create the table.
        #}
        {%- call statement() -%}
            CREATE OR REPLACE TABLE {{ target_relation }} AS (
                {{ sql | replace('__STAGE_TOKEN__', stage_name ~ '/' ~ invocation_id) }}
            )
        {%- endcall -%}
    {%- endif %}

    {# Perform the main load operation using COPY INTO #}
    {# See https://docs.snowflake.net/manuals/user-guide/data-load-considerations-load.html #}
    {# See https://docs.snowflake.net/manuals/user-guide/data-load-transform.html #}
    {%- call statement('main') -%}
        {# TODO: Figure out how to deal with the ordering of columns changing in the model sql... #}
        COPY INTO {{ target_relation }}
        FROM (
            {{ sql | replace('__STAGE_TOKEN__', stage_name)}}
        )
    {% endcall %}

    {{ run_hooks(post_hooks, inside_transaction=True) }}

    -- `COMMIT` happens here
    {{ adapter.commit() }}

    {{ run_hooks(post_hooks, inside_transaction=False) }}

{%- endmaterialization %}

```

## Notes

* The code assumes your data resides in an S3 bucket, for an Azure it needs to be slightly tweaked how the stage is created.

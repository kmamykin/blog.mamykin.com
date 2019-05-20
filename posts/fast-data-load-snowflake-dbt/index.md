---
permalink: /posts/fast-data-load-snowflake-dbt/
title: "Fast Inncremental Data Load into Snowflake with DBT"
author: "Kliment Mamykin"
date: "2019-05-23"
tags: 
  - analytics
  - data engineering
  - snowflake
  - dbt
draft: true
---

Currently at my work I was pondering how to efficiently load terrabites of data stored in S3 to 
our new shiny instance of Snowflake with (dbt)[https://getdbt.com]. We have evolved to an architecture
where all of our data ends up in S3 (either directly written by various data ingestion applications
or offloaded from our Postgres instance). Having a "data lake" as a component of data architecture has been 
broadly discussed lately, and offers numerous advantages (ref). One of which is the ability to make 
the data available for analytial queries by loading it to specialized databases like Redshift, Snowflake, Druid etc.

At the same time our analytics team converted to using `DBT` for all our data loading and transformation needs.
DBT is an open-source tool to express data transformations in plain SQL and has worked out great 
for us as we transitioned dozens of very complicated spagetti SQL embedded in out BI tool 
into a source controlled, tested, auto-deployed codebase. 

(dbt materializations: table, view, incremental)

(how we do incremental load in Redshift and Spectrum)

(why incremental does not work in Snowflake (not performant))

So how do we efficiently and incrementally load data from S3 into Snowflake with DBT?

(look at how Snowflake does data load)

(how to implement snowflake data load with DBT: custom materialization)

(Wish list - list metadata for a table)

## Heading 2

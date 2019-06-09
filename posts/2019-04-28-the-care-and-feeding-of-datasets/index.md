---
permalink: /posts/the-care-and-feeding-of-datasets/
title: "The Care and Feeding of Datasets"
author: "Kliment Mamykin"
date: "2019-05-01"
tags: 
  - markdown
draft: true
---

![Photo by Tom Ezzatkhah on Unsplash](tom-ezzatkhah-103592-unsplash.jpg)


## Introduction

Main position of this article: growing and curating a dataset is the main problem to solve 
at most companies to derive value from Machine Learning and AI. Building/testing/deploying a ML model is relatively cheap
given an abundance of open source frameworks, services, learning courses. However building a dataset to solve a business 
problem takes 90% effort. While its generally acknowledged that the effort to build such a dataset is dispproportionally large comparing to the effort to built a ML model, 
there is not a lot of conceptual frameworks and principled approaches that help chart the course how to approach such a problem. 

For a lot of non-high-tech businesses (excluding Google/Facebook/Amazon application of very sophisticated models) a vast number of problems 
can be solved or simplified or augmented with simple models (regressions, random trees, etc) without having to reach to a complicated deep learning NN.

What about startups that focus on the area? market research?

Crowdsensing potentially has a different cost of additional unlabeled example (high) vs cost of labeling... 
Talk about spectrum of applications with acquisition (high) + annotation (low) vs acquisition (low) + annotation (high).

This article attempts to reframe the conversation with a larger focus on dataset curation and chart the course for an implementation 
of 

### General questions

### Growing Dataset

Goal: Bootstrap dataset (to avoid cold start problem)

Concerns:

* Balanced
* Diverse
* Ambiguous
* Size - when is it to have enough data
* (robustness, generalizability, and variability) from 

Approaches:

* Random sample (with class balance)
* Visual-Interactive labeling
* Clustering and adding cluster centroids
* Active Learning - uncertainty sampling

### Feeding Dataset

Concerns

* Cost
* Accuracy

Approaches

* Human labeling
    * Experts
    * Crowd-sourcing (noisy, sparse)
* Data Programming
    * Program Labeling Functions
    * Automatic Labeling Functions selection
* Label aggregation
* Active Learning (error/variance reduction methods, since have access to production model and lots of classified examples)

### Pruning & Fixing

Goal: Grow and Refine dataset with real-use examples

Approaches

* QA results of production model
* Customer feedback
* Uncertainty sampling (is Uncertainty sampling on production classified examples the same as error/variance reduction methods? )
* Adversarial examples
* Synthetic data

Photo by jesse orrico on Unsplash jesse-orrico-184803-unsplash.jpg

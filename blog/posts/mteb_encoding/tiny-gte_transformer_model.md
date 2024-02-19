---
title: tiny-gte  "Tiny, yet powerful, it is small in size but packs a lot of power."
description: let's explore this applications with txtai and other workflow with some notes and future directions
date: 2023-10-21
draft: false
tags:
  - nlp
  - transformers
  - embedding
  - txtai
  - vector_db
  - architecture
  - Idea_Forge
  - publish
authors:
  - kareem
---

## Table of contents

#definition : This is a [sentence-transformers](https://www.sbert.net/) model: It maps sentences & paragraphs to a 384 dimensional dense vector space and can be used for tasks like clustering or semantic search. It is distilled from `thenlper/gte-small`, with comparable (slightly worse) performance at around half the size.

## Details

- It's around ~45MB very small compared to other models like MiniLM-L6-V2 which is equal to ~80 MB
- Embedding vector size 384d
- BERT based
- Distilied from thenlper/gte-small,

## Notice about using small size

## Use Cases

# Todo

1. [x] Explain the MTEB
   1. Add the link of it into the this blog
2. Explain fastembed
3. Make comparison between tiny-gte and small gte
4. Add the small-gte-4096 comparison

# References

- https://huggingface.co/TaylorAI/gte-tiny
- https://www.linkedin.com/posts/prithivirajdamodaran_%3F%3F%3F%3F-%3F%3F%3F%3F-%3F%3F%3F%3F%3F-%3F%3F%3F%3F-activity-7120279840569597952-iwc-/?utm_source=share&utm_medium=member_desktop

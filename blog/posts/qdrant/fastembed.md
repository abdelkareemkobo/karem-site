---
title: "Overview of FastEmbed with Qdrant"
description: "Quick overview of Fastembed"
author: kareem
date: 2025-05-25
draft: true
featured: false
categories:
  - blogging
  - qdrant
image: ""
---

## What is FastEmbed?

**It's lighweight, fast, Python library built for embedding generation.**

For embedding generation i mean any type of embedding like Dense Embedding, Late Interaction and Sparse text. 

which can be text embedding, image (CLIP or Colpali) or any type like audio also. 

It's ready for production and cloud use first class without Pytorch dependenices, and it uses the ONNX Runtime. 
which makes it great candidate for serverless runtimes like AWS Lambda. 

It's not fast by a huge margin compared to the Pytorch but it use data parallelism for encoding large datasets in easier way and uses generator by default so you consume less memory for working with large datases. 

you can use it for : 

1. encoding the data
2. use it for rerankers 
3. easier work with Qdrant 

### Dense models with fastembed


### Add your Dense model with fastembed

### Sparse text embeddings 

SPLADE++
MiniCOl

### Late Interactino models (colBERT & Jinai/Colbert)

### Image Embeddings 

### Late interaction multimodal models (ColPali)

## Rerankers

### extend rerankers

## FastEmbed with Qdrant

## Qdrant + FastEmbed + Late Interaction 


### Supported models and how to submit one?


They [support multiples](https://qdrant.github.io/fastembed/examples/Supported_Models/) and you can add the one you want thought github issues, but i find they already have the best models for your task without overhead.

## How I am using FastEmbed?

It helps me for encoding multiple data for training models. I was using it to encode 100M sentences for training a colbert model Arabic which was very slow and hard in my 
setup and will take a long time more than 2 days for my 2 poor old GPUs. 
... i will share everything but after releasing the model.

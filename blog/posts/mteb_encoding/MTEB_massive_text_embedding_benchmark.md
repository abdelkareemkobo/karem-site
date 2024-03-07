---
title: MTEB Massive Text Embedding Benchmark
description: MTEB  Benchmark aims to provide clarity on how models perform on a variety of embedding tasks and thus serves as the gateway  to finding universal text embedding applicable to a variety of tasks.
date: 2023-10-28
draft: false
featured: true
tags:
  - nlp
  - benchmark
  - huggingface
  - embedding
  - papers
  - "#Idea_Forge"
  - publish
authors:
  - kareem
---
## Introduction

#defintion MTEB: MTEB spans 8 embedding tasks covering a total of 58 datasets and 112 languages. Through the benchmarking of 33 models on MTEB.

## Embedding models

- Text embedding models like GLove lack context awareness and ar=e thus commonly labeled as word embedding model. They consist of a layer mapping each input word to a vector often followed by an averaging layer to provide a final embedding invariant of input length.
- Transformers inject context awareness into language models via self-attention and form the foundation of most recent embedding models.
  - BERT uses the transformer architecture and performs large-scale self-supervised pre-training. The resulting model can directly be used to produce text embeddings via an averaging operation alike Glove.
  - SBERT be beneficial to perform additional fine-tuning of the transformer for competitive embedding performance.
  - _Most recent fine-tuned embedding models use a contrastive loss objective to perform supervised fine-tuned on positive and negative text pairs_
  - #critique Due to the large variety of available pretrained transformers,there is an at least equally large variety of potential text embedding models to be explored.This leads to confusion about which model provides practitioners with the best performance for their embedding use case.

---

## The problem

- The problem with the current evaluation regime of current text embedding models rarely covers the breadth of their possible use cases.
  - #example SimCSE or SBERT solely evaluate on STS and classification tasks,leaving open questions about the transfer ability of the embedding models to search or clustering tasks.
- evaluating embedding methods on many tasks requires implementing multiple evaluation pipelines.
- implementation details like preprocessing or hyperparameters may influence the results making it unclear whether performance improvements simply come from a favorable evaluation pipeline. This leads to the "blind" application of these models to new use cases in industry or requires incremental work to reevaluate them on different tasks.

---

## The solution with this benchmark

- **MTEB** consists of 58 datasets covering 112 languages from 8 embedding tasks:

1. Bitext mining
2. classification
3. clustering
4. pair classification
5. reranking, retrieval
6. STS
7. summarization.

- MTEB software is available open-source1 enabling evaluation of any embedding model by adding less than 10 lines of code.
- Datasets and the MTEB leaderboard are available on the Hugging Face Hub2 .
- We evaluate over 30 models on MTEB with additional speed and memory benchmarking to provide a holistic view of the state of text embedding models. We cover both models available open-source as well as models accessible via APIs, such as the OpenAI Embeddings endpoint
- It aims to sheds light on the weaknesses and strenghts of individual models,such as SimCSE’s[(Gao et al., 2021b)](https://doi.org/10.48550/ARXIV.2004.07180) low performance on clustering and retrieval despite its strong performance on STS.

## The MTEB Desiderata

METB is build on a set of desiderat.

1. **Diversity**:
   - it consists of 58 total datasets, 10 are multilingual, covering 112 different langauges.
   - Sentence-level and paragraph level datasets are included to contrast performance on short and long texts.
2. **Simplicity**
   - It provides a simple API for plugging in any model that given a list of text can produce a vector for each list of texts can produce a vector for each list item with a consistent shape.
3. **Extensibility**
   - you can add new datasets for existing tasks via a single file that specifies the task and a Huggingface dataset name where the data has been uploaded.
   - New tasks require implementing a task interface for loading the data and an evaluator for benchmarking
4. **Reproduciblity**
   - Through versioning at a dataset and software level,they make it easy to reproduce results in METP.
   - JSON files corresponding to all results available in this paper have been made available together with the MTEB benchmark

## Tasks and Evaluation

#definition **Bitext Mining**  
Inputs are two sets of sentences from two different languages. For each sentence in the first set, the best match in the second set needs to be found.

- The matches are commonly translations.
- The Provided model i used to embed each sentence and the closest pairs are found via cosine similarity
- F1 serves as the main metric for bitext mining.Accuracy, precision and recall are also computed.
  #definition **Classification**
- A train and test set are embedded with the provided model.
- The train set embeddings are used to train a logistic regression classifier with 100 maximum iterations, which is scored on the test set.
- The main metric is accuracy with average precision and f1 additionally provided.
  #definition **Clustering**
  Given a set of sentences or paragraphs, the goal is to group them into meaningful clusters.
- A mini-batch k-means model with batch size 32 and k equal to the number of different labels is trained on the embedded texts.
- The model scored using V-measure - V-measure does not depend on the cluster label,thus the permutation of labels does not affect the score.
  #definition **Classification**
  A pair of text inputs is provided and a label needs to be assigned. Labels are typically binary variables denoting duplicate or paraphrase pairs.
- The two texts are embedded and their distance is computed with various metrics (cosine similarity, dot product, euclidean distance, manhattan distance).
- Using the best binary thresh- old accuracy, average precision, f1, precision and recall are computed.
- The average precision score based on cosine similarity is the main metric.
  #definition **Reranking** :
  Inputs are a query and a list of relevant and irrelevant reference texts. The aim is to rank the results according to their relevance to the
  query.
- The model is used to embed the references which are then compared to the query using cosine similarity.
- The resulting ranking is scored for each query and averaged across all queries.
- Metrics are mean MRR@k and MAP with the latter being the main metric.
  #definition **Retrieval**
  Each dataset consists of a corpus, queries and a mapping for each query to relevant documents from the corpus.
- The aim is to find these relevant documents.
- The provided model is used to embed all queries and all corpus documents and similarity scores are computed using cosine similarity. After ranking the corpus documents for each query based on the scores, nDCG@k, MRR@k, MAP@k, precision@k and recall@k are computed for several values of k. nDCG@10 serves as the main metric.
- MTEB reuses datasets and evaluation from BEIR (Thakur et al., 2021).
  #definition **Semantic Textual Similarity (STS)**
  Given a sentence pair the aim is to determine their similarity.
  Labels are continuous scores with higher numbers indicating more similar sentences.
- The provided model is used to embed the sentences and their similarity is computed using various distance metrics.
- Distances are benchmarked with ground truth similarities using Pearson and Spearman correlations.
- Spearman correlation based on cosine similarity serves as the main metric (Reimers et al.,2016).
  #definition **Summarization**
  A set of human-written and machine-generated summaries are provided. The aim is to score the machine summaries.
  The provided model is first used to embed all summaries.
  For each machine summary embedding, distances to all human summary embeddings are computed.
  The closest score (e.g. highest cosine similarity) is kept and used as the model’s score of a single machine-generated summary. Pearson and Spearman correlations with ground truth human assessments of the machine-generated summaries are computed. Like for STS, Spearman correlation based on cosine similarity serves as the main metric

---

#sidenote **Non-Transformers** : LASER (Heffernan et al.,2022) is the only context aware non-transformer model we benchmark, relying on an LSTM (Hochreiter and Schmidhuber, 1997) instead. Similar to LaBSE, the model trains on parallel data and focuses on bitext mining applications.
![[Pasted image 20231028124555.png]]

## Analysis

- we observe that there is considerable variability between tasks. No model claims the state-of-the-art in all seven English tasks.
- There is even more variability in the results per dataset present in the appendix.
- Further, there remains a large gap between self-supervised and supervised methods.
- Self-supervised large language models have been able to close this gap in many natural language generation tasks (Chowd- hery et al., 2022).
- However, they appear to still require supervised fine-tuning for competitive em- bedding performance.
- We find that performance strongly coorelates with model size, A majority of MTEB tasks are domainted by multi-billion parameter models.However, these come at a significant cost
- **For classification**
  - ST5 models dominate the classification task across most datasets
  - ST5-XXL has the highest average performance, 3% ahead of the best non-ST5 model, OpenAI Ada Similarity
- **Clustering**
  - Despite being almost 50x smaller, the MPNet embedding model is on par with the ST5- XXL state-of-the-art on Clustering. This may be due to the large variety of datasets MPNet (and MiniLM) has been fine-tuned on.
  - Clustering requires coherent distances between a large number of embeddings.
  - Models like SimCSE-sup or SGPTnli, which are only fine-tuned on a single dataset,NLI, may produce incoherent embeddings when encountering topics unseen during fine-tuning.
  - Relatedly, we find that the query embeddings of SGPT-msmarco and the Ada Search endpoint are competitive with SGPT-nli and the Ada Similarity endpoint,respectively.
  - We refer to the public leaderboard5 for Ada Search results. This could be due to the MSMARCO dataset being significantly larger than NLI.
  - Thus, while the OpenAI docs recommend using the similarity embeddings for clustering use cases6 , the retrieval query embeddings may be the better choice in some cases.
- **Pair Classification**
  - GTR-XL and GTR-XXL have the strongest performance. Pair classification is closest to STS in its framing, yet models rank significantly differently on the two tasks. This highlights the importance of benchmarking on a diverse set of tasks to avoid blindly reusing a model for a different task.
- **Reranking**
  - MPNet and MiniLM models perform strongly on reranking tasks.
  - On SciDocsRR (Co-han et al., 2020a) they perform far better than big- ger models, which is likely due to parts of SciDocsRR being included in their training data.
  - Our scale of experiments and that of model pre-training make controlling for data contamination challenging.
  - Thus, we ignore overlap of MTEB datasets with model training datasets in MTEB scores.
  - As long as enough datasets are averaged, we believe these effects to be insignificant.
- **Retrieval**
  - SGPT-5.8B-msmarco is the best em- bedding model on the BEIR subset in MTEB as well as on the full BEIR benchmark (Thakur et al., 2021; Muennighoff, 2022).
  - The even larger 7.1B SGPT model making use of BLOOM (Scao et al., 2022) performs significantly weaker, which is likely due to the multilinguality of BLOOM.
  - Models geared towards STS (SimCSE, ST5, SGPT- nli) perform badly on retrieval tasks.
  - Retrieval tasks are unique in that there are two distinct types of texts: Queries and documents (“asymmetric”), while other tasks only have a single type of text (“symmetric”).
  - On the QuoraRetrieval dataset, which has been shown to be largely symmetric (Muennighoff, 2022), the playing field is more even with SGPT-5.8B-nli outperforming SGPT- 5.8B-msmarco,
- **STS & Summarization**
  - Retrieval models (GTR, SGPT-msmarco) perform badly on STS, while ST5-XXL has the highest performance.
  - This highlights the bifurcation of the field into separate embedding models for retrieval (asymmetric) and similarity (symmetric) use cases (Muennighoff, 2022).

## Efficiency

**Maximum speed** -> Word Embedding models offer maximum speed with Glove taking the lead on both performance and speed, thus making the choice simple in this case

**Maximum performance** -> If latency is less important than performance Depending on the task at hand, GTR-XXL, ST5-XXL or SGPT-5.8B may be the right choice,

**Speed and Peformance** -> The fine-tuned MPNet and MiniLM models lead the middle cluster making the choice easy.

---

#todo you can check the gte architecture here it's highly related to the MTEP [[tiny-gte_transformer_model]]

---

## Conclusion

- We found model performance on different tasks to vary strongly with no model claiming state-of-the-art on all tasks.
- Our studies on scaling behavior, model efficiency and multilinguality revealed various intricacies of models that should ease the decision-making process for future research or industry applications of text embeddings.

---

Thanks for reading. If you have any questions, feel free to comment down below or reach out to me on twitter [@AbdelkareemElk1](https://twitter.com/AbdelkareemElk1).

## References

- https://huggingface.co/blog/mteb
- https://arxiv.org/abs/2210.07316

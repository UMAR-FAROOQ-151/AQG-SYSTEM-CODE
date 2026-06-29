# Syllabus Aligned Automated Question Generation Using Retrieval Augmented and Fine Tuned Large Language Models
## Overview

This project presents an intelligent question generation framework based on Retrieval-Augmented Generation (RAG) and parameter-efficient fine-tuning of Large Language Models. The system is designed to generate syllabus-aligned academic questions by retrieving relevant educational content before generation. Two instruction-tuned language models, **LLaMA 3** and **Mistral**, are implemented following the same research methodology and evaluated under identical experimental settings.

The proposed framework integrates dense retrieval, sparse retrieval, hybrid retrieval, and cross-encoder reranking to improve context selection before question generation. The generated questions are evaluated using both retrieval-based and generation-based metrics.

---

# Research Objectives

The objectives of this implementation are:

* Develop a syllabus-aware automatic question generation system.
* Improve retrieval quality using a hybrid retrieval pipeline.
* Fine-tune open-source Large Language Models using QLoRA.
* Compare different retrieval strategies.
* Evaluate the effectiveness of fine-tuned models against their corresponding base models.

---

# Project Structure

```
Project
│
├── dataset.json
├── syllabus.json
├── golden_queries.json
│
├── outputs/
│
├── llama3_finetuned_qg_best/
├── mistral_finetuned_qg_best/
│
├── evaluation graphs
├── retrieval evaluation
├── dataset visualizations
│
└── training notebook
```

---

# Methodology

The complete workflow consists of the following stages.

## 1. Data Preparation

Three datasets are used throughout the project.

* Academic question generation dataset
* Structured syllabus knowledge base
* Golden query benchmark for retrieval evaluation

The syllabus is divided into overlapping chunks to preserve semantic continuity while enabling efficient retrieval.

---

## 2. Retrieval Pipeline

Four retrieval strategies are implemented and compared.

### Dense Retrieval

Semantic embeddings are generated using the MixedBread embedding model and indexed with FAISS for similarity search.

### Sparse Retrieval

Keyword-based retrieval is performed using BM25.

### Dense-BM25 Hybrid

Results from dense retrieval and BM25 are combined using Reciprocal Rank Fusion (RRF).

### Proposed Retrieval Model

The proposed approach combines:

* Dense Retrieval
* BM25 Retrieval
* Candidate Fusion
* Cross-Encoder Reranking

The Cross-Encoder ranks candidate passages according to their semantic relevance to the input query before selecting the final context for generation.

---

# Retrieval Components

Embedding Model

* mixedbread-ai/mxbai-embed-large-v1

Vector Database

* FAISS

Sparse Retriever

* BM25

Reranker

* cross-encoder/ms-marco-MiniLM-L-6-v2

---

# Fine-Tuning

Both LLaMA and Mistral models are fine-tuned using Parameter-Efficient Fine-Tuning (PEFT).

The implementation uses:

* QLoRA
* 4-bit quantization
* LoRA adapters
* Early stopping
* Mixed precision training

Training is performed using the Unsloth framework to improve memory efficiency and reduce training time.

---

# Prompt Design

The model follows an instruction-based prompting strategy.

Each prompt contains:

* System instructions
* User request
* Retrieved syllabus context

The model is restricted to generating questions only from the retrieved syllabus content without introducing external knowledge.

---

# Model Evaluation

## Retrieval Evaluation

The retrieval pipeline is evaluated using the following metrics.

* Hit@5
* Precision@5
* Recall@5
* F1 Score
* Mean Reciprocal Rank (MRR)
* Mean Average Precision (MAP)
* Normalized Discounted Cumulative Gain (nDCG)
* ROUGE-L
* BERTScore

Each retrieval strategy is evaluated using the same golden query benchmark.

---

## Question Generation Evaluation

Generated questions are evaluated using:

* Dist-1
* Dist-2
* Self-BLEU
* Syllabus Alignment

The evaluation measures diversity, redundancy, and semantic consistency between generated questions and retrieved syllabus content.

---

# Comparative Experiments

The implementation performs multiple comparisons.

## Retrieval Comparison

* Dense Retrieval
* BM25
* Dense + BM25 Hybrid
* Proposed Hybrid Retrieval with Cross-Encoder Reranking

## Model Comparison

For both LLaMA and Mistral:

* Base Model
* Fine-Tuned Model

Experiments are conducted both with and without explicit system instructions.

---

# Dataset Analysis

The project also performs exploratory analysis of the training dataset, including:

* Question difficulty distribution
* Bloom's Taxonomy distribution
* Question type distribution

These visualizations help understand dataset characteristics before model training.

---

# Libraries

Major libraries used in this implementation include:

* PyTorch
* Transformers
* Unsloth
* PEFT
* TRL
* Accelerate
* Sentence Transformers
* FAISS
* BM25
* NumPy
* Matplotlib
* Seaborn
* NLTK
* ROUGE Score
* BERTScore

---

# Output

The implementation produces:

* Fine-tuned LLaMA model
* Fine-tuned Mistral model
* Retrieval evaluation results
* Generation evaluation results
* Comparative performance tables
* Dataset visualizations
* Retrieval comparison graphs

---

# Reproducibility

To reproduce the experiments:

1. Install all required libraries.
2. Place the dataset files in the project directory.
3. Run the preprocessing stage.
4. Build the retrieval indexes.
5. Fine-tune the selected language model.
6. Evaluate retrieval performance.
7. Evaluate generated questions.
8. Compare the base and fine-tuned models.

---

# Notes

The generated questions are restricted to the retrieved syllabus content. The retrieval stage is responsible for selecting the most relevant educational material before generation, ensuring that the language model produces syllabus-aligned questions while minimizing the influence of external knowledge.

The same experimental pipeline is implemented for both LLaMA and Mistral to provide a consistent comparison across models.

# RAG Architecture

> **Chapter 5 – Retrieval-Augmented Generation (RAG)**

> *Building Enterprise Knowledge Platforms for AI Agents*

---

# Learning Objectives

After completing this section you should be able to:

- Explain the complete RAG architecture.
- Understand indexing and retrieval pipelines.
- Explain how RAG integrates with Agent Runtime.
- Understand knowledge flow.
- Design enterprise RAG systems.
- Explain production RAG architecture in interviews.

---

# Why RAG Architecture Matters

Many engineers think RAG is

```
Question

↓

Vector Search

↓

LLM
```

In reality,

a production RAG system contains

multiple pipelines

working together.

The two major pipelines are

```
Indexing Pipeline

and

Retrieval Pipeline
```

One prepares knowledge.

The other retrieves it.

---

# First Principle

RAG separates

```
Knowledge Preparation
```

from

```
Knowledge Consumption.
```

This separation allows enterprise knowledge to evolve without retraining the language model.

---

# Mental Model

Imagine a library.

Librarians

```
Receive Books

↓

Catalogue

↓

Organize

↓

Index
```

Readers

```
Ask Question

↓

Search

↓

Retrieve

↓

Read
```

The librarian and the reader perform different workflows.

RAG works the same way.

---

# The Two Pipelines

```
Knowledge Pipeline

↓

Documents

↓

Processing

↓

Indexing

──────────────

Runtime Pipeline

↓

Question

↓

Retrieval

↓

Grounded Answer
```

One pipeline runs

offline.

The other runs

online.

---

# High-Level Architecture

```
                 Enterprise Knowledge
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
     PDFs           Databases         Wikis
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                Document Processing
                         ▼
                    Chunking
                         ▼
                   Embedding Model
                         ▼
                    Vector Database
                         │
────────────────────────────────────────────────
                         │
                    User Question
                         ▼
                 Query Understanding
                         ▼
                   Query Embedding
                         ▼
                    Vector Search
                         ▼
                     Top-K Results
                         ▼
                     Re-ranking
                         ▼
                  Grounded Context
                         ▼
                   Prompt Builder
                         ▼
                         LLM
                         ▼
               Evidence-Based Response
```

---

# Indexing Pipeline

The indexing pipeline prepares knowledge.

```
Knowledge Source

↓

Parser

↓

Cleaner

↓

Chunking

↓

Embeddings

↓

Index

↓

Vector Database
```

This pipeline usually runs

before

users ask questions.

---

# Retrieval Pipeline

The retrieval pipeline starts

only after

a user asks a question.

```
Question

↓

Embedding

↓

Retriever

↓

Top-K Documents

↓

Prompt Builder

↓

LLM
```

This pipeline must be fast.

---

# Why Separate the Pipelines?

Imagine

10 million documents.

Embedding them

for every question

would be extremely expensive.

Instead

compute embeddings once,

reuse many times.

This separation dramatically improves efficiency.

---

# Components of a RAG System

Every production system includes:

- Knowledge Sources
- Document Processing
- Chunking
- Embeddings
- Index
- Retriever
- Re-ranker
- Prompt Builder
- LLM

Each component has a single responsibility.

---

# Knowledge Sources

Examples

- Product documentation
- Invoices
- Contracts
- HR policies
- Wikis
- Databases
- Support tickets
- APIs

Knowledge sources may be structured or unstructured.

---

# Document Processing

Before retrieval,

documents must be prepared.

Typical tasks include

- parsing
- OCR
- cleaning
- metadata extraction
- language detection
- normalization

Poor preprocessing leads to poor retrieval.

---

# Chunking

Documents are divided into smaller units.

```
Document

↓

Chunks

↓

Embeddings
```

Chunking affects

- retrieval quality
- latency
- token usage

We'll explore chunking in detail later.

---

# Embeddings

Every chunk becomes

a vector.

```
Chunk

↓

Embedding

↓

Vector
```

Vectors capture semantic similarity rather than exact keywords.

---

# Vector Database

The vector database stores embeddings.

Responsibilities

- nearest-neighbour search
- metadata filtering
- indexing
- retrieval

It is

not

the entire RAG system.

---

# Retriever

The retriever receives

the user question.

Pipeline

```
Question

↓

Embedding

↓

Search

↓

Top-K Chunks
```

The retriever selects

candidate knowledge.

---

# Re-ranking

Initial retrieval

is not always optimal.

Re-ranking improves relevance.

```
Top-K

↓

Re-ranker

↓

Best Chunks
```

Only the best evidence

reaches the LLM.

---

# Prompt Builder

The Prompt Builder combines

- system instructions
- retrieved context
- conversation
- user question

into

the runtime prompt.

---

# LLM

The LLM

does not retrieve.

It reasons

over

the retrieved evidence.

This distinction is fundamental.

---

# Running Case Study

Invoice Explainability Agent

```
Question

↓

Invoice Retriever

↓

Pricing Retriever

↓

Tax Retriever

↓

Grounded Context

↓

LLM

↓

Explanation
```

The explanation

references

enterprise knowledge,

not assumptions.

---

# RAG in the Agent Runtime

Chapter 4 Runtime

```
Planner

↓

State

↓

Memory

↓

Tools
```

Chapter 5 extends it.

```
Planner

↓

Knowledge Router

↓

Retriever

↓

Grounded Context

↓

Prompt Builder

↓

LLM
```

The runtime now reasons over organizational knowledge.

---

# Online vs Offline Processing

Offline

```
Ingest

↓

Index

↓

Store
```

Online

```
Question

↓

Retrieve

↓

Generate
```

Understanding this distinction is important for architecture discussions.

---

# Enterprise Architecture

```
                Agent Runtime
                      │
                      ▼
              Knowledge Router
                      │
         ┌────────────┼────────────┐
         ▼            ▼            ▼
    Vector DB     SQL Search    API Search
         │            │            │
         └────────────┼────────────┘
                      ▼
               Unified Retriever
                      ▼
                 Re-ranking
                      ▼
               Grounded Context
                      ▼
                      LLM
```

This pattern is common in enterprise AI platforms.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| Poor chunking | Better chunk strategy |
| Missing documents | Improve ingestion |
| Wrong retrieval | Re-ranking |
| Outdated index | Re-indexing |
| Large context | Context filtering |

---

# Engineering Perspective

Think of RAG as

a Knowledge Platform,

not

a search engine.

The objective is

to provide

the best possible evidence

before reasoning begins.

---

# Production Insight

Production RAG architecture often separates

```
Offline Indexing

↓

Knowledge Store

↓

Online Retrieval

↓

Prompt Builder

↓

LLM
```

Each layer scales independently.

---

# Engineering Notebook

Experiment.

Draw two pipelines.

Pipeline A

Indexing.

Pipeline B

Retrieval.

Question

Which pipeline

runs continuously?

Which pipeline

runs only when

a user asks a question?

Document your reasoning.

---

# Common Misconceptions

## "Vector DB equals RAG."

False.

The Vector Database is one component.

---

## "The LLM performs retrieval."

False.

Retrieval happens before inference.

---

## "Every document should enter the prompt."

False.

Only the most relevant evidence should be retrieved.

---

## "RAG eliminates hallucinations."

False.

Good retrieval reduces hallucination risk,

but evaluation and validation are still required.

---

# Best Practices

✅ Separate indexing from retrieval.

✅ Keep knowledge fresh.

✅ Retrieve only relevant chunks.

✅ Re-rank before generation.

✅ Monitor retrieval quality.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Small document set | Simple RAG | Lower complexity |
| Enterprise knowledge | Multi-stage RAG | Better relevance |
| Frequently changing data | Incremental indexing | Fresh knowledge |
| Large knowledge base | Hybrid retrieval | Better coverage |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable enterprise knowledge.

## Options

1. Prompt only.

2. Simple vector search.

3. Multi-stage RAG architecture.

## Decision

Multi-stage RAG architecture.

## Trade-offs

Pros

- Better grounding
- Better scalability
- Easier maintenance
- Lower hallucination risk

Cons

- Additional infrastructure
- More operational complexity

## Recommendation

Treat RAG as an enterprise knowledge platform with separate indexing and retrieval pipelines.

---

# Key Takeaways

- RAG consists of two major pipelines: indexing and retrieval.
- The LLM reasons over retrieved evidence.
- Vector databases are only one part of the architecture.
- Re-ranking improves retrieval quality.
- Enterprise RAG systems are knowledge platforms.

---

# Interview Questions

### Q1

What are the two major RAG pipelines?

---

### Q2

Why separate indexing from retrieval?

---

### Q3

Why isn't a Vector Database the same as RAG?

---

### Q4

What is the role of the Retriever?

---

### Q5

Why is Re-ranking important?

---

### Q6

How does RAG fit inside an Agent Runtime?

---

### Q7

Draw a production RAG architecture.

---

### Q8

How would you scale a RAG platform?

---

# Hands-on Exercise

## Objective

Design a production RAG architecture.

### Requirements

Include:

- Knowledge Sources
- Document Processing
- Chunking
- Embeddings
- Vector Database
- Retriever
- Re-ranking
- Prompt Builder
- LLM

### Deliverable

Draw the architecture and explain the responsibility of every component.

### Expected Outcome

You should be able to distinguish the offline indexing pipeline from the online retrieval pipeline and explain how both contribute to grounded AI responses.

---

# Production Readiness Checklist

☑ Knowledge sources identified

☑ Indexing pipeline designed

☑ Retrieval pipeline designed

☑ Embedding strategy defined

☑ Re-ranking implemented

☑ Knowledge freshness policy

☑ Retrieval metrics

☑ Decision logs

☑ Observability

---

# Further Reading

- Lewis et al. — Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks
- LangChain Documentation
- LangGraph Documentation
- Qdrant Documentation
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI architecture and retrieval systems
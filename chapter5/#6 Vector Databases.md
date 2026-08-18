# Vector Databases

> **Chapter 5 – Retrieval-Augmented Generation (RAG)**

> *Building semantic search infrastructure for AI systems.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain why Vector Databases exist.
- Understand semantic search.
- Explain Approximate Nearest Neighbor (ANN) search.
- Compare Vector Databases with SQL databases.
- Design production vector architectures.
- Select an appropriate vector database.
- Explain vector search during interviews.

---

# Why Vector Databases Exist

Suppose your enterprise has

```
80 million

knowledge chunks.
```

Question

Can we compare

every vector

against

every query?

Yes.

But it would be far too slow.

Vector Databases exist

to perform

fast semantic search

over very large collections of embeddings.

---

# First Principle

A Vector Database is

> **A database optimized for storing, indexing and searching high-dimensional vectors using similarity search.**

Notice

its purpose

is search,

not reasoning.

---

# Mental Model

Imagine Google Maps.

Suppose you ask

```
Coffee near me
```

Google Maps

doesn't inspect

every business

in the world.

Instead,

it uses efficient indexes

to quickly locate nearby candidates.

Vector Databases do something similar

for embeddings.

---

# Position in the Knowledge Lifecycle

```
Knowledge Sources

↓

Processing

↓

Chunking

↓

Embeddings

↓

Vector Database

↓

Retriever

↓

LLM
```

The database sits

between

indexing

and

retrieval.

---

# SQL vs Vector Database

Traditional SQL

```
WHERE

invoice_id = 123
```

Exact matching.

Vector Search

```
Question

↓

Embedding

↓

Most Similar Vectors
```

Meaning-based retrieval.

---

# Keyword Search vs Semantic Search

Keyword Search

```
Hotel
```

Finds

```
Hotel
```

Semantic Search

```
Hotel
```

Also finds

```
Accommodation

Lodging

Business Stay
```

Similarity

rather than

exact spelling.

---

# Vector Search Pipeline

```
Question

↓

Embedding

↓

Vector Search

↓

Top-K Results

↓

Prompt Builder

↓

LLM
```

Notice

the LLM

never searches

the database directly.

---

# High-Dimensional Space

Embeddings

often contain

hundreds

or

thousands

of dimensions.

Example

```
[0.14,

0.82,

-0.31,

...

768 dimensions]
```

Humans cannot visualize this space,

but similarity algorithms can.

---

# Nearest Neighbor Search

Question

```
Find the most similar vectors.
```

Simple approach

```
Compare

with

every vector.
```

Too expensive.

---

# Approximate Nearest Neighbor (ANN)

Production systems usually use

Approximate Nearest Neighbor search.

Instead of checking

every vector,

ANN quickly finds

very good candidates.

Advantages

- Fast
- Scalable
- High recall

Trade-off

Approximate,

not guaranteed exact.

---

# Similarity Metrics

Common similarity measures include

- Cosine Similarity
- Euclidean Distance
- Dot Product

Different embedding models may recommend different metrics.

---

# Indexing

Vectors are indexed

for fast retrieval.

Conceptually

```
Vectors

↓

Index

↓

Fast Search
```

The index

is

the performance optimization.

---

# Metadata Filtering

Enterprise retrieval often combines

semantic similarity

with

metadata filters.

Example

```
Department = Finance

AND

Language = English

AND

Version = Current
```

Metadata narrows the search space before ranking.

---

# Top-K Retrieval

Suppose

100,000

documents match.

Return

only

Top-K

```
Question

↓

Retriever

↓

Top 5

↓

LLM
```

The prompt remains concise.

---

# Running Case Study

Invoice Explainability Agent

Question

```
Why is my hotel invoice higher?
```

Pipeline

```
Question

↓

Embedding

↓

Vector DB

↓

Pricing Policy

↓

Tax Rules

↓

Discount Policy

↓

LLM
```

The Vector Database retrieves

candidate evidence.

---

# Popular Vector Databases

Common options include

- Qdrant
- Chroma
- Milvus
- Pinecone
- Weaviate
- FAISS (library rather than a standalone database)

Selection depends on

deployment,

scale,

features,

and

operational requirements.

---

# Choosing a Vector Database

Consider

- Scale
- Latency
- Filtering
- Cloud vs Self-hosted
- High availability
- Cost
- Security
- Operational complexity

Choose based on workload,

not popularity.

---

# Multi-Index Search

Large organizations often maintain

multiple indexes.

Example

```
HR

Finance

Engineering

Legal
```

Retriever

↓

Knowledge Router

↓

Correct Index

This improves relevance.

---

# Hybrid Search

Enterprise systems often combine

```
Keyword Search

+

Vector Search

↓

Merged Results

↓

Re-ranking
```

Hybrid search frequently outperforms either technique alone.

---

# Engineering Perspective

A Vector Database

is

an indexing engine.

It does not

replace

- SQL
- Object Storage
- Data Warehouse

It complements them.

---

# Production Insight

Enterprise architecture

```
Question

↓

Embedding

↓

Knowledge Router

↓

Vector DB

↓

Metadata Filter

↓

Top-K

↓

Re-ranking

↓

Prompt
```

Every stage

is measurable.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| Wrong similarity metric | Evaluate retrieval quality |
| Missing metadata | Enrich documents |
| Stale index | Incremental indexing |
| Slow search | Better indexing strategy |
| Low recall | Tune ANN parameters |

---

# Engineering Notebook

Experiment.

Index

100 documents.

Then

10,000.

Then

100,000.

Measure

- retrieval latency
- memory usage
- recall

Question

How does performance change as the corpus grows?

---

# Common Misconceptions

## "Vector Database equals RAG."

False.

It is one component.

---

## "Vector Databases replace SQL."

False.

SQL retrieves structured records.

Vector Databases retrieve semantic similarity.

---

## "Exact search is always better."

False.

ANN usually provides an excellent balance between speed and retrieval quality.

---

## "Metadata is optional."

False.

Enterprise retrieval often depends heavily on metadata filtering.

---

# Best Practices

✅ Choose the correct similarity metric.

✅ Store metadata.

✅ Monitor retrieval latency.

✅ Tune ANN indexes.

✅ Re-index when embeddings change.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Small prototype | Chroma / FAISS | Simplicity |
| Large enterprise | Qdrant / Milvus / Pinecone / Weaviate | Scalability |
| Frequently changing knowledge | Incremental indexing | Freshness |
| Regulated environments | Self-hosted deployment | Governance |

---

# Engineering Decision Record (EDR)

## Problem

Need scalable semantic retrieval.

## Options

1. SQL only.

2. Full scan.

3. Vector Database with ANN.

## Decision

Vector Database with ANN and metadata filtering.

## Trade-offs

Pros

- Fast retrieval
- Semantic search
- Scalable

Cons

- Additional infrastructure
- Index maintenance
- Embedding management

## Recommendation

Treat Vector Databases as specialized semantic search engines within a broader Knowledge Platform.

---

# Key Takeaways

- Vector Databases enable fast semantic search.
- They store embeddings and metadata.
- ANN provides scalable approximate retrieval.
- Metadata filtering improves precision.
- Vector Databases complement traditional databases.

---

# Interview Questions

### Q1

Why do Vector Databases exist?

---

### Q2

SQL vs Vector Database?

---

### Q3

What is ANN?

---

### Q4

Why is metadata important?

---

### Q5

What is Top-K retrieval?

---

### Q6

Why doesn't the LLM search the database directly?

---

### Q7

How would you choose a Vector Database?

---

### Q8

Draw a production semantic search architecture.

---

# Hands-on Exercise

## Objective

Design a semantic search platform.

### Requirements

Include:

- Embedding generation
- Vector Database
- Metadata filtering
- Top-K retrieval
- Re-ranking
- Prompt Builder

### Deliverable

Draw the architecture and explain the responsibility of each component.

### Expected Outcome

You should be able to explain how semantic search differs from traditional database search and why a Vector Database is only one part of a production RAG system.

---

# Production Readiness Checklist

☑ Vector Database selected

☑ Similarity metric defined

☑ Metadata indexed

☑ ANN configured

☑ Retrieval latency monitored

☑ Incremental indexing

☑ Backup strategy

☑ Security controls

☑ Capacity planning

---

# Further Reading

- Qdrant Documentation
- Milvus Documentation
- Weaviate Documentation
- Pinecone Documentation
- FAISS Documentation
- LangChain Vector Store Interfaces
- ByteByteGo articles on search systems and AI architecture
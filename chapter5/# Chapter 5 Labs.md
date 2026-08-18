# Chapter 5 Labs

> **Retrieval-Augmented Generation (RAG)**

> *Building Enterprise Knowledge Platforms*

---

# Lab Philosophy

Every lab follows the same engineering process.

```
Problem

↓

Hypothesis

↓

Architecture

↓

Implementation

↓

Measurement

↓

Observation

↓

Conclusion

↓

Next Improvement
```

Engineering means

measuring,

not guessing.

---

# Lab 1

## Build Your First RAG

Difficulty

⭐

Time

20 Minutes

---

## Objective

Transform

Prompt-only AI

into

Knowledge-aware AI.

Architecture

```
Question

↓

Retriever

↓

Knowledge

↓

LLM

↓

Answer
```

---

## Build

Use

- one PDF
- one embedding model
- one vector database

---

## Measure

Compare

Prompt-only

vs

RAG.

---

## Expected Outcome

Grounded responses.

---

# Lab 2

## Document Processing

Difficulty

⭐

---

Prepare

multiple document types.

```
PDF

DOCX

HTML
```

Pipeline

```
Parser

↓

Cleaner

↓

Metadata
```

---

Measure

Document quality

before

and

after

processing.

---

Expected

Cleaner retrieval.

---

# Lab 3

## Chunking Strategies

Difficulty

⭐⭐

---

Implement

```
Fixed

Sliding Window

Recursive

Semantic
```

Chunking.

---

Measure

- retrieval precision
- chunk size
- latency
- citation quality

---

Expected

Document-aware chunking

outperforms

fixed chunking.

---

# Lab 4

## Embedding Pipeline

Difficulty

⭐⭐

---

Pipeline

```
Chunks

↓

Embeddings

↓

Vector DB
```

---

Implement

batch embedding.

Add

metadata.

Version

embeddings.

---

Measure

throughput

and

cost.

---

Expected

Reusable knowledge index.

---

# Lab 5

## Semantic Search

Difficulty

⭐⭐⭐

---

Compare

```
Keyword Search
```

vs

```
Vector Search
```

vs

```
Hybrid Search
```

---

Measure

- Recall
- Precision
- Latency

---

Expected

Hybrid retrieval

usually performs best.

---

# Lab 6

## Re-ranking

Difficulty

⭐⭐⭐

---

Pipeline

```
Retriever

↓

Top 20

↓

Re-ranker

↓

Top 5
```

---

Compare

with

retrieval only.

---

Measure

- precision
- answer quality
- citations

---

Expected

Better evidence selection.

---

# Lab 7

## Grounding

Difficulty

⭐⭐⭐⭐

---

Require

every answer

to include

citations.

Example

```
Pricing Policy

Section 4.2
```

---

Measure

hallucination rate.

---

Expected

More trustworthy answers.

---

# Lab 8

## Multi-Source Retrieval

Difficulty

⭐⭐⭐⭐

---

Knowledge

```
PDF

Wiki

SQL

API
```

Implement

Knowledge Router.

---

Measure

retrieval quality.

---

Expected

Enterprise-style retrieval.

---

# Lab 9

## Production RAG

Difficulty

⭐⭐⭐⭐

---

Architecture

```
Knowledge Sources

↓

Retriever

↓

Re-ranker

↓

Grounded Context

↓

LLM
```

---

Add

- caching
- metadata filters
- monitoring

---

Measure

- latency
- cache hit rate
- retrieval quality

---

Expected

Production-ready retrieval.

---

# Lab 10

## Enterprise Knowledge Platform

Difficulty

⭐⭐⭐⭐⭐

---

Build

Version 5

of

Invoice Explainability Agent.

Architecture

```
Knowledge Sources

↓

Ingestion

↓

Processing

↓

Chunking

↓

Embedding

↓

Vector DB

↓

Knowledge Router

↓

Hybrid Retrieval

↓

Re-ranking

↓

Grounded Context

↓

Prompt Builder

↓

LLM

↓

Evidence-Based Response
```

---

Features

✓ Multi-source ingestion

✓ Metadata

✓ Chunking

✓ Embeddings

✓ Hybrid Retrieval

✓ Grounding

✓ Citations

✓ Governance

✓ Monitoring

---

Deliverable

Enterprise

Knowledge Platform.

---

# Bonus Lab

## Knowledge Freshness

Simulate

pricing policy changes.

Pipeline

```
Document Update

↓

Incremental Index

↓

Retriever

↓

LLM
```

Measure

index update time

and

retrieval freshness.

---

Expected

No full re-index.

---

# Engineering Notebook

Every lab

must document

```
Problem

↓

Hypothesis

↓

Architecture

↓

Implementation

↓

Metrics

↓

Observation

↓

Conclusion

↓

Next Improvement
```

---

# Evaluation Rubric

| Category | Points |
|-----------|--------:|
| Document Processing | 10 |
| Chunking | 15 |
| Embedding Pipeline | 10 |
| Retrieval | 20 |
| Grounding | 15 |
| Knowledge Platform | 15 |
| Engineering Notebook | 10 |
| Reflection | 5 |

Total

100

---

# Chapter Completion Checklist

By the end

of Chapter 5

you should be able to

✅ Process enterprise documents

✅ Design chunking strategies

✅ Build embedding pipelines

✅ Use vector databases

✅ Build retrieval pipelines

✅ Implement hybrid retrieval

✅ Add re-ranking

✅ Ground answers

✅ Design an Enterprise Knowledge Platform

---

# Capstone Challenge

Build

Invoice Explainability Agent

Version 5.

Requirements

```
Enterprise Knowledge

+

Document Processing

+

Chunking

+

Embeddings

+

Vector DB

+

Knowledge Router

+

Hybrid Retrieval

+

Grounding

+

Citations

+

Observability
```

Goal

Create

a

production-quality

Enterprise Knowledge Platform.

---

# Reflection Questions

1.

Why did one chunking strategy outperform another?

---

2.

Did hybrid retrieval improve quality?

---

3.

How much did re-ranking improve precision?

---

4.

Were grounded responses more trustworthy?

---

5.

How would you reduce retrieval latency while preserving answer quality?

---

# Production Engineering Challenge

Refactor

the platform

into

independent services.

```
Knowledge Ingestion

↓

Chunking Service

↓

Embedding Service

↓

Knowledge Index

↓

Retriever

↓

Knowledge Router

↓

Prompt Builder

↓

LLM
```

No service

should depend directly

on

the LLM.

The platform

should serve

multiple AI agents.

---

# Success Criteria

A successful implementation should:

- Continuously ingest new knowledge.
- Update indexes incrementally.
- Retrieve relevant evidence.
- Produce grounded answers with citations.
- Support multiple knowledge sources.
- Monitor retrieval quality.
- Scale independently of the LLM.
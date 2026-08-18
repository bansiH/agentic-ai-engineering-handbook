# Retrieval Pipeline

> **Chapter 5 – Retrieval-Augmented Generation (RAG)**

> *Finding the right knowledge before the model starts reasoning.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain Retrieval Pipelines.
- Understand query understanding.
- Explain metadata filtering.
- Understand hybrid retrieval.
- Design production retrieval systems.
- Explain retrieval architecture during interviews.

---

# Why Retrieval Pipelines Exist

Suppose a user asks

```
Why is my Uber Business
hotel invoice higher?
```

Question

Should we search

every document?

No.

Should we retrieve

the first five matches?

Also no.

Production retrieval

is a multi-stage pipeline.

---

# First Principle

Retrieval is

> **The process of finding the smallest set of high-quality evidence required for reasoning.**

The objective is

not

to retrieve

the most documents.

The objective is

to retrieve

the right documents.

---

# Mental Model

Imagine asking

a corporate librarian

```
Show me

the pricing policy

used for

my hotel invoice.
```

The librarian

does not

bring

every document.

They retrieve

only

the relevant material.

That is

the retrieval pipeline.

---

# Position in the Knowledge Lifecycle

```
Knowledge Platform

↓

Vector Database

↓

Retrieval Pipeline

↓

Prompt Builder

↓

LLM
```

Retrieval sits

between

knowledge

and

reasoning.

---

# Production Retrieval Pipeline

```
User Question

↓

Query Understanding

↓

Query Rewriting

↓

Embedding

↓

Retriever

↓

Metadata Filtering

↓

Hybrid Search

↓

Top-K

↓

Re-ranking

↓

Grounded Context

↓

Prompt Builder

↓

LLM
```

Every stage

improves

retrieval quality.

---

# Stage 1

## Query Understanding

Understand

what

the user

actually wants.

Example

```
Explain

my hotel invoice.
```

Intent

↓

Invoice Explanation

This understanding

helps

select

retrieval strategy.

---

# Stage 2

## Query Rewriting

Users often ask

ambiguous questions.

Example

```
What about last month?
```

Rewrite

↓

```
Compare current invoice

with previous month's invoice.
```

The rewritten query

improves retrieval.

---

# Stage 3

## Query Embedding

The question becomes

a vector.

```
Question

↓

Embedding
```

The retriever

searches using

the vector,

not raw text.

---

# Stage 4

## Candidate Retrieval

Initial search.

```
Embedding

↓

Vector Database

↓

Top 50 Results
```

This stage prioritizes recall over precision.

---

# Stage 5

## Metadata Filtering

Filter candidates.

Example

```
Department

=

Finance
```

```
Language

=

English
```

```
Version

=

Current
```

Filtering removes irrelevant knowledge before ranking.

---

# Stage 6

## Hybrid Search

Combine

```
Vector Search

+

Keyword Search
```

Architecture

```
Question

↓

Vector Search

Keyword Search

↓

Merge
```

Hybrid retrieval often performs better than either approach alone.

---

# Stage 7

## Top-K Selection

Suppose

50 candidates exist.

Return

only

Top-K.

Example

```
Top 5
```

Choosing K is an engineering trade-off.

Too small

↓

Miss evidence.

Too large

↓

Waste tokens.

---

# Stage 8

## Re-ranking

Initial retrieval

is approximate.

Re-ranking

improves precision.

```
Top-K

↓

Re-ranker

↓

Best Chunks
```

Only the highest-quality evidence reaches the model.

---

# Stage 9

## Context Assembly

Retrieved evidence

becomes

grounded context.

```
Chunks

↓

Prompt Builder

↓

Runtime Prompt
```

The LLM now reasons

over evidence.

---

# Running Case Study

Invoice Explainability Agent

```
Question

↓

Rewrite

↓

Embedding

↓

Pricing Policy

↓

Tax Rules

↓

Discount Policy

↓

Re-ranker

↓

Prompt

↓

LLM
```

The explanation is

grounded

in retrieved knowledge.

---

# Multi-Source Retrieval

Enterprise systems often retrieve from

```
Vector DB

SQL

Wiki

API

Object Store
```

Knowledge Router

↓

Unified Context

Different sources

can complement each other.

---

# Query Expansion

Sometimes

additional terms improve retrieval.

Example

```
Hotel

↓

Accommodation

↓

Lodging
```

Expansion

increases recall,

but should be used carefully to avoid introducing noise.

---

# Engineering Perspective

Retrieval is

a search problem,

not

an AI problem.

Many retrieval improvements

come from

information retrieval

rather than

larger language models.

---

# Production Insight

Enterprise retrieval architecture

```
Question

↓

Knowledge Router

↓

Retriever

↓

Metadata Filter

↓

Hybrid Search

↓

Re-ranker

↓

Grounded Context

↓

LLM
```

Every stage

is observable.

---

# Retrieval Metrics

Measure

- Recall
- Precision
- MRR
- nDCG
- Retrieval Latency
- Top-K Accuracy
- Citation Accuracy

Do not evaluate retrieval using LLM quality alone.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| Wrong query | Query rewriting |
| Too many results | Metadata filtering |
| Too few results | Query expansion |
| Poor ranking | Re-ranking |
| Outdated documents | Re-indexing |

---

# Engineering Notebook

Experiment.

Retrieve

using

1.

Vector Search.

2.

Keyword Search.

3.

Hybrid Search.

Measure

- retrieval quality
- latency
- answer quality

Question

Which strategy

best supports

enterprise invoices?

---

# Common Misconceptions

## "Vector Search is the retrieval pipeline."

False.

It is one stage.

---

## "Top-K should always be larger."

False.

Larger contexts increase token usage and may reduce precision.

---

## "Metadata filtering is optional."

False.

Enterprise systems rely heavily on metadata.

---

## "The LLM retrieves documents."

False.

The retrieval pipeline executes before inference.

---

# Best Practices

✅ Understand the query.

✅ Rewrite ambiguous questions.

✅ Filter aggressively.

✅ Use hybrid retrieval where appropriate.

✅ Re-rank before generation.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Small corpus | Simple vector retrieval | Simplicity |
| Enterprise search | Hybrid retrieval | Better recall |
| Highly regulated data | Metadata filtering | Governance |
| Large corpus | Re-ranking | Higher precision |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable enterprise retrieval.

## Options

1. Simple vector search.

2. Hybrid retrieval.

3. Multi-stage retrieval pipeline.

## Decision

Multi-stage retrieval pipeline with query understanding, filtering and re-ranking.

## Trade-offs

Pros

- Better retrieval quality
- Lower hallucination risk
- Better grounding

Cons

- Additional latency
- More infrastructure
- Pipeline complexity

## Recommendation

Treat retrieval as a production search system rather than a single vector lookup.

---

# Key Takeaways

- Retrieval is a multi-stage pipeline.
- Query understanding improves search quality.
- Metadata filtering reduces irrelevant results.
- Re-ranking increases precision.
- Retrieval quality determines grounding quality.

---

# Interview Questions

### Q1

What is a Retrieval Pipeline?

---

### Q2

Why rewrite queries?

---

### Q3

What is hybrid retrieval?

---

### Q4

Why use metadata filtering?

---

### Q5

What is Top-K retrieval?

---

### Q6

Why re-rank results?

---

### Q7

How would you evaluate retrieval quality?

---

### Q8

Draw a production Retrieval Pipeline.

---

# Hands-on Exercise

## Objective

Design an enterprise Retrieval Pipeline.

### Requirements

Include:

- Query Understanding
- Query Rewriting
- Embedding
- Vector Search
- Metadata Filtering
- Hybrid Search
- Re-ranking
- Context Assembly

### Deliverable

Produce an architecture diagram and explain the responsibility of each stage.

### Expected Outcome

You should demonstrate how multiple retrieval stages improve grounding and reduce irrelevant context compared with a simple vector search.

---

# Production Readiness Checklist

☑ Query understanding

☑ Query rewriting

☑ Metadata filtering

☑ Hybrid retrieval

☑ Top-K tuning

☑ Re-ranking

☑ Retrieval metrics

☑ Monitoring

☑ Freshness policy

---

# Further Reading

- LangChain Retrieval Documentation
- LangGraph Documentation
- Qdrant Documentation
- BM25 and hybrid search concepts
- Microsoft Agent Framework Documentation
- ByteByteGo articles on search systems and AI architecture
- Information Retrieval literature on ranking and evaluation
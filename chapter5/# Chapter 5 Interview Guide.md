# Chapter 5 Interview Guide

> **Retrieval-Augmented Generation (RAG)**

> *Think Like a Knowledge Platform Engineer*

---

# How To Use This Guide

Do **not** memorize answers.

Instead,

answer every question using

```
Problem

↓

Architecture

↓

Trade-offs

↓

Production

↓

Recommendation
```

Interviewers want to understand

your engineering judgement,

not definitions.

---

# Interview Levels

Questions are organized into

```
Level 1

↓

Level 2

↓

Level 3

↓

Level 4
```

---

# Level 1

## RAG Fundamentals

---

### Q1

Why does RAG exist?

Expected Answer

LLMs possess general knowledge from training,

but enterprise information changes continuously.

RAG retrieves current,

organization-specific knowledge

before inference.

---

### Q2

Training Knowledge

vs

Enterprise Knowledge?

Expected Answer

Training Knowledge

↓

Static

↓

Model Training

Enterprise Knowledge

↓

Dynamic

↓

Retrieved

at runtime.

---

### Q3

Why can't Prompt Engineering replace RAG?

Expected Discussion

Prompts define behavior.

Retrieval provides knowledge.

Missing information cannot be solved by better wording.

---

### Q4

What is Grounding?

Expected Answer

Grounding means

the response is supported

by retrieved evidence

rather than assumptions.

---

### Q5

Why does RAG reduce hallucinations?

Expected Answer

The model reasons over retrieved evidence

instead of relying entirely on internal knowledge.

---

# Level 2

## Knowledge Engineering

---

### Q6

What are the two major RAG pipelines?

Expected Answer

Offline

↓

Knowledge Preparation

Online

↓

Knowledge Retrieval

---

### Q7

Why is Document Processing important?

Expected Discussion

Poor document quality leads to

poor chunking,

poor embeddings,

poor retrieval,

and poor answers.

---

### Q8

Large Chunks

vs

Small Chunks?

Expected Answer

Large chunks

↓

More context

↓

Lower precision.

Small chunks

↓

Higher precision

↓

Less context.

---

### Q9

Why use Embeddings?

Expected Answer

Embeddings transform text into vector representations

that enable semantic similarity search.

---

### Q10

Why version embeddings?

Expected Discussion

Model upgrades,

rollback,

migration,

reproducibility.

---

### Q11

SQL

vs

Vector Database?

Expected Answer

SQL

↓

Exact lookup.

Vector Database

↓

Semantic similarity search.

---

### Q12

What is ANN?

Expected Answer

Approximate Nearest Neighbor search

provides scalable semantic retrieval

without comparing every vector.

---

# Level 3

## Production RAG

---

### Q13

Why use Hybrid Retrieval?

Expected Discussion

Combining

keyword,

semantic,

metadata

often improves retrieval quality.

---

### Q14

Why Re-ranking?

Expected Answer

Retriever maximizes recall.

Re-ranking improves precision.

---

### Q15

Why is Metadata important?

Expected Discussion

Filtering,

governance,

authorization,

citations,

auditability.

---

### Q16

How do you keep enterprise knowledge fresh?

Expected Answer

Incremental indexing,

change detection,

re-embedding,

version management.

---

### Q17

Why separate indexing

from

retrieval?

Expected Answer

Indexing prepares knowledge.

Retrieval serves users.

Different scalability requirements.

---

### Q18

How would you monitor RAG?

Examples

- Retrieval latency
- Recall
- Precision
- Citation rate
- Freshness
- Cost

---

### Q19

What belongs inside

an Enterprise Knowledge Platform?

Expected Discussion

Ingestion

Processing

Chunking

Embedding

Vector DB

Retriever

Knowledge Router

Governance

Evaluation

---

### Q20

How does RAG integrate

with

an Agent Runtime?

Expected Answer

RAG provides grounded context.

The runtime coordinates

planning,

state,

memory,

tools,

and retrieval.

---

# Level 4

## Architecture

---

### Q21

Draw

a Production RAG architecture.

Expected Whiteboard

```
Knowledge Sources

↓

Processing

↓

Chunking

↓

Embedding

↓

Vector DB

↓

Retriever

↓

Grounding

↓

Prompt

↓

LLM
```

---

### Q22

What is

the biggest challenge

in

Production RAG?

Expected Discussion

Knowledge freshness,

not embeddings.

---

### Q23

Should every document

be indexed?

Expected Answer

No.

Access control,

governance,

quality,

and relevance determine

what should be indexed.

---

### Q24

How would you secure

a Knowledge Platform?

Expected Discussion

Authentication

Authorization

Metadata

Classification

Encryption

Audit Logs

---

### Q25

How would you improve

retrieval quality?

Expected Discussion

Better chunking

↓

Metadata

↓

Hybrid Retrieval

↓

Re-ranking

↓

Grounding

---

### Q26

How would you evaluate

RAG?

Possible metrics

- Recall
- Precision
- nDCG
- MRR
- Faithfulness
- Citation accuracy
- Correctness

---

### Q27

Why is Grounding

more important

than

confidence?

Expected Answer

Confidence is a model estimate.

Grounding provides

verifiable evidence.

---

### Q28

How would you design

a Knowledge Router?

Expected Discussion

Finance

↓

Finance Index

HR

↓

HR Index

Legal

↓

Legal Index

---

### Q29

How would you scale

a Knowledge Platform?

Expected Discussion

Independent services

↓

Distributed indexing

↓

Caching

↓

Incremental updates

↓

Multiple indexes

---

### Q30

Explain

the complete

Invoice Explainability

Knowledge Platform.

Expected Answer

Candidate should explain

every stage

from

document ingestion

to

grounded response.

---

# Whiteboard Exercises

---

## Exercise 1

Draw

Knowledge Lifecycle.

---

## Exercise 2

Draw

Offline

vs

Online

Pipelines.

---

## Exercise 3

Draw

Retrieval Pipeline.

---

## Exercise 4

Draw

Enterprise Knowledge Platform.

---

## Exercise 5

Draw

Production RAG Runtime.

---

# Common Interview Traps

---

## Trap

Vector Database

=

RAG.

Reality

Vector Database

is

one component.

---

## Trap

Embeddings

solve

hallucinations.

Reality

Grounding,

retrieval quality,

and evidence

reduce hallucinations.

---

## Trap

Chunk size

doesn't matter.

Reality

Chunking

is

one of the biggest determinants

of retrieval quality.

---

## Trap

Hybrid Search

is unnecessary.

Reality

Enterprise systems often combine

semantic,

keyword,

and metadata search.

---

## Trap

Knowledge

never changes.

Reality

Knowledge freshness

is

one of the largest operational challenges.

---

# Engineering Thought Process

Always answer

using

```
Problem

↓

Architecture

↓

Trade-offs

↓

Reliability

↓

Production
```

Never stop

at definitions.

---

# Chapter 5 Self-Assessment

Can you explain

without notes

✅ RAG

✅ Document Processing

✅ Chunking

✅ Embeddings

✅ Vector Databases

✅ Retrieval

✅ Re-ranking

✅ Grounding

✅ Enterprise Knowledge Platform

If yes,

you are ready

for

Chapter 6

Evaluation Engineering.

---

# Staff Engineer Challenge

Design

an Enterprise Knowledge Platform

supporting

- Multiple knowledge sources
- Incremental indexing
- Hybrid retrieval
- Re-ranking
- Grounding
- Governance
- Knowledge freshness
- Observability
- Evaluation

Explain

why

every component exists.

Discuss

trade-offs,

failure modes,

scaling,

and operational considerations.

---

# Further Reading

- Lewis et al. — *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*
- LangChain Documentation
- LangGraph Documentation
- Qdrant Documentation
- OpenAI Documentation
- Microsoft Agent Framework Documentation
- Information Retrieval literature (BM25, nDCG, MRR)
- ByteByteGo articles on AI architecture, search systems and distributed systems
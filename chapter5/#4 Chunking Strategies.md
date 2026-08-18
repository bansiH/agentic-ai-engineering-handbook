# Chunking Strategies

> **Chapter 5 – Retrieval-Augmented Generation (RAG)**

> *Designing knowledge units for reliable retrieval.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain why chunking exists.
- Understand different chunking strategies.
- Select the appropriate chunking strategy.
- Understand chunk size trade-offs.
- Design enterprise chunking pipelines.
- Explain chunking in interviews.

---

# Why Chunking Exists

Suppose your company has

```
Travel Policy

420 pages
```

Question

Should the entire document become

one embedding?

No.

Now suppose

every word

becomes

one chunk.

Also no.

Both approaches fail.

Chunking exists to create

meaningful,

retrievable,

knowledge units.

---

# First Principle

Chunking is

> **The process of dividing large knowledge sources into meaningful units that maximize retrieval quality while minimizing cost and noise.**

Chunk quality directly influences

retrieval quality.

---

# Mental Model

Imagine a library.

Would every shelf contain

one

book?

No.

Would every shelf contain

one

sentence?

Also no.

Instead,

books are divided into

logical sections.

Chunking follows the same idea.

---

# Knowledge Engineering Pipeline

```
Knowledge Source

↓

Document Processing

↓

Chunking

↓

Embedding

↓

Vector Database

↓

Retriever
```

Chunking sits

between

processing

and

embedding.

---

# Why Large Chunks Fail

Imagine

400-page document

↓

One embedding.

Question

Can retrieval identify

one paragraph

inside that document?

Usually not.

The embedding becomes

too broad.

---

# Why Tiny Chunks Fail

Imagine

one sentence

per chunk.

Question

Does the sentence contain enough context?

Often

No.

Important relationships disappear.

---

# The Chunking Trade-off

```
Large Chunks

↓

More Context

↓

Lower Precision

────────────

Small Chunks

↓

Higher Precision

↓

Less Context
```

Good chunking balances

both.

---

# Characteristics of Good Chunks

A good chunk is

- self-contained
- meaningful
- coherent
- retrievable
- not excessively large
- not excessively small

---

# Chunking Strategies

Several strategies exist.

Each solves a different problem.

---

# Strategy 1

## Fixed-size Chunking

Simplest approach.

```
Document

↓

500 Tokens

↓

500 Tokens

↓

500 Tokens
```

Advantages

- Simple
- Fast

Disadvantages

May split ideas in the middle.

---

# Strategy 2

## Sliding Window Chunking

Chunks overlap.

```
Chunk A

────────────

Chunk B

      ────────────

Chunk C

            ────────────
```

Benefits

Relationships crossing boundaries are preserved.

Trade-off

More storage.

---

# Strategy 3

## Paragraph Chunking

Split by paragraphs.

```
Paragraph

↓

Chunk
```

Works well when paragraphs represent complete ideas.

---

# Strategy 4

## Section-based Chunking

Split using document structure.

```
Heading

↓

Section

↓

Chunk
```

Example

```
Travel Policy

↓

Expense Limits

↓

Chunk
```

Enterprise documentation often benefits from structural chunking.

---

# Strategy 5

## Semantic Chunking

Instead of fixed size,

group semantically related content.

```
Idea

↓

Semantic Chunk
```

This generally produces more meaningful retrieval,

but requires additional processing.

---

# Strategy 6

## Recursive Chunking

A hierarchical approach.

```
Document

↓

Section

↓

Paragraph

↓

Sentence
```

If a chunk is still too large,

split again.

This approach is common in modern frameworks.

---

# Strategy 7

## Parent–Child Chunking

Two representations exist.

Parent

↓

Large Context

Child

↓

Small Retrieval Unit

Retriever selects the child.

The parent provides additional context.

---

# Chunk Metadata

Every chunk should include metadata.

Example

```yaml
document:
Travel Policy

section:
Hotel Expenses

page:
42

version:
3.2
```

Metadata supports

filtering,

governance,

and

citations.

---

# Chunk Size

There is

no universal answer.

Factors include

- document type
- model context window
- retrieval strategy
- user questions
- embedding model

Engineering requires experimentation.

---

# Chunk Overlap

Overlap preserves relationships.

Example

```
Chunk A

The hotel stay...

--------------------

Chunk B

...was extended because...
```

Without overlap,

important context may disappear.

---

# Running Case Study

Invoice Explainability Agent

Knowledge

```
Pricing Policy

Tax Rules

Discount Policy
```

Instead of embedding

entire policies,

create

logical sections.

Example

```
Hotel Pricing

↓

Chunk

Taxi Pricing

↓

Chunk

Cancellation Rules

↓

Chunk
```

Retrieval becomes more precise.

---

# Engineering Perspective

Chunking is

a retrieval optimization problem,

not

a document formatting problem.

The objective is

retrieval quality.

---

# Production Insight

Enterprise chunking pipeline

```
Documents

↓

Structure Detection

↓

Chunk Strategy

↓

Metadata

↓

Embedding
```

Different document types may use different chunking strategies.

---

# Chunk Quality Metrics

Useful metrics include

- retrieval precision
- retrieval recall
- average chunk size
- overlap ratio
- citation accuracy
- answer grounding

Measure,

don't guess.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| Chunks too large | Reduce size |
| Chunks too small | Increase context |
| Split sentences | Recursive or semantic chunking |
| Duplicate overlap | Tune overlap size |
| Missing metadata | Metadata extraction |

---

# Engineering Notebook

Experiment.

Create

three chunking strategies.

1.

Fixed

2.

Sliding Window

3.

Semantic

Retrieve

the same question.

Measure

- precision
- latency
- answer quality

Question

Which strategy best supports invoice explanations?

---

# Common Misconceptions

## "Smaller chunks are always better."

False.

Very small chunks often lose context.

---

## "Larger chunks are always better."

False.

Retrieval becomes less precise.

---

## "Chunk size is the only important factor."

False.

Chunk boundaries,

overlap,

and

metadata

are equally important.

---

## "Every document should use the same chunking strategy."

False.

Legal contracts,

source code,

policies,

and

emails

often require different strategies.

---

# Best Practices

✅ Preserve semantic meaning.

✅ Keep chunks coherent.

✅ Add metadata.

✅ Tune overlap.

✅ Evaluate retrieval quality.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Simple documents | Fixed-size | Simplicity |
| Long reports | Sliding Window | Preserve context |
| Policies | Section-based | Natural structure |
| Legal documents | Recursive | Hierarchical structure |
| Enterprise knowledge | Semantic + Metadata | Highest retrieval quality |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable retrieval.

## Options

1. Whole document.

2. Fixed chunks.

3. Semantic chunking.

4. Recursive chunking.

## Decision

Recursive chunking with metadata and overlap.

## Trade-offs

Pros

- Better retrieval
- Better grounding
- Higher citation accuracy

Cons

- More preprocessing
- Additional storage
- Greater implementation complexity

## Recommendation

Choose chunking strategies based on document structure rather than using one universal approach.

---

# Key Takeaways

- Chunking creates meaningful retrieval units.
- Poor chunking degrades retrieval quality.
- Different document types require different chunking strategies.
- Metadata improves retrieval and governance.
- Chunking should be evaluated empirically.

---

# Interview Questions

### Q1

Why does chunking exist?

---

### Q2

Large chunks vs Small chunks?

---

### Q3

What is Sliding Window chunking?

---

### Q4

What is Recursive chunking?

---

### Q5

What is Semantic chunking?

---

### Q6

Why is metadata important?

---

### Q7

How would you choose a chunk size?

---

### Q8

Draw a production chunking pipeline.

---

# Hands-on Exercise

## Objective

Compare chunking strategies.

### Step 1

Take one enterprise policy document.

### Step 2

Create:

- Fixed-size chunks
- Sliding Window chunks
- Section-based chunks

### Step 3

Index each version separately.

### Step 4

Run identical retrieval queries.

### Step 5

Compare:

- retrieval precision
- answer quality
- citation accuracy
- latency

### Expected Outcome

You should observe that chunking strategy significantly influences retrieval quality and that document-aware chunking often outperforms fixed-size chunking.

---

# Production Readiness Checklist

☑ Chunk strategy selected

☑ Metadata extraction

☑ Overlap configured

☑ Recursive fallback

☑ Chunk quality evaluation

☑ Retrieval metrics

☑ Citation testing

☑ Versioning

☑ Monitoring

---

# Further Reading

- LangChain Text Splitters Documentation
- Recursive Character Text Splitter concepts
- Qdrant Documentation
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI search and retrieval
- Research on semantic chunking and document segmentation
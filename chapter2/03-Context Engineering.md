# Context Engineering

> **Chapter 2 – Prompt & Context Engineering**

> *Context is the fuel that powers reasoning.*

---

# Learning Objectives

After completing this section, you should be able to:

- Explain Context Engineering from first principles.
- Distinguish prompts from context.
- Design context pipelines.
- Understand context sources.
- Build production context assembly pipelines.
- Optimize context for cost and quality.
- Reduce hallucinations using grounded context.

---

# Why Context Engineering Exists

Most engineers believe

```
Better Prompt

↓

Better Answer
```

This is only partially true.

The more accurate architecture is

```
Prompt

+

Relevant Context

↓

LLM

↓

Better Answer
```

Modern enterprise AI systems spend more engineering effort on **context** than on prompt wording.

---

# First Principle

Prompt Engineering answers:

> **How should the model behave?**

Context Engineering answers:

> **What information should the model reason over?**

This distinction is one of the most important concepts in modern AI engineering.

---

# Mental Model

Imagine hiring an expert financial consultant.

Without context

```
Question

↓

Guess
```

With context

```
Question

↓

Invoice

↓

Pricing Rules

↓

Tax Rules

↓

Reasoning
```

The consultant didn't become smarter.

They simply received the right information.

LLMs behave the same way.

---

# Prompt vs Context

Prompt

```
You are an expert invoice analyst.
```

Context

```
Invoice

Pricing Policy

Tax Rules

Discount Rules

Customer Contract
```

Prompt defines behavior.

Context provides knowledge.

---

# Context Sources

Production AI systems rarely rely on one source.

Typical context sources include:

```
Conversation History

↓

Retriever (RAG)

↓

Tool Outputs

↓

User Profile

↓

Business Policies

↓

Database Queries

↓

External APIs
```

All of these may contribute to the final prompt.

---

# Context Architecture

```
User Request

↓

Context Sources

├── Conversation

├── Retriever

├── Tools

├── Database

├── User Profile

└── Policies

↓

Context Builder

↓

Prompt Builder

↓

LLM
```

Notice

the LLM is **not** responsible for finding context.

That is application logic.

---

# Static Context

Static context rarely changes.

Examples:

- Company values
- Invoice explanation style
- Output format
- Safety rules
- Compliance requirements

Static context usually belongs in:

- System Prompt
- Configuration
- Prompt Templates

---

# Dynamic Context

Dynamic context changes every request.

Examples:

- Current invoice
- Current pricing rules
- Current tax rules
- Customer account
- Recent conversation

Dynamic context should be retrieved,

not hardcoded.

---

# Retrieved Context (RAG)

Architecture

```
Question

↓

Embedding

↓

Vector Search

↓

Relevant Chunks

↓

Context Builder

↓

Prompt
```

Only the relevant information is injected.

This reduces hallucinations and token usage.

---

# Tool Context

Sometimes the required information is not in documents.

Example

```
Current Invoice

↓

Invoice API

↓

JSON

↓

Prompt
```

The output of tools becomes context.

---

# Conversation Context

Multi-turn conversations require context continuity.

```
User

↓

Conversation Memory

↓

Context Builder

↓

Prompt
```

Conversation history should be summarized or filtered when necessary to control token usage.

---

# Context Assembly

A production pipeline typically assembles context from multiple sources.

```
Conversation

+

Retriever

+

Tool Results

+

Policies

+

User Request

↓

Context Builder

↓

Prompt Builder

↓

LLM
```

This assembled context becomes the runtime prompt.

---

# Context Budget

The context window is finite.

Therefore,

every piece of context competes for space.

Think of context as a budget.

```
Context Window

+----------------------+
| System Prompt        |
| Conversation         |
| Retrieved Chunks     |
| Tool Results         |
| User Prompt          |
+----------------------+
```

Adding unnecessary information consumes budget without adding value.

---

# Good Context vs Bad Context

### Good

```
Invoice

Relevant Pricing Rule

Relevant Tax Rule

Customer Contract
```

### Bad

```
Entire Employee Handbook

All Pricing Policies

All Historical Invoices

Complete Knowledge Base
```

Quality matters more than quantity.

---

# Context Ranking

Not every document deserves inclusion.

Production systems often:

1. Retrieve candidates.
2. Rank by relevance.
3. Keep only the highest-value context.

Architecture

```
Retriever

↓

20 Documents

↓

Ranker

↓

Top 5

↓

Prompt
```

---

# Context Filtering

Filtering removes noise.

Examples:

- Duplicate documents
- Expired policies
- Unrelated invoices
- Low-confidence retrievals

This improves grounding and reduces cost.

---

# Context Freshness

Some information changes frequently.

Examples:

- Exchange rates
- Tax rates
- Inventory
- Invoice status

Production systems should retrieve fresh information rather than relying on stale prompts.

---

# Running Case Study

Invoice Explainability Agent

Without Context

```
Question

↓

LLM

↓

Guess
```

With Context

```
Question

↓

Invoice API

↓

Pricing Rules

↓

Tax Policy

↓

Prompt Builder

↓

LLM

↓

Grounded Explanation
```

Notice the LLM is reasoning over evidence.

---

# Engineering Perspective

Context Engineering is really

**Information Engineering.**

The challenge is no longer:

> "Can the model answer?"

The challenge becomes:

> "Can the system provide the right information before the model answers?"

---

# Hallucination Prevention

Hallucinations often occur because the model lacks reliable context.

Common causes:

- Missing retrieval
- Incorrect retrieval
- Stale documents
- Conflicting documents
- Excessive irrelevant context

The solution is usually **better context engineering**, not simply rewriting the prompt.

---

# Production Insight

A production context pipeline:

```
User

↓

Authentication

↓

Retriever

↓

Tool Calls

↓

Context Filter

↓

Context Ranker

↓

Prompt Builder

↓

LLM
```

Notice the amount of engineering that happens before the model is invoked.

---

# Engineering Notebook

## Experiment

Prepare three versions of the same request.

### Version A

Prompt only.

### Version B

Prompt + Relevant Context.

### Version C

Prompt + Excessive Context.

Measure:

- Token usage
- Latency
- Response quality
- Groundedness

Observation:

Relevant context generally provides the best balance between quality and efficiency.

---

# Common Misconceptions

## "More context is always better."

False.

Irrelevant context can distract the model and increase cost.

---

## "Prompt Engineering solves hallucinations."

Only partially.

Missing or poor-quality context is often the root cause.

---

## "Conversation history should always be included."

False.

Summarization or selective inclusion is often preferable.

---

## "RAG replaces Prompt Engineering."

False.

Prompt Engineering and Context Engineering complement each other.

---

# Best Practices

✅ Retrieve only relevant information.

✅ Keep context fresh.

✅ Remove duplicates.

✅ Rank retrieved documents.

✅ Measure token usage.

✅ Treat context as a scarce resource.

---

# Engineering Decision Record (EDR)

## Problem

Need accurate invoice explanations.

## Options

1. Prompt only.

2. Prompt + entire knowledge base.

3. Prompt + ranked, relevant context.

## Decision

Prompt + ranked, relevant context.

## Trade-offs

Pros

- Better grounding
- Lower hallucination risk
- Lower token cost

Cons

- Requires retrieval infrastructure
- More orchestration logic

## Recommendation

Treat Context Engineering as a first-class engineering discipline.

---

# Key Takeaways

- Context Engineering determines what the model reasons over.
- Good context is more valuable than large context.
- Production systems assemble context dynamically.
- Retrieval, tools and conversation history all contribute to context.
- Better context engineering often reduces hallucinations more effectively than prompt rewriting.

---

# Interview Questions

### Q1

What is Context Engineering?

---

### Q2

How is it different from Prompt Engineering?

---

### Q3

What are common context sources?

---

### Q4

Why is context ranking important?

---

### Q5

What is a context budget?

---

### Q6

Why does excessive context sometimes reduce answer quality?

---

### Q7

How would you reduce hallucinations using Context Engineering?

---

### Q8

Draw a production context assembly pipeline.

---

# Hands-on Exercise

## Objective

Build a Context Builder.

### Step 1

Create:

- System Prompt
- Invoice
- Pricing Rules
- Tax Rules

### Step 2

Assemble them into one runtime prompt.

### Step 3

Repeat using:

- no context
- relevant context
- excessive context

### Step 4

Measure:

- response quality
- latency
- token usage

### Expected Outcome

Relevant, curated context should produce the best balance of correctness, grounding and efficiency.

---

# Further Reading

- LangChain Documentation
- LangGraph Documentation
- Microsoft Agent Framework Documentation
- OpenAI Documentation
- Anthropic Documentation
- Qdrant Documentation
- Chroma Documentation
- ByteByteGo articles on AI architecture and retrieval systems
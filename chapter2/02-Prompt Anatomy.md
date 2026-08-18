# Prompt Anatomy

> **Chapter 2 – Prompt & Context Engineering**

---

# Learning Objectives

After completing this section, you should be able to:

- Explain the different parts of a production prompt.
- Understand System, Developer, User and Assistant messages.
- Separate instructions from data.
- Build maintainable prompt architectures.
- Understand prompt layering.
- Design reusable prompts for enterprise AI systems.

---

# Why Prompt Anatomy Matters

Most beginners think a prompt is

```
Question

↓

LLM

↓

Answer
```

That is suitable for experimentation.

Enterprise AI systems rarely work that way.

Instead they construct prompts from multiple independent components.

---

# First Principle

A production prompt is **not one piece of text.**

It is

a structured software artifact.

Think of it as

```
Configuration

+

Business Rules

+

Context

+

User Input

=

Runtime Prompt
```

---

# Mental Model

Imagine building a web application.

You do not place

- HTML
- CSS
- JavaScript
- SQL
- Configuration

inside one file.

Instead

each has

its own responsibility.

Prompt engineering follows the same principle.

---

# The Prompt Stack

A production prompt typically consists of four layers.

```
System Prompt

↓

Developer Instructions

↓

Context

↓

User Request
```

Each layer solves a different problem.

---

# Layer 1 — System Prompt

The System Prompt defines

**permanent behaviour.**

Examples

```
You are an Invoice Explainability Assistant.

Always answer professionally.

Never invent invoice data.

Cite retrieved policies when available.

Return JSON when requested.
```

Notice

Nothing here changes

between users.

---

## Responsibility

The System Prompt defines

- personality
- constraints
- policies
- response style
- safety rules

Think of it as

the operating system

for the AI.

---

# Layer 2 — Developer Instructions

Developer Instructions define

application behaviour.

Example

```
If the user asks about invoices,

retrieve invoice data first.

If payment exceeds ₹50,000,

require approval.

Never reveal internal identifiers.
```

These are

software decisions,

not user preferences.

---

# System Prompt vs Developer Instructions

System Prompt

```
Who are you?
```

Developer Instructions

```
How should this application behave?
```

They are related,

but not identical.

---

# Layer 3 — Context

Context is

the information

required

to answer the current question.

Example

```
Invoice

Pricing Rules

Tax Rules

Discount Policy
```

Context changes

every request.

---

# Layer 4 — User Prompt

Finally,

the user's request.

Example

```
Why is my invoice ₹12,540?
```

This should remain separate from

system instructions

and

business rules.

---

# Prompt Anatomy

Complete architecture

```
System Prompt

↓

Developer Instructions

↓

Retrieved Context

↓

Conversation

↓

User Request

↓

LLM
```

Everything is assembled

before

calling the model.

---

# Runtime Prompt

The LLM never receives

individual pieces.

Instead

the Prompt Builder combines everything.

```
Template

+

Variables

+

Context

+

Conversation

↓

Runtime Prompt

↓

LLM
```

---

# Engineering Analogy

Think of

building a SQL query.

You do not write

```
SELECT ...

WHERE ...

ORDER BY ...
```

manually

every request.

Instead

templates

+

variables

↓

final query.

Prompt Builders work similarly.

---

# Separation of Concerns

Good prompts separate

instructions

from

data.

Bad

```
You are helpful.

Invoice:

...

Pricing Rules:

...

Tax Rules:

...

Question:

...
```

Everything mixed together.

Good

```
System Prompt

↓

Context

↓

Question
```

Each responsibility

is isolated.

---

# Why This Matters

Suppose

pricing rules change.

Should you edit

the System Prompt?

No.

Only

the retrieved context

changes.

This makes prompts

maintainable.

---

# Conversation History

Enterprise prompts often include

conversation history.

Architecture

```
Previous Messages

↓

Conversation Summary

↓

Runtime Prompt
```

Notice

history

is

context,

not

system instructions.

---

# Prompt Lifecycle

A production prompt evolves through multiple stages.

```
Developer

↓

Prompt Template

↓

Variables

↓

Prompt Builder

↓

Context Injection

↓

LLM

↓

Structured Output

↓

Validator

↓

Application
```

The prompt is generated

just before

the model is called.

---

# Running Case Study

Invoice Explainability Agent.

Prompt Builder

```
System Prompt

↓

Developer Rules

↓

Invoice

↓

Pricing Rules

↓

Tax Rules

↓

Conversation

↓

User Question

↓

LLM
```

This architecture is much easier to maintain than one enormous prompt.

---

# Engineering Perspective

Prompts should be

- version controlled
- reviewed
- tested
- reusable

Treat them like

source code,

not documentation.

---

# Production Insight

Many enterprise systems maintain

a Prompt Registry.

```
Prompt Template

↓

Version

↓

Review

↓

Deployment

↓

Monitoring
```

This enables

safe updates

and

rollback.

---

# Common Misconceptions

## "Everything belongs in the system prompt."

False.

Only permanent behaviour belongs there.

---

## "User input should modify system instructions."

False.

Keep user data separate from application rules.

---

## "Prompt engineering is writing longer prompts."

False.

Prompt quality comes from structure,

not length.

---

## "Conversation history belongs in the system prompt."

False.

Conversation history is runtime context.

---

# Best Practices

✅ Separate instructions from data.

✅ Build prompts programmatically.

✅ Version prompts.

✅ Test prompts.

✅ Keep context dynamic.

---

# Engineering Decision Record (EDR)

## Problem

Need maintainable prompts.

## Options

1. One giant prompt
2. Layered prompt architecture

## Decision

Use layered prompts.

## Trade-offs

Pros

- Easier maintenance
- Better reuse
- Cleaner testing

Cons

- Requires Prompt Builder

## Recommendation

Never build production prompts manually.

Generate them.

---

# Key Takeaways

- A production prompt is a software artifact.
- Prompt layers have different responsibilities.
- System instructions should remain stable.
- Context changes every request.
- Prompt Builders generate runtime prompts.

---

# Interview Questions

### Q1

What is a System Prompt?

---

### Q2

How are Developer Instructions different?

---

### Q3

Why should prompts be layered?

---

### Q4

What belongs in Context?

---

### Q5

Why separate instructions from data?

---

### Q6

What is a Prompt Builder?

---

### Q7

Why should prompts be version controlled?

---

# Hands-on Exercise

## Objective

Build a layered prompt.

### Step 1

Create

- System Prompt
- Developer Instructions
- Context
- User Prompt

### Step 2

Combine them using a Prompt Builder.

### Step 3

Run the prompt.

### Step 4

Change only the context.

Observe how behaviour remains consistent while answers change appropriately.

### Expected Outcome

You should observe that separating prompt layers improves maintainability and reuse.

---

# Further Reading

- LangChain Prompt Templates
- LangGraph Documentation
- OpenAI Prompt Engineering Guide
- Anthropic Prompt Engineering Guide
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI architecture
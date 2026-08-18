# Prompt Templates

> **Chapter 2 – Prompt & Context Engineering**

> *From Hardcoded Prompts to Reusable Prompt Components*

---

# Learning Objectives

After completing this section you should be able to:

- Explain why Prompt Templates exist.
- Separate prompt logic from business logic.
- Build reusable prompt templates.
- Parameterize prompts.
- Version prompt templates.
- Build Prompt Registries.
- Understand PromptOps.

---

# Why Prompt Templates Exist

Suppose your application contains

```
"You are a finance assistant..."
```

inside

50

different Python files.

Now

your company changes

invoice policy.

How many prompts

must be updated?

Probably

50.

This is exactly the problem Prompt Templates solve.

---

# First Principle

A prompt is

**configuration**

not

application code.

Treat prompts

the same way you treat

SQL,

YAML,

HTML,

or

Kubernetes manifests.

---

# Engineering Problem

Hardcoded prompt

```python
response = llm.invoke(
"""
You are an invoice analyst.

Explain the invoice.
"""
)
```

Simple.

Not maintainable.

---

# Better Architecture

```
Prompt Template

↓

Variables

↓

Prompt Builder

↓

Runtime Prompt

↓

LLM
```

Prompt becomes

a reusable component.

---

# Engineering Analogy

Imagine SQL.

Bad

```sql
SELECT * FROM invoices
WHERE customer='ABC'
```

Hardcoded.

Better

```sql
SELECT *

FROM invoices

WHERE customer=?
```

Parameters

improve

reuse.

Prompt Templates work

the same way.

---

# Prompt Template

Instead of

```
Explain this invoice.
```

Create

```
You are an expert invoice analyst.

Customer:

{{customer}}

Invoice:

{{invoice}}

Pricing Rules:

{{pricing}}

Question:

{{question}}
```

The template

never changes.

Only variables change.

---

# Runtime Prompt

Template

```
Customer

{{customer}}
```

Variables

```
customer = ABC Ltd
```

Result

```
Customer

ABC Ltd
```

The Prompt Builder assembles

the final prompt.

---

# Prompt Lifecycle

```
Developer

↓

Prompt Template

↓

Variables

↓

Prompt Builder

↓

Runtime Prompt

↓

LLM

↓

Structured Output
```

This lifecycle is the foundation of PromptOps.

---

# Prompt Variables

Typical variables

```
Customer

Invoice

Pricing Rules

Language

Output Format

Current Date

Conversation
```

Everything dynamic

should be

a variable.

---

# Static vs Dynamic Sections

Static

```
System Instructions
```

Dynamic

```
Invoice

Customer

Policies

Conversation
```

Never mix

the two unnecessarily.

---

# Prompt Builder

The Prompt Builder

has one responsibility.

Assemble prompts.

Architecture

```
Template

+

Variables

+

Context

↓

Runtime Prompt
```

The LLM should never build its own prompt.

---

# Prompt Registry

Enterprise systems

rarely store prompts

inside source code.

Instead

```
Prompt Registry

↓

Version

↓

Environment

↓

Deployment
```

Examples

```
Invoice Prompt

v1

v2

v3
```

Rollback becomes easy.

---

# Prompt Versioning

Suppose

v3

reduces answer quality.

Architecture

```
Prompt

↓

Version

↓

Deployment

↓

Monitoring

↓

Rollback
```

Prompt versioning

should follow

the same discipline

as application releases.

---

# PromptOps

PromptOps

is the operational discipline of managing prompts in production.

Typical lifecycle

```
Author

↓

Review

↓

Version

↓

Testing

↓

Deployment

↓

Monitoring

↓

Improvement
```

Exactly like DevOps,

but for prompts.

---

# LangChain Example

Conceptually

```python
template = PromptTemplate(
    input_variables=["invoice","question"],
    template="""
Explain

{invoice}

Question

{question}
"""
)
```

Notice

the template

contains

placeholders,

not business data.

---

# Flowise Example

```
Chat Input

↓

Prompt Template

↓

Chat Model

↓

Output
```

The template node

receives variables

from earlier nodes.

---

# Enterprise Example

Invoice Explainability Agent

```
Prompt Template

↓

Invoice

↓

Pricing Rules

↓

Customer

↓

Question

↓

Prompt Builder

↓

LLM
```

The same template

works

for millions

of invoices.

---

# Prompt Repository

A typical enterprise repository

```
prompts/

invoice.md

payment.md

refund.md

travel.md
```

Each prompt

has

- owner
- version
- tests
- deployment history

---

# Engineering Perspective

Prompt Templates improve

- reuse
- consistency
- testing
- maintenance
- collaboration

Without templates,

prompt engineering becomes difficult to scale.

---

# Production Insight

Prompt deployment pipeline

```
Git

↓

Prompt Review

↓

Prompt Tests

↓

Deployment

↓

Monitoring
```

Prompt changes

should follow

the same release process

as application code.

---

# Common Misconceptions

## "Prompt Templates are only string replacement."

False.

Templates define

software architecture

for prompts.

---

## "Prompt Templates replace Context Engineering."

False.

Templates define structure.

Context provides information.

---

## "Every prompt needs a new template."

False.

Good templates

support many scenarios

through variables.

---

## "Prompts don't need version control."

False.

Prompt regressions

can break production systems.

---

# Best Practices

✅ Keep templates small.

✅ Parameterize everything dynamic.

✅ Store prompts in Git.

✅ Version prompts.

✅ Test prompt changes.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Prototype | Inline Prompt | Fast iteration |
| Small project | Prompt Template | Reuse |
| Enterprise | Prompt Registry | Governance |
| Multi-team | PromptOps | Versioning & Collaboration |

---

# Engineering Decision Record (EDR)

## Problem

Need maintainable invoice prompts.

## Options

1. Hardcoded prompts.

2. Prompt Templates.

3. Prompt Registry.

## Decision

Prompt Templates with centralized registry.

## Trade-offs

Pros

- Easier maintenance

- Better reuse

- Version control

- Safer deployments

Cons

- Additional infrastructure

## Recommendation

Treat Prompt Templates as software artifacts.

---

# Key Takeaways

- Prompt Templates separate structure from data.
- Variables make prompts reusable.
- Prompt Builders assemble runtime prompts.
- Prompt Registries enable versioning.
- PromptOps applies software engineering practices to prompt management.

---

# Interview Questions

### Q1

Why use Prompt Templates?

---

### Q2

What belongs inside a Prompt Template?

---

### Q3

Why should prompts be version controlled?

---

### Q4

What is a Prompt Registry?

---

### Q5

Explain PromptOps.

---

### Q6

How are Prompt Templates different from Context Engineering?

---

### Q7

Would you hardcode prompts in production?

Why?

---

### Q8

How would you roll back a faulty prompt?

---

# Hands-on Exercise

## Objective

Create a reusable Prompt Template.

### Step 1

Create variables

- invoice
- customer
- pricing
- question

### Step 2

Build the runtime prompt.

### Step 3

Run three different invoices using the same template.

### Step 4

Version the prompt as

v1

and

v2.

### Expected Outcome

You should observe that prompt reuse dramatically simplifies maintenance while preserving consistent behavior.

---

# Production Readiness Checklist

☑ Prompt Template created

☑ Variables parameterized

☑ Prompt stored in Git

☑ Version assigned

☑ Prompt tests added

☑ Prompt review completed

☑ Monitoring configured

☑ Rollback strategy defined

---

# Further Reading

- LangChain Prompt Templates Documentation
- LangGraph Documentation
- OpenAI Prompt Engineering Guide
- Anthropic Prompt Engineering Guide
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI Engineering
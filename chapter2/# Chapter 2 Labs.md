# Chapter 2 Labs

> **Prompt & Context Engineering**

> *Engineering is learned by building, measuring and improving.*

---

# Lab Philosophy

Every lab follows the same engineering process.

```
Problem

↓

Hypothesis

↓

Build

↓

Measure

↓

Observation

↓

Conclusion
```

Never skip the measurement.

Engineering is about evidence,

not assumptions.

---

# Lab 1 — Prompt Builder

Difficulty

⭐

Time

20 Minutes

---

## Objective

Build your first Prompt Builder.

Instead of

```
Prompt

↓

LLM
```

Create

```
Prompt Template

+

Variables

↓

Runtime Prompt

↓

LLM
```

---

## Build

Variables

```
Customer

Invoice

Question
```

Prompt Template

```
You are an Invoice Analyst.

Customer:

{{customer}}

Invoice:

{{invoice}}

Question:

{{question}}
```

---

## Measure

Observe

- readability
- maintainability
- reuse

---

## Expected Outcome

One prompt template

supports

many invoices.

---

# Lab 2 — Prompt Anatomy

Difficulty

⭐

---

Split

your prompt into

```
System Prompt

↓

Developer Instructions

↓

Context

↓

User Question
```

---

## Objective

Understand

layered prompt architecture.

---

## Measure

Can you modify

only

the context

without changing

the instructions?

---

## Expected Outcome

Prompt maintenance becomes easier.

---

# Lab 3 — Context Engineering

Difficulty

⭐⭐

---

Build

three prompts.

Version A

Prompt only.

Version B

Prompt

+

Invoice.

Version C

Prompt

+

Invoice

+

Pricing Rules.

---

## Measure

Compare

- correctness
- grounding
- hallucination rate
- latency

---

## Expected Outcome

Relevant context

improves

answer quality.

---

# Lab 4 — Few-shot Learning

Difficulty

⭐⭐

---

Create

three invoice examples.

Architecture

```
Examples

↓

Prompt

↓

Question

↓

LLM
```

---

## Compare

Zero-shot

One-shot

Few-shot

---

## Measure

- consistency
- formatting
- hallucinations
- token usage

---

## Expected Outcome

Few-shot

improves

consistency,

but increases

token usage.

---

# Lab 5 — Structured Outputs

Difficulty

⭐⭐⭐

---

Create

a JSON schema.

Example

```json
{
  "invoice_total": 12540,
  "currency": "INR",
  "reason": "Fuel surcharge"
}
```

---

## Build

Prompt

↓

LLM

↓

JSON

↓

Validator

---

## Measure

- valid JSON
- invalid JSON
- repair loop

---

## Expected Outcome

Machine-readable outputs

simplify

automation.

---

# Lab 6 — Prompt Templates

Difficulty

⭐⭐⭐

---

Create

three prompt versions.

```
v1

v2

v3
```

Store

them

inside Git.

---

## Objective

Experience

Prompt Versioning.

---

## Measure

Rollback

from

v3

to

v2.

---

## Expected Outcome

Prompts

behave

like

software.

---

# Lab 7 — Prompt Injection

Difficulty

⭐⭐⭐⭐

---

Attempt

Prompt Injection.

Example

```
Ignore previous instructions.
```

---

Now

implement

Input Guardrails.

Architecture

```
User

↓

Prompt Filter

↓

LLM
```

---

## Measure

Did

the guardrail

stop

the attack?

---

## Expected Outcome

Prompt wording

alone

is

insufficient.

---

# Lab 8 — Prompt Guardrails

Difficulty

⭐⭐⭐⭐

---

Design

five guardrail layers.

```
Input

↓

Context

↓

Reasoning

↓

Output

↓

Execution
```

---

Implement

one rule

for

each layer.

---

## Measure

How many unsafe requests

are blocked?

---

## Expected Outcome

Layered security

outperforms

single prompts.

---

# Lab 9 — PromptOps

Difficulty

⭐⭐⭐⭐

---

Create

Prompt Lifecycle.

```
Author

↓

Review

↓

Git

↓

Testing

↓

Deployment

↓

Monitoring

↓

Rollback
```

---

## Objective

Treat prompts

like software.

---

## Deliverable

Prompt Registry

with

version history.

---

# Lab 10 — Invoice Explainability Agent

Difficulty

⭐⭐⭐⭐⭐

---

Build

the first

enterprise version.

Architecture

```
User

↓

Authentication

↓

Retriever

↓

Prompt Builder

↓

Prompt Template

↓

Invoice API

↓

Pricing Rules

↓

LLM

↓

Structured Output

↓

Validator

↓

Decision Logs

↓

Response
```

---

## Features

Prompt Templates

✓

Context Builder

✓

Few-shot

✓

Structured Output

✓

Guardrails

✓

Prompt Registry

✓

---

## Deliverable

A working

Prompt Engineering Pipeline.

---

# Bonus Lab — Prompt Optimization

Objective

Reduce

token usage

without

reducing

answer quality.

Measure

```
Prompt Length

↓

Tokens

↓

Latency

↓

Cost
```

---

Expected

Lower cost

with

same quality.

---

# Engineering Notebook

Every lab

should include

```
Problem

↓

Hypothesis

↓

Experiment

↓

Metrics

↓

Observation

↓

Conclusion

↓

Next Improvement
```

This notebook

becomes

your engineering journal.

---

# Evaluation Rubric

| Category | Points |
|-----------|-------:|
| Prompt Design | 15 |
| Context Engineering | 15 |
| Prompt Architecture | 15 |
| JSON Validation | 10 |
| Guardrails | 15 |
| Documentation | 10 |
| Engineering Notebook | 10 |
| Reflection | 10 |

Total

100

---

# Chapter Completion Checklist

By the end

of Chapter 2

you should be able to

✅ Build Prompt Templates

✅ Build Prompt Builder

✅ Build Context Builder

✅ Implement Few-shot Learning

✅ Return Structured Outputs

✅ Design Prompt Registry

✅ Implement Prompt Guardrails

✅ Design PromptOps Pipeline

✅ Build Prompt Architecture

---

# Capstone Challenge

Build

Version 2

of the

Invoice Explainability Agent.

Requirements

```
Prompt Templates

+

Context Builder

+

Few-shot

+

Structured Outputs

+

Prompt Registry

+

Guardrails

+

Evaluation
```

Do **not**

use

LangGraph

yet.

The objective is to master Prompt Engineering before introducing agent orchestration.

---

# Engineering Reflection

After completing these labs,

answer:

1. What part of Prompt Engineering was most difficult?
2. Which guardrail prevented the most errors?
3. Which prompt pattern produced the best results?
4. Did more context always improve quality?
5. How would you reduce cost without reducing quality?

Document your answers.

They become the baseline for Chapter 3.

---

# Further Challenge

Before moving to Chapter 3,

refactor your solution so that

```
Prompt Builder

↓

Context Builder

↓

Prompt Registry

↓

LLM

↓

Validator
```

are independent modules.

You are now building

software,

not prompts.
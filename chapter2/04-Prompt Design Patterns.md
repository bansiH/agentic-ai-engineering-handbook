# Prompt Design Patterns

> **Chapter 2 – Prompt & Context Engineering**

> *Design reusable prompts like reusable software.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain what Prompt Design Patterns are.
- Select the appropriate prompt pattern for a business problem.
- Understand the strengths and weaknesses of common prompting techniques.
- Build reusable prompt templates.
- Apply prompt patterns to enterprise AI systems.
- Avoid common prompt anti-patterns.

---

# Why Prompt Design Patterns Exist

Software engineering has

- Design Patterns
- Architecture Patterns
- API Patterns

LLM applications need

Prompt Patterns.

A Prompt Pattern is

> **A reusable solution to a recurring prompting problem.**

Instead of reinventing prompts,

we reuse proven structures.

---

# First Principle

Good Prompt Engineering is

not

writing beautiful prompts.

It is

selecting

the correct pattern

for the current task.

---

# Pattern Selection

```text
Business Problem

↓

Prompt Pattern

↓

Prompt Template

↓

Context

↓

LLM

↓

Structured Output
```

The prompt pattern is an architectural decision.

---

# Pattern 1 – Role Prompting

## Problem

The model needs domain-specific behavior.

---

## Pattern

Assign the model a role.

Example

```text
You are an expert financial auditor.

Explain this invoice.
```

---

## Architecture

```text
Role

↓

Context

↓

Question

↓

LLM
```

---

## Benefits

- Better consistency
- Domain vocabulary
- Improved explanations

---

## Limitations

A role does not provide knowledge.

It only influences behavior.

The required knowledge must still come from context.

---

## Enterprise Example

Invoice Explainability Agent

```
You are an enterprise invoice analyst.
```

instead of

```
You are a helpful assistant.
```

---

## Production Recommendation

Prefer explicit domain roles.

---

# Pattern 2 – Instruction Prompting

## Problem

The model needs precise instructions.

---

## Pattern

State exactly what should happen.

Example

```text
Summarize the invoice.

Return JSON.

Do not invent values.
```

---

## Architecture

```text
Instructions

↓

LLM

↓

Controlled Behavior
```

---

## Benefits

- Better predictability
- Easier evaluation

---

## Common Mistake

Vague instructions.

---

# Pattern 3 – Constraint Prompting

## Problem

The model should not exceed boundaries.

---

## Pattern

Explicitly define constraints.

Example

```text
Only answer using the supplied context.

If information is missing,

say

"I don't know."
```

---

## Enterprise Benefit

Reduces hallucinations.

---

# Pattern 4 – Persona Prompting

Role

defines

expertise.

Persona

defines

communication style.

Example

```text
Explain the invoice

to a first-year engineering student.
```

or

```text
Explain the invoice

to the CFO.
```

Same facts.

Different communication.

---

# Pattern 5 – Delimiter Pattern

Separate

instructions

from

data.

Example

```text
System Instructions

====

Invoice

====

Question
```

Delimiters reduce ambiguity.

---

# Pattern 6 – XML / JSON Prompting

Complex prompts become easier to parse.

Example

```xml
<invoice>

...

</invoice>
```

or

```json
{
  "invoice": {...}
}
```

Useful for structured enterprise workflows.

---

# Pattern 7 – Step-by-Step Reasoning (Conceptual)

Some tasks benefit from decomposing work into smaller reasoning steps.

For example:

```text
1. Read the invoice.
2. Identify pricing components.
3. Identify taxes.
4. Explain differences.
```

In production, prefer structured workflows over relying on hidden reasoning.

---

# Pattern 8 – ReAct Pattern

The model alternates between

reasoning

and

actions.

Architecture

```text
Question

↓

Reason

↓

Tool

↓

Observation

↓

Reason

↓

Answer
```

This pattern is common in modern AI agents.

---

# Pattern 9 – Planning Pattern

Useful for

large

multi-step tasks.

Example

```
Create a plan

before

executing.
```

Typical workflow

```text
Goal

↓

Plan

↓

Execute

↓

Review
```

---

# Pattern 10 – Reflection Pattern

After producing an answer,

the model reviews it.

```text
Answer

↓

Review

↓

Improve

↓

Final Answer
```

Useful for

- reports
- documentation
- code review

---

# Pattern 11 – Structured Output Pattern

Instead of

free text

return

JSON.

Example

```json
{
  "invoice_total": 12540,
  "tax": 1912,
  "reason": "Fuel surcharge"
}
```

Enterprise systems prefer structured outputs.

---

# Pattern 12 – Retrieval Pattern

The model first retrieves information.

Architecture

```text
Question

↓

Retriever

↓

Context

↓

LLM
```

The prompt no longer relies only on the model's internal knowledge.

---

# Pattern Selection Matrix

| Pattern | Best For | Avoid When |
|----------|----------|------------|
| Role | Domain expertise | Knowledge is missing |
| Instruction | Deterministic tasks | Instructions are ambiguous |
| Constraint | Hallucination reduction | Constraints conflict |
| Persona | Audience adaptation | Domain expertise is required |
| Delimiters | Complex prompts | Very small prompts |
| JSON/XML | APIs and automation | Human-only output |
| ReAct | Tool use | No tools exist |
| Planning | Long workflows | Simple questions |
| Reflection | Quality improvement | Ultra-low latency |
| Retrieval | Enterprise knowledge | No external data exists |

---

# Running Case Study

Invoice Explainability Agent

Final prompt architecture

```text
Role

↓

Instructions

↓

Constraints

↓

Retrieved Context

↓

Invoice

↓

Question

↓

LLM

↓

JSON Output
```

Notice

multiple patterns

working together.

---

# Engineering Perspective

Production prompts rarely use

one

pattern.

Instead

they combine

multiple patterns

into

a Prompt Architecture.

---

# Common Misconceptions

## "One perfect prompt exists."

False.

Choose patterns based on the task.

---

## "Long prompts are always better."

False.

Clear structure beats unnecessary length.

---

## "Role Prompting provides knowledge."

False.

Knowledge comes from retrieved context or tools.

---

## "Prompt patterns replace software engineering."

False.

Prompt patterns complement application architecture.

---

# Best Practices

✅ Separate instructions from context.

✅ Prefer reusable templates.

✅ Keep prompts version controlled.

✅ Test pattern combinations.

✅ Measure prompt performance.

---

# Engineering Decision Record (EDR)

## Problem

Need reliable invoice explanations.

## Options

1. One large prompt.

2. Layered prompt architecture.

3. Prompt patterns + context engineering.

## Decision

Combine multiple prompt patterns with retrieved context.

## Trade-offs

Pros

- Better maintainability
- Better reuse
- Easier testing
- Lower hallucination risk

Cons

- More engineering effort
- Requires Prompt Builder

## Recommendation

Treat Prompt Patterns as reusable engineering components.

---

# Key Takeaways

- Prompt Design Patterns solve recurring prompting problems.
- Patterns are reusable engineering solutions.
- Most enterprise prompts combine multiple patterns.
- Prompt patterns improve maintainability and consistency.
- Pattern selection depends on workload, not personal preference.

---

# Interview Questions

### Q1

What is a Prompt Design Pattern?

---

### Q2

When would you use Role Prompting?

---

### Q3

Role vs Persona?

---

### Q4

Why use Constraint Prompting?

---

### Q5

When should you return JSON?

---

### Q6

Why are delimiters useful?

---

### Q7

Explain ReAct.

---

### Q8

Why do enterprise systems combine multiple prompt patterns?

---

# Hands-on Exercise

## Objective

Design a production prompt using multiple patterns.

### Requirements

Use:

- Role
- Instructions
- Constraints
- Retrieved Context
- JSON Output

### Build

Create an Invoice Explainability Prompt.

### Evaluate

Measure:

- consistency
- groundedness
- JSON validity
- hallucination rate

### Expected Outcome

A layered prompt should produce more reliable and maintainable outputs than a single free-form prompt.

---

# Further Reading

- LangChain Prompt Templates
- LangGraph Documentation
- OpenAI Prompt Engineering Guide
- Anthropic Prompt Engineering Guide
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI architecture
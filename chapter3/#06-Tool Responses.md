# Tool Responses

> **Chapter 3 – Tools & Function Calling**

> *Observations are the fuel that powers Agent reasoning.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain why Tool Responses matter.
- Understand Observations.
- Design structured Tool Responses.
- Differentiate Tool Responses from LLM Responses.
- Handle failures and partial responses.
- Build observation-driven AI Agents.
- Design enterprise Tool Response contracts.

---

# Why Tool Responses Matter

Suppose an agent executes

```
get_invoice()
```

What happens next?

Many beginners think

```
Tool

↓

Answer
```

Actually

the Tool returns

an **Observation**.

The LLM reasons

over

that observation.

---

# First Principle

A Tool does **not**

answer

the user.

A Tool

returns

information.

The LLM

interprets

that information.

---

# Mental Model

Imagine a detective.

The detective

does not

solve a case

by reading one clue.

Instead

```
Observation

↓

Reasoning

↓

Another Observation

↓

Reasoning

↓

Conclusion
```

AI Agents work exactly the same way.

---

# Observation

An Observation is

> **Structured information returned by a tool after execution.**

Examples

```
Invoice JSON

SQL Result

Weather

Customer Record

Payment Status
```

The observation

becomes

new context

for the next reasoning step.

---

# Tool Response Lifecycle

```
Tool

↓

Execution

↓

Observation

↓

Agent State

↓

LLM

↓

Reasoning

↓

Answer
```

Notice

the observation

updates

the agent's state.

---

# Structured Responses

Bad

```
Everything worked.
```

Good

```json
{
  "status":"SUCCESS",
  "invoice_total":12540,
  "currency":"INR",
  "tax":1912
}
```

Structured responses are easier to validate,

reuse,

and audit.

---

# Response Anatomy

Every production Tool Response should contain

```
Status

↓

Data

↓

Metadata

↓

Errors

↓

Timing
```

Think of the response as a software contract.

---

# Success Response

Example

```json
{
  "status":"SUCCESS",
  "invoice_id":"INV-123",
  "invoice_total":12540,
  "currency":"INR",
  "retrieved_at":"2026-08-18T10:00:00Z"
}
```

The LLM now reasons over facts,

not assumptions.

---

# Failure Response

Never return

raw exceptions.

Instead

```json
{
  "status":"FAILED",
  "reason":"INVOICE_NOT_FOUND"
}
```

This allows the LLM to explain the situation without exposing internal errors.

---

# Partial Response

Sometimes

only part of the request succeeds.

Example

```
Invoice

✓

Tax

✓

Discount

✗
```

Structured response

```json
{
  "status":"PARTIAL_SUCCESS",
  "invoice_total":12540,
  "discount":"UNAVAILABLE"
}
```

The agent can continue reasoning while acknowledging incomplete information.

---

# Metadata

Useful metadata includes

- timestamp
- source
- latency
- confidence
- version
- request ID

Example

```json
{
  "source":"InvoiceAPI",
  "latency_ms":180,
  "version":"v2"
}
```

Metadata supports debugging and observability.

---

# Evidence and Citations

Enterprise AI systems often need to explain

**where information came from**.

Example

```json
{
  "reason":"Fuel surcharge",
  "evidence":[
    "Pricing Policy v3.2",
    "Invoice Line Item 7"
  ]
}
```

This improves transparency and user trust.

---

# Agent State

Every observation updates the agent's state.

```
Question

↓

Observation

↓

Updated State

↓

Reasoning

↓

Next Tool?
```

The agent no longer reasons only from the original user question.

It reasons from accumulated observations.

---

# Multi-Tool Example

Question

```
Why is my invoice higher?
```

Pipeline

```
Invoice Tool

↓

Observation

↓

Pricing Tool

↓

Observation

↓

Tax Tool

↓

Observation

↓

LLM

↓

Explanation
```

Each observation enriches the agent's understanding.

---

# Observation vs Response

Observation

↓

Internal

Response

↓

External

Example

```
Invoice JSON

↓

Observation

↓

LLM

↓

Customer-friendly explanation
```

The customer never sees the raw observation.

---

# Running Case Study

Invoice Explainability Agent

```
get_invoice()

↓

Invoice JSON

↓

Agent State

↓

get_pricing_rules()

↓

Pricing JSON

↓

Agent State

↓

LLM

↓

Grounded Explanation
```

This pattern repeats throughout the handbook.

---

# Engineering Perspective

Observations are

temporary reasoning artifacts.

Databases remain

the system of record.

The LLM reasons

over observations,

not databases directly.

---

# Production Insight

Enterprise response pipeline

```
Tool

↓

Response Validator

↓

Observation Builder

↓

Agent State

↓

LLM
```

The Observation Builder converts raw API results into consistent reasoning inputs.

---

# Confidence

Some tools return confidence scores.

Example

```json
{
  "invoice_total":12540,
  "confidence":0.99
}
```

Confidence can help downstream workflows,

but should not replace validation or human judgement.

---

# Provenance

Every observation should have provenance.

Questions to answer:

- Where did the data come from?
- When was it retrieved?
- Which service returned it?
- Which version was used?

This supports auditability and debugging.

---

# Engineering Notebook

Experiment

Create

three observations.

1. Success

2. Failure

3. Partial Success

Ask the LLM to explain each.

Observe

whether the model responds differently based on structured status fields.

Conclusion

Consistent observation formats improve downstream reasoning.

---

# Common Misconceptions

## "The tool answers the user."

False.

The tool returns an observation.

The LLM answers the user.

---

## "Raw API responses should be sent directly to the LLM."

Not always.

Applications often normalize and enrich responses before passing them to the model.

---

## "Failures should be hidden."

No.

Failures should be represented safely using structured status codes.

---

## "Metadata is optional."

Metadata is extremely valuable for monitoring, debugging and governance.

---

# Best Practices

✅ Return structured responses.

✅ Include status fields.

✅ Preserve provenance.

✅ Include timestamps.

✅ Normalize responses across tools.

✅ Distinguish observations from user-facing responses.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Success | Structured JSON | Reliable reasoning |
| Failure | Structured error | Safe recovery |
| Partial data | Partial Success contract | Continue reasoning |
| Enterprise APIs | Include provenance | Auditability |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable tool observations.

## Options

1. Raw API response.

2. Normalized Observation.

3. Observation + Metadata + Provenance.

## Decision

Observation with metadata and provenance.

## Trade-offs

Pros

- Easier reasoning
- Better debugging
- Better auditing
- Consistent downstream processing

Cons

- Additional transformation layer

## Recommendation

Treat observations as first-class engineering artifacts.

---

# Key Takeaways

- Tools return observations, not final answers.
- Observations update the agent's state.
- Structured observations improve reasoning.
- Metadata and provenance improve trust and observability.
- Normalize responses before passing them to the LLM.

---

# Interview Questions

### Q1

What is an Observation?

---

### Q2

Why shouldn't raw API responses always be sent directly to the model?

---

### Q3

Observation vs Response?

---

### Q4

What metadata belongs in Tool Responses?

---

### Q5

Why is provenance important?

---

### Q6

How should failures be represented?

---

### Q7

How do observations update agent state?

---

### Q8

Draw a production Tool Response pipeline.

---

# Hands-on Exercise

## Objective

Design a production Tool Response contract.

### Requirements

Create three response types:

- Success
- Failure
- Partial Success

Include:

- status
- data
- metadata
- provenance
- timestamp

### Deliverable

Implement a mock Observation Builder that converts raw API responses into standardized observations.

### Expected Outcome

The agent should reason consistently regardless of which tool produced the observation because every response follows the same contract.

---

# Production Readiness Checklist

☑ Structured response schema

☑ Success and failure contracts

☑ Partial success support

☑ Metadata included

☑ Provenance recorded

☑ Response normalization

☑ Observation builder

☑ Audit logging

☑ Monitoring metrics

---

# Further Reading

- LangGraph Documentation (State and Messages)
- LangChain Tools Documentation
- OpenAPI Specification
- JSON Schema Specification
- OpenTelemetry (Observability concepts)
- Microsoft Agent Framework Documentation
- ByteByteGo articles on distributed systems and AI architecture
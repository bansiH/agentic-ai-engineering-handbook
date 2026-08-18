# Tool Anatomy

> **Chapter 3 – Tools & Function Calling**

> *Designing reliable software interfaces for AI Agents*

---

# Learning Objectives

After completing this section you should be able to:

- Explain what a Tool is.
- Understand Tool Schemas.
- Understand Tool Contracts.
- Explain Tool Metadata.
- Understand Tool Inputs and Outputs.
- Build production-ready tools.
- Design enterprise Tool APIs.

---

# Why Tool Anatomy Matters

Most developers think a tool is simply

```
Python Function
```

That is only the implementation.

An AI Agent never reasons over Python.

It reasons over

- Tool Description
- Tool Schema
- Tool Parameters
- Tool Result

Understanding these pieces is critical for building reliable agents.

---

# First Principle

A Tool is

> **A well-defined software capability that exposes a stable contract to an AI system.**

Notice

Tool

≠

Function

A function is one implementation.

A tool is an interface.

---

# Engineering Mental Model

Think of a REST API.

You don't need to know

how

Stripe

implements

```
POST /payments
```

You only need

the contract.

Exactly the same applies to AI tools.

---

# Tool Anatomy

Every tool consists of

```
Name

↓

Description

↓

Input Schema

↓

Validation

↓

Execution

↓

Output Schema

↓

Metadata
```

Every component serves a purpose.

---

# Component 1

## Tool Name

The name uniquely identifies the capability.

Example

```
get_invoice

calculate_tax

find_driver

book_ride
```

Guidelines

- short
- descriptive
- action-oriented
- stable

Bad

```
tool1
```

Good

```
get_invoice
```

---

# Component 2

## Tool Description

This is one of the most important fields.

The LLM often decides

whether to use a tool

based primarily on

its description.

Example

```
Retrieve an invoice by invoice ID.

Returns

customer

pricing

tax

discounts

currency.
```

A poor description leads to poor tool selection.

---

# Component 3

## Input Schema

Every tool accepts structured inputs.

Example

```json
{
  "invoice_id": "INV-123"
}
```

The schema defines

- required fields
- optional fields
- types
- constraints

The schema is

the contract.

---

# Engineering Analogy

Think of a database schema.

Without one,

applications cannot reliably exchange information.

Tool Schemas serve the same purpose.

---

# Component 4

## Validation

Never trust model-generated arguments.

Validate:

- required fields
- data types
- ranges
- formats

Example

```
Invoice ID missing

↓

Reject
```

Example

```
Amount < 0

↓

Reject
```

Validation belongs in deterministic application logic.

---

# Component 5

## Execution

Only after validation

should the application execute the tool.

Architecture

```
LLM

↓

Tool Request

↓

Validator

↓

Execution
```

The LLM proposes.

The application executes.

---

# Component 6

## Output Schema

Tool outputs should also be structured.

Example

```json
{
  "invoice_total": 12540,
  "currency": "INR",
  "tax": 1912,
  "discount": 300
}
```

Structured outputs are easier to validate and reuse.

---

# Component 7

## Metadata

Useful metadata includes

- Tool version
- Owner
- Timeout
- Permissions
- Cost
- Latency
- Tags

Example

```yaml
owner: Finance Team
version: 2.1
timeout: 5s
permission: invoice.read
```

Metadata supports governance and operations.

---

# Complete Tool Architecture

```
User

↓

Agent

↓

Tool Name

↓

Description

↓

Input Schema

↓

Validation

↓

Execution

↓

Output Schema

↓

Observation

↓

LLM

↓

Response
```

---

# Running Case Study

Invoice Explainability Agent

Tool

```
get_invoice
```

Description

```
Retrieve invoice details by invoice ID.
```

Input

```json
{
  "invoice_id": "INV-001"
}
```

Output

```json
{
  "invoice_total": 12540,
  "currency": "INR",
  "pricing_rule": "Fuel surcharge"
}
```

The LLM reasons over this observation rather than inventing invoice data.

---

# Tool Registry

Enterprise systems usually maintain a centralized Tool Registry.

```
Tool Registry

↓

Tool Name

↓

Schema

↓

Description

↓

Version

↓

Permissions
```

Benefits

- discoverability
- governance
- version control
- auditing

---

# Versioning

Tools evolve.

Example

```
get_invoice

v1

↓

v2

↓

v3
```

Backward compatibility should be considered when changing schemas.

---

# Permissions

Not every user should access every tool.

Example

```
Invoice Tool

↓

invoice.read
```

```
Refund Tool

↓

payment.refund
```

Authorization belongs outside the LLM.

---

# Error Handling

Typical failures

```
Tool Timeout

↓

Retry
```

```
Invalid Arguments

↓

Validation Error
```

```
Permission Denied

↓

Safe Response
```

```
Service Unavailable

↓

Fallback
```

The LLM should receive a structured error, not an application crash.

---

# Engineering Perspective

Treat tools like production APIs.

That means

- versioning
- validation
- monitoring
- testing
- documentation

---

# Production Insight

A production tool pipeline

```
LLM

↓

Tool Router

↓

Schema Validation

↓

Authorization

↓

Execution

↓

Monitoring

↓

Observation
```

Notice

Execution happens only after multiple deterministic checks.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| Missing parameter | Schema validation |
| Wrong type | Type validation |
| Unauthorized request | Authorization |
| Timeout | Retry with limits |
| Service unavailable | Fallback / graceful error |
| Invalid response | Output validation |

---

# Common Misconceptions

## "A tool is just a Python function."

False.

A production tool is a contract plus an implementation.

---

## "The LLM validates tool inputs."

False.

Validation belongs to deterministic software.

---

## "Tool descriptions don't matter."

False.

Descriptions strongly influence tool selection.

---

## "JSON output is optional."

Structured outputs are strongly recommended because they simplify downstream processing.

---

# Best Practices

✅ Give every tool a clear description.

✅ Define input and output schemas.

✅ Validate before execution.

✅ Version tool contracts.

✅ Log every invocation.

✅ Keep tools deterministic.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Internal API | JSON Schema | Reliable contract |
| Financial Tool | Strong validation | Safety |
| Public API | Versioning | Backward compatibility |
| High-risk tool | Authorization + Audit | Governance |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable invoice retrieval.

## Options

1. Raw Python function.

2. Tool with schema.

3. Versioned Tool Registry.

## Decision

Versioned Tool Registry with schema validation.

## Trade-offs

Pros

- Better discoverability
- Strong validation
- Easier governance
- Safer evolution

Cons

- Additional engineering effort
- Registry maintenance

## Recommendation

Treat tools as production APIs, not helper functions.

---

# Key Takeaways

- A tool is a software contract.
- Schemas define inputs and outputs.
- Validation is mandatory.
- Tool descriptions influence LLM behavior.
- Tool registries improve maintainability.
- Enterprise tools require governance.

---

# Interview Questions

### Q1

What is a Tool?

---

### Q2

Why is a Tool different from a Python function?

---

### Q3

Why are Tool Schemas important?

---

### Q4

What belongs in Tool Metadata?

---

### Q5

Why validate tool inputs?

---

### Q6

Why should tool outputs be structured?

---

### Q7

What is a Tool Registry?

---

### Q8

How would you version enterprise tools?

---

# Hands-on Exercise

## Objective

Design a production-ready Invoice Tool.

### Requirements

- Tool name
- Description
- Input schema
- Output schema
- Validation rules
- Version
- Permissions

### Deliverable

Document the tool contract and implement a mock version.

### Expected Outcome

You should produce a reusable, versioned tool interface that can be consumed by an AI agent.

---

# Production Readiness Checklist

☑ Tool name defined

☑ Description documented

☑ Input schema created

☑ Output schema created

☑ Validation implemented

☑ Authorization enforced

☑ Version assigned

☑ Monitoring configured

☑ Audit logging enabled

---

# Further Reading

- OpenAPI Specification
- JSON Schema Specification
- OpenAI Function Calling Documentation
- Anthropic Tool Use Documentation
- Model Context Protocol (MCP) Specification
- LangChain Tools Documentation
- LangGraph Documentation
- Microsoft Agent Framework Documentation
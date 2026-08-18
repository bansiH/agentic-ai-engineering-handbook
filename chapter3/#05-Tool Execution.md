# Tool Execution

> **Chapter 3 – Tools & Function Calling**

> *Reliable execution is more important than successful execution.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain the Tool Execution lifecycle.
- Understand retries.
- Understand idempotency.
- Understand timeouts.
- Understand circuit breakers.
- Understand execution policies.
- Build reliable enterprise Tool Execution pipelines.

---

# Why Tool Execution Matters

Suppose an AI Agent decides

```
refund_payment()
```

Should the application immediately execute it?

Absolutely not.

Before execution we must answer

- Is the request valid?
- Is the user authorized?
- Is the service available?
- Has this already been executed?
- Can it be safely retried?

Production Tool Execution is therefore an engineering discipline.

---

# First Principle

Function Calling decides

**what**

should happen.

Tool Execution decides

**whether**

it is safe

to perform that action.

---

# Execution Pipeline

```
LLM

↓

Function Request

↓

Schema Validation

↓

Authentication

↓

Authorization

↓

Execution Policy

↓

Tool Execution

↓

Observation

↓

LLM
```

Notice

many deterministic systems execute

before

the tool runs.

---

# Engineering Mental Model

Imagine an airport.

Passenger

↓

Identity Check

↓

Security

↓

Boarding Pass

↓

Gate

↓

Aircraft

Only after passing every checkpoint

can the passenger board.

Tool Execution follows the same philosophy.

---

# Step 1

## Schema Validation

Before execution

validate

```
Required Fields

↓

Correct Types

↓

Allowed Values

↓

Business Constraints
```

Example

```json
{
    "invoice_id": ""
}
```

↓

Reject

Missing ID.

---

# Step 2

## Authentication

Question

Who is making this request?

Example

```
Employee

↓

Authenticated
```

Without authentication

execution should stop.

---

# Step 3

## Authorization

Question

Is the authenticated user

allowed

to perform

this action?

Example

```
Refund

↓

Finance Manager

↓

Allowed
```

Customer

↓

Denied

Authorization

belongs

outside

the LLM.

---

# Step 4

## Execution Policy

Every tool should define

execution rules.

Example

```
Refund

↓

Maximum ₹50,000

↓

Human Approval Required
```

Policies protect business operations.

---

# Step 5

## Tool Execution

Only now

does

the application

call

the tool.

Example

```
REST API

↓

ERP

↓

CRM

↓

Database

↓

Response
```

---

# Step 6

## Observation

The tool returns

structured data.

Example

```json
{
  "status":"SUCCESS",
  "invoice_total":12540
}
```

This becomes

new context

for the LLM.

---

# Execution States

```
Queued

↓

Running

↓

Completed
```

Possible failure states

```
Timeout

↓

Retry
```

```
Permission Denied

↓

Reject
```

```
Unavailable

↓

Fallback
```

---

# Idempotency

Imagine

```
refund_payment()
```

Network fails.

Application retries.

Without idempotency

```
Refund

↓

Refund Again

↓

Double Refund
```

Disaster.

Idempotency ensures

the same request

produces

the same result

without unintended duplicate effects.

---

# Idempotency Keys

Typical pattern

```
Request

↓

Idempotency Key

↓

Execution

↓

Store Result
```

If the same key arrives again,

return the stored result instead of executing the action again.

---

# Retries

Some failures are temporary.

Examples

- network timeout
- service unavailable
- transient database error

Retry policy

```
Failure

↓

Retry

↓

Retry

↓

Retry

↓

Fail
```

Retries should be limited and controlled.

---

# Exponential Backoff

Do not retry immediately.

Instead

```
1 second

↓

2 seconds

↓

4 seconds

↓

8 seconds
```

This reduces pressure on recovering services.

---

# Timeouts

Every tool should have a maximum execution time.

Example

```
Invoice API

↓

Timeout

↓

5 seconds
```

After the timeout

return a safe error or fallback.

Never wait forever.

---

# Circuit Breaker

Suppose a downstream service is failing.

Without protection

```
Request

↓

Failure

↓

Request

↓

Failure

↓

Request

↓

Failure
```

Circuit Breaker

```
Repeated Failure

↓

Open Circuit

↓

Reject Requests

↓

Recovery Check

↓

Close Circuit
```

This prevents cascading failures.

---

# Bulkheads

Do not allow one failing tool

to take down the entire agent.

Example

```
Weather API

↓

Failure
```

Invoice Tool

↓

Still Works

Isolation improves resilience.

---

# Running Case Study

Invoice Explainability Agent

Execution

```
LLM

↓

get_invoice()

↓

Schema Validation

↓

Authorization

↓

Invoice API

↓

Observation

↓

LLM

↓

Explanation
```

Every stage is observable and testable.

---

# Error Categories

| Error | Typical Response |
|--------|------------------|
| Invalid Input | Reject |
| Unauthorized | Reject |
| Timeout | Retry |
| Service Unavailable | Retry or Fallback |
| Permanent Failure | Escalate |
| Unknown | Safe Error |

---

# Engineering Perspective

Never expose

raw exceptions

to the LLM.

Instead

return structured observations.

Example

```json
{
  "status":"FAILED",
  "reason":"SERVICE_TIMEOUT"
}
```

The model can explain the situation without inventing technical details.

---

# Production Insight

Enterprise execution pipeline

```
LLM

↓

Tool Router

↓

Schema Validation

↓

Authentication

↓

Authorization

↓

Execution Policy

↓

Retry Manager

↓

Circuit Breaker

↓

API

↓

Observation

↓

LLM
```

Every box is deterministic.

---

# Engineering Notebook

Experiment.

Simulate

```
Invoice API

↓

Timeout
```

Question

Should the LLM retry?

Correct answer

No.

The application manages retries.

The LLM receives only the final observation.

---

# Common Misconceptions

## "The LLM executes tools."

False.

Applications execute tools.

---

## "Retries belong inside prompts."

False.

Retries belong in application infrastructure.

---

## "Every failure should be retried."

False.

Validation failures should not be retried.

Transient failures may be.

---

## "Tool execution is an AI problem."

False.

It is primarily a distributed systems problem.

---

# Best Practices

✅ Validate before execution.

✅ Authenticate every request.

✅ Authorize every action.

✅ Use idempotency keys.

✅ Configure retries.

✅ Configure timeouts.

✅ Use circuit breakers.

✅ Log every execution.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Financial transactions | Idempotency | Prevent duplicates |
| External APIs | Timeouts + Retries | Reliability |
| Frequently failing services | Circuit Breaker | Stability |
| High-risk actions | Human Approval | Governance |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable invoice retrieval.

## Options

1. Execute immediately.

2. Validate before execution.

3. Full execution pipeline.

## Decision

Full execution pipeline.

## Trade-offs

Pros

- Safer
- More reliable
- Easier auditing
- Better resilience

Cons

- Additional infrastructure
- Slightly higher latency

## Recommendation

Treat Tool Execution as a distributed systems workflow, not simply a function invocation.

---

# Key Takeaways

- Tool Execution is a deterministic application workflow.
- Validation precedes execution.
- Authentication and authorization are mandatory.
- Idempotency prevents duplicate side effects.
- Retries, timeouts and circuit breakers improve reliability.
- The LLM reasons over observations, not raw exceptions.

---

# Interview Questions

### Q1

What is Tool Execution?

---

### Q2

Why is validation required before execution?

---

### Q3

What is idempotency?

---

### Q4

Why use exponential backoff?

---

### Q5

What problem does a Circuit Breaker solve?

---

### Q6

Should the LLM perform retries?

---

### Q7

How should execution failures be represented?

---

### Q8

Draw a production Tool Execution pipeline.

---

# Hands-on Exercise

## Objective

Build a reliable execution pipeline.

### Requirements

- Schema validation
- Authentication
- Authorization
- Timeout
- Retry policy
- Circuit breaker (conceptual or library-based)
- Structured observation

### Deliverable

Implement a mock `get_invoice()` execution flow with simulated success and failure cases.

### Expected Outcome

The application—not the LLM—should manage operational concerns while returning structured observations for the model to reason over.

---

# Production Readiness Checklist

☑ Schema validation

☑ Authentication

☑ Authorization

☑ Execution policy

☑ Idempotency

☑ Timeout

☑ Retry policy

☑ Circuit breaker

☑ Structured observations

☑ Decision logs

☑ Monitoring

---

# Further Reading

- Martin Fowler – Circuit Breaker Pattern
- Retry and Backoff patterns
- OpenAPI Specification
- LangGraph Documentation
- LangChain Documentation
- Microsoft Agent Framework Documentation
- OpenAI Function Calling Documentation
- ByteByteGo articles on distributed systems and AI architecture
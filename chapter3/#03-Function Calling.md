# Function Calling

> **Chapter 3 – Tools & Function Calling**

> *The bridge between natural language reasoning and software execution.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain Function Calling from first principles.
- Understand the complete Function Calling lifecycle.
- Understand why JSON Schema is required.
- Explain argument generation.
- Understand execution flow.
- Explain retries and validation.
- Design production-safe Function Calling pipelines.
- Explain Function Calling during interviews.

---

# Why Function Calling Exists

Suppose the user asks

```
What is my invoice total?
```

The model knows

how invoices work.

But it does not know

your invoice.

Instead of guessing,

the model should say

> "I need to retrieve the invoice."

This transition

from

language generation

to

software execution

is Function Calling.

---

# First Principle

Function Calling does **not**

mean

the LLM executes code.

Instead,

the LLM proposes

a structured request.

The application decides

whether

to execute it.

---

# Engineering Mental Model

Think of a junior engineer.

Instead of directly changing production,

they submit

a request.

```
Engineer

↓

Change Request

↓

Approval

↓

Execution
```

Function Calling works similarly.

```
LLM

↓

Function Request

↓

Application

↓

Execution
```

---

# The Function Calling Lifecycle

The complete lifecycle looks like this.

```
User Question

↓

LLM

↓

Should a Function be Called?

↓

Generate Function Name

↓

Generate Arguments

↓

Schema Validation

↓

Authorization

↓

Execute Function

↓

Observation

↓

LLM

↓

Final Response
```

Notice

there are

two reasoning phases.

Before

and

after

tool execution.

---

# Step 1

## User Question

Example

```
Why is my invoice ₹12,540?
```

The model first determines

whether

it has enough information.

---

# Step 2

## Function Decision

The model asks

```
Can I answer?

OR

Do I need a tool?
```

Possible outcomes

```
General Knowledge

↓

No Tool
```

```
Enterprise Data

↓

Use Tool
```

---

# Step 3

## Function Selection

Suppose several tools exist.

```
get_invoice()

get_customer()

calculate_tax()

find_driver()
```

Which one?

The model decides

using

- Tool Name
- Tool Description
- Tool Schema
- User Question

---

# Step 4

## Argument Generation

Suppose

the chosen function is

```
get_invoice()
```

The model generates

arguments.

Example

```json
{
  "invoice_id": "INV-001"
}
```

Notice

The model is

not

executing anything.

It is proposing

parameters.

---

# Step 5

## Schema Validation

Never trust

generated arguments.

Validate

```
Required Fields

↓

Types

↓

Ranges

↓

Formats
```

Example

```
invoice_id missing

↓

Reject
```

Validation belongs

to deterministic software.

---

# Step 6

## Authorization

The model may request

```
get_customer_balance()
```

Question

Is the user

allowed

to see it?

Architecture

```
Function Request

↓

Authorization

↓

Allowed?

↓

YES

↓

Execute

NO

↓

Reject
```

Authorization

never belongs

inside the LLM.

---

# Step 7

## Function Execution

Now

and only now

does

the application

execute.

```
Application

↓

REST API

↓

Database

↓

ERP

↓

CRM

↓

Observation
```

---

# Step 8

## Observation

Example

```json
{
  "invoice_total":12540,
  "tax":1912,
  "discount":300
}
```

This becomes

new context

for the LLM.

---

# Step 9

## Final Reasoning

The LLM now reasons

over

real data.

```
Observation

↓

Reasoning

↓

Explanation
```

This dramatically reduces hallucinations.

---

# Complete Architecture

```
User

↓

LLM

↓

Function Request

↓

Schema Validator

↓

Authorization

↓

Execution

↓

Observation

↓

LLM

↓

Final Answer
```

This architecture appears in almost every enterprise AI system.

---

# JSON Schema

Functions require

contracts.

Example

```json
{
  "name":"get_invoice",
  "parameters":{
    "invoice_id":{
      "type":"string"
    }
  }
}
```

The schema allows

both

the LLM

and

the application

to understand the interface.

---

# Function Calling vs API Calling

Function Calling

↓

AI proposes

API Calling

↓

Application executes

The distinction is critical.

The LLM never

directly

calls production systems.

---

# Function Calling vs Tools

Function Calling

↓

Mechanism

Tools

↓

Capabilities

Example

```
Tool

↓

get_invoice
```

Function Calling

↓

Use

get_invoice()
```

Function Calling is

how

the tool is invoked.

---

# Function Calling vs MCP

Function Calling

↓

One interaction

between

model

and

application.

Model Context Protocol (MCP)

↓

Standardized framework

for exposing

multiple tools,

resources,

and prompts.

Think of Function Calling

as

one capability

inside

a larger interoperability ecosystem.

---

# Running Case Study

Invoice Explainability Agent

```
User

↓

get_invoice()

↓

Invoice API

↓

Invoice JSON

↓

LLM

↓

Explanation
```

No hallucination.

Real invoice.

---

# Engineering Perspective

The LLM should

never

execute

business logic.

Instead

```
LLM

↓

Intent

↓

Application

↓

Execution
```

The application remains

the system of control.

---

# Production Insight

Production pipeline

```
LLM

↓

Function Proposal

↓

Schema Validation

↓

Authorization

↓

Execution

↓

Retries

↓

Monitoring

↓

Observation

↓

LLM
```

Every stage

is observable.

---

# Failure Modes

## Invalid Arguments

↓

Reject

---

## Timeout

↓

Retry

---

## Permission Denied

↓

Safe Response

---

## Service Unavailable

↓

Fallback

---

## Invalid Response

↓

Schema Validation

↓

Repair

---

# Engineering Notebook

Experiment.

Prompt

```
Retrieve my invoice.
```

Observe

the model

does not answer immediately.

Instead

it proposes

```
get_invoice()
```

Question

Who executed

the function?

Correct answer

The application.

---

# Common Misconceptions

## "The LLM calls APIs."

False.

Applications call APIs.

The LLM proposes.

---

## "Function Calling is just JSON."

False.

JSON is the transport.

Function Calling is the workflow.

---

## "Validation belongs inside the LLM."

False.

Validation belongs in deterministic software.

---

## "Function Calling replaces Prompt Engineering."

False.

Prompt Engineering tells the model **when** and **how** to use functions.

---

# Best Practices

✅ Use JSON Schema.

✅ Validate arguments.

✅ Authorize every function.

✅ Log every invocation.

✅ Return structured observations.

✅ Keep functions deterministic.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Enterprise API | Function Calling | Structured invocation |
| Financial workflow | Function Calling + Policy Engine | Governance |
| Public search | Retrieval Tool | Read-only access |
| High-risk action | Function Calling + Human Approval | Safety |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable invoice retrieval.

## Options

1. Prompt only

2. Direct API from UI

3. Function Calling

## Decision

Function Calling with validation and authorization.

## Trade-offs

Pros

- Reliable
- Structured
- Auditable
- Governable

Cons

- Additional infrastructure
- Schema management
- Execution pipeline

## Recommendation

Treat Function Calling as the standard bridge between language models and enterprise software.

---

# Key Takeaways

- Function Calling bridges reasoning and execution.
- The LLM proposes; the application executes.
- JSON Schema defines the contract.
- Validation and authorization are mandatory.
- Function results become observations for further reasoning.
- Production systems should log every function invocation.

---

# Interview Questions

### Q1

What is Function Calling?

---

### Q2

Does the LLM execute the function?

---

### Q3

Why is JSON Schema important?

---

### Q4

Why should arguments be validated?

---

### Q5

Function Calling vs Tool?

---

### Q6

Function Calling vs API?

---

### Q7

Function Calling vs MCP?

---

### Q8

Draw the complete Function Calling lifecycle.

---

# Hands-on Exercise

## Objective

Implement your first Function Calling workflow.

### Step 1

Create

```
get_invoice()
```

### Step 2

Define

JSON Schema.

### Step 3

Validate arguments.

### Step 4

Execute.

### Step 5

Return

JSON.

### Step 6

Allow

the LLM

to explain

the invoice.

### Expected Outcome

The model should generate a structured function request, the application should execute the function after validation and authorization, and the final explanation should be based on the returned observation.

---

# Production Readiness Checklist

☑ Function Schema defined

☑ JSON Schema validated

☑ Authorization implemented

☑ Execution isolated

☑ Structured observations

☑ Retries configured

☑ Timeouts configured

☑ Audit logs enabled

☑ Metrics collected

---

# Further Reading

- OpenAI Function Calling Documentation
- Anthropic Tool Use Documentation
- LangChain Tools Documentation
- LangGraph Documentation
- Model Context Protocol (MCP) Specification
- JSON Schema Specification
- OpenAPI Specification
- Microsoft Agent Framework Documentation
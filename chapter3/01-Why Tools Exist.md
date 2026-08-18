# Why Tools Exist

> **Chapter 3 – Tools & Function Calling**

> *From Language Models to Software Systems*

---

# Learning Objectives

After completing this section you should be able to:

- Explain why LLMs require tools.
- Understand the difference between knowledge and access.
- Explain why enterprise AI systems rely on tools.
- Understand the role of tools in AI Agents.
- Explain the transition from Chatbots to AI Agents.

---

# The Engineering Question

Suppose a user asks

```
What is my invoice total from yesterday?
```

Can an LLM answer?

Usually,

No.

Not because the model lacks intelligence.

Because it lacks **access**.

This distinction is one of the most important concepts in Agent Engineering.

---

# First Principle

LLMs possess

```
Knowledge
```

They do **not** possess

```
Access
```

Knowledge comes from training.

Access comes from software.

---

# Mental Model

Imagine hiring a brilliant financial analyst.

They know

- accounting
- taxation
- finance

Ask them

```
What is my invoice?
```

They reply

```
I don't have access.
```

Give them access

to

```
Invoice Database
```

Now they can answer.

LLMs behave exactly the same way.

---

# Knowledge vs Access

| Knowledge | Access |
|------------|--------|
| Learned during training | Retrieved at runtime |
| General information | Private or live information |
| Static | Dynamic |
| Built into the model | Obtained from tools |

Examples

Knowledge

```
What is GST?
```

Access

```
What was my GST amount yesterday?
```

---

# Why Prompts Fail

Suppose we write

```
You are an expert invoice assistant.

Explain yesterday's invoice.
```

Question

How does the model know

yesterday's invoice?

It doesn't.

No prompt can invent

private enterprise data.

The application must retrieve it.

---

# The Evolution

Traditional chatbot

```
User

↓

Prompt

↓

LLM

↓

Answer
```

AI Agent

```
User

↓

Agent

↓

LLM

↓

Tool

↓

Observation

↓

LLM

↓

Answer
```

Notice

The model now reasons

over

real observations.

---

# Engineering Analogy

Think of a calculator.

Without input

it cannot compute.

Similarly,

an LLM cannot reason about data

it cannot access.

Tools provide that input.

---

# What Is a Tool?

A tool is

> **A deterministic software capability that an AI system can invoke to obtain information or perform an action.**

Examples

- REST API
- SQL Query
- Search Engine
- Calendar
- Email
- Weather
- Payment API
- Internal Business Service

The tool performs the action.

The LLM decides whether the tool should be used.

---

# Architecture

```
User

↓

Agent

↓

LLM

↓

Should I use a tool?

↓

YES

↓

Tool

↓

Observation

↓

LLM

↓

Answer
```

The observation becomes part of the model's reasoning.

---

# Types of Tools

## Retrieval Tools

Purpose

Read information.

Examples

- Invoice lookup
- Knowledge base
- Search
- SQL SELECT

---

## Action Tools

Purpose

Change something.

Examples

- Book ride
- Create ticket
- Send email
- Refund payment

Action tools usually require stronger governance.

---

## Computational Tools

Purpose

Perform calculations.

Examples

- Calculator
- Currency conversion
- Tax computation

---

## External Service Tools

Purpose

Call external APIs.

Examples

- Maps
- Weather
- Payment Gateway
- CRM

---

# Why Enterprise AI Depends on Tools

Enterprise information changes constantly.

Examples

- Invoice totals
- Account balance
- Inventory
- Pricing
- Order status

Training an LLM every time these values change is impractical.

Instead,

retrieve the information when needed.

---

# Running Case Study

Invoice Explainability Agent

Without tools

```
User

↓

LLM

↓

Guess
```

With tools

```
User

↓

Invoice Tool

↓

Pricing Tool

↓

Tax Tool

↓

LLM

↓

Grounded Explanation
```

The explanation becomes evidence-based.

---

# Engineering Perspective

The LLM should **not** access databases directly.

Instead,

```
LLM

↓

Tool Request

↓

Application

↓

Database

↓

Result

↓

LLM
```

The application controls permissions,

validation,

and auditing.

---

# Production Insight

Typical enterprise tool layer

```
LLM

↓

Tool Router

↓

Authentication

↓

Authorization

↓

API Gateway

↓

Business Services

↓

Response
```

Notice

the LLM never bypasses enterprise security.

---

# Failure Modes

What if the tool fails?

Examples

```
API Timeout

↓

Retry
```

```
Permission Denied

↓

Return Safe Error
```

```
Database Unavailable

↓

Fallback Response
```

The application—not the LLM—handles operational failures.

---

# Governance

Every tool invocation should record:

- User
- Timestamp
- Tool Name
- Parameters
- Result
- Latency
- Success/Failure

These records support:

- Auditing
- Debugging
- Compliance

---

# Common Misconceptions

## "LLMs already know everything."

False.

Training knowledge is different from runtime access.

---

## "RAG replaces tools."

False.

RAG retrieves documents.

Tools retrieve or modify structured systems.

Both are useful.

---

## "The LLM should call APIs directly."

False.

The application should mediate every tool invocation.

---

## "Every question needs a tool."

False.

General knowledge questions often do not.

---

# Best Practices

✅ Separate reasoning from execution.

✅ Validate tool inputs.

✅ Validate tool outputs.

✅ Log every tool call.

✅ Keep tools deterministic.

✅ Enforce authorization outside the LLM.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| General knowledge | LLM only | No runtime data required |
| Enterprise data | Retrieval Tool | Live information |
| Financial transaction | Action Tool + Policy Engine | Governance |
| Database lookup | SQL/API Tool | Structured data |
| Search documents | RAG | Unstructured data |

---

# Engineering Decision Record (EDR)

## Problem

Need accurate invoice explanations.

## Options

1. Prompt only

2. Prompt + RAG

3. Prompt + Tools

4. Prompt + Tools + RAG

## Decision

Use Tools for structured enterprise data and RAG for unstructured knowledge.

## Trade-offs

Pros

- Live information
- Lower hallucination risk
- Better auditability

Cons

- More infrastructure
- API management
- Error handling

## Recommendation

Treat tools as deterministic software components controlled by the application.

---

# Key Takeaways

- LLMs provide reasoning.
- Tools provide access.
- Enterprise AI depends on tools.
- Applications—not LLMs—should execute tools.
- Tool results become observations that improve reasoning.

---

# Interview Questions

### Q1

Why do AI Agents need tools?

---

### Q2

Knowledge vs Access?

---

### Q3

Why can't prompts replace tools?

---

### Q4

What types of tools exist?

---

### Q5

Should an LLM connect directly to a database?

Why?

---

### Q6

How should tool failures be handled?

---

### Q7

Draw an enterprise tool architecture.

---

### Q8

RAG vs Tools?

---

# Hands-on Exercise

## Objective

Convert a chatbot into a tool-enabled assistant.

### Step 1

Create a prompt-only invoice assistant.

### Step 2

Replace hardcoded invoice data with a mock Invoice API.

### Step 3

Return the API response to the LLM.

### Step 4

Compare:

- Correctness
- Grounding
- Hallucination rate
- Maintainability

### Expected Outcome

The tool-enabled assistant should provide explanations based on retrieved invoice data rather than assumptions.

---

# Further Reading

- LangChain Tools Documentation
- LangGraph Documentation
- OpenAI Function Calling Documentation
- Anthropic Tool Use Documentation
- Model Context Protocol (MCP) Specification
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI Agent Architecture
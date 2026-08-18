# Chapter 3 Labs

> **Tools & Function Calling**

> *Building Production AI Agents*

---

# Lab Philosophy

Every lab follows

the Engineering Method.

```
Problem

↓

Hypothesis

↓

Architecture

↓

Build

↓

Measure

↓

Observation

↓

Conclusion

↓

Next Iteration
```

This is exactly how production AI systems evolve.

---

# Lab 1

## Hello Tool

Difficulty

⭐

Time

20 Minutes

---

## Objective

Transform

Prompt-only AI

into

Tool-enabled AI.

---

Architecture

```
User

↓

LLM

↓

get_invoice()

↓

Observation

↓

LLM

↓

Answer
```

---

## Build

Create

```
get_invoice()
```

Return

mock JSON.

---

## Measure

Observe

difference

between

Prompt-only

vs

Tool-enabled.

---

## Expected Outcome

The model

stops guessing

and starts reasoning

over observations.

---

# Lab 2

## Tool Schema

Difficulty

⭐

---

Build

JSON Schema

for

```
get_invoice()
```

Validate

arguments

before execution.

---

Measure

- invalid arguments
- missing fields
- incorrect types

---

Expected

Schema validation

prevents execution errors.

---

# Lab 3

## Function Calling

Difficulty

⭐⭐

---

Architecture

```
User

↓

LLM

↓

Function Request

↓

Execution

↓

Observation

↓

LLM
```

---

Build

your first

Function Calling

pipeline.

---

Measure

- tool selection
- argument quality
- response correctness

---

# Lab 4

## Tool Selection

Difficulty

⭐⭐

---

Create

five tools.

```
Invoice

Tax

Pricing

Weather

Calendar
```

Ask

```
Explain invoice.
```

Observe

selected tool.

---

Improve

tool descriptions.

Repeat.

---

Expected

Selection accuracy improves.

---

# Lab 5

## Tool Execution

Difficulty

⭐⭐⭐

---

Simulate

```
Timeout

↓

Retry

↓

Success
```

Then

```
Permission Denied
```

Observe

execution pipeline.

---

Measure

latency

retry count

failure rate

---

Expected

Reliable execution

requires

more than

Function Calling.

---

# Lab 6

## Tool Responses

Difficulty

⭐⭐⭐

---

Create

three responses.

```
Success
```

```
Failure
```

```
Partial Success
```

Observe

how

the LLM

responds.

---

Measure

answer quality.

---

Expected

Structured observations

improve reasoning.

---

# Lab 7

## Multi-tool Agent

Difficulty

⭐⭐⭐⭐

---

Architecture

```
Invoice Tool

↓

Pricing Tool

↓

Tax Tool

↓

LLM
```

---

Build

three-tool pipeline.

---

Measure

latency

tool usage

answer quality.

---

Expected

Multiple observations

produce

better explanations.

---

# Lab 8

## Tool Orchestration

Difficulty

⭐⭐⭐⭐

---

Build

two versions.

Sequential

```
Invoice

↓

Pricing

↓

Tax
```

Parallel

```
Invoice

Pricing

Tax

↓

Merge
```

---

Compare

- latency
- complexity
- maintainability

---

Expected

Parallel execution

reduces latency

for independent tasks.

---

# Lab 9

## Production Tool Layer

Difficulty

⭐⭐⭐⭐

---

Design

```
Tool Router

↓

Registry

↓

Authorization

↓

Execution

↓

Observation Builder
```

Implement

mock version.

---

Measure

- latency
- logs
- retries
- metrics

---

Expected

Platform architecture

simplifies

enterprise integrations.

---

# Lab 10

## Production Agent

Difficulty

⭐⭐⭐⭐⭐

---

Build

Version 3

of

Invoice Explainability Agent.

Architecture

```
User

↓

Authentication

↓

Planner

↓

Prompt Builder

↓

Retriever

↓

Invoice Tool

↓

Pricing Tool

↓

Tax Tool

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

Features

✓ Prompt Templates

✓ Context Builder

✓ Tool Calling

✓ Function Calling

✓ Tool Router

✓ Validation

✓ Structured Output

✓ Decision Logs

✓ Retry

✓ Timeout

✓ Observability

---

Deliverable

Production-style

Agent.

---

# Bonus Lab

## Model Context Protocol

Build

conceptual architecture.

```
Agent

↓

MCP Client

↓

Invoice MCP Server

↓

Pricing MCP Server

↓

Observation

↓

LLM
```

---

Expected

Understand

why

MCP exists.

---

# Engineering Notebook

Every lab

must include

```
Question

↓

Hypothesis

↓

Architecture

↓

Implementation

↓

Measurements

↓

Observation

↓

Conclusion

↓

Next Improvement
```

---

# Evaluation Rubric

| Category | Points |
|-----------|---------|
| Architecture | 20 |
| Tool Design | 15 |
| Tool Execution | 15 |
| Orchestration | 15 |
| Observability | 10 |
| Documentation | 10 |
| Engineering Notebook | 10 |
| Reflection | 5 |

Total

100

---

# Chapter Completion Checklist

By the end

of Chapter 3

you should be able to

✅ Build Tools

✅ Build Function Calling

✅ Build Tool Router

✅ Build Tool Registry

✅ Build Tool Layer

✅ Build Multi-tool Agent

✅ Build Production Agent

---

# Capstone Challenge

Build

Invoice Explainability Agent

Version 3

Requirements

```
Prompt Builder

+

Context Builder

+

Tool Router

+

Invoice Tool

+

Pricing Tool

+

Tax Tool

+

Structured Output

+

Validation

+

Decision Logs

+

Observability
```

Do **not**

implement

memory

yet.

That belongs

to

Chapter 4.

---

# Reflection

Answer

1.

Why do AI Agents need tools?

2.

Which orchestration pattern worked best?

3.

Which failures occurred most often?

4.

How would you improve reliability?

5.

Which part should become deterministic?

Document

your answers.

These become

the starting point

for

Chapter 4.

---

# Further Challenge

Refactor

your solution

into

```
Planner

↓

Executor

↓

Tool Router

↓

Observation Store

↓

State

↓

LLM
```

This prepares you

for

LangGraph

and

Agent Loops.
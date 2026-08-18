# Chapter 3 Interview Guide

> **Tools & Function Calling**

> *Think Like an AI Platform Engineer*

---

# How To Use This Guide

Do not memorize answers.

Instead,

answer every question using

```
Problem

↓

Architecture

↓

Trade-offs

↓

Production

↓

Recommendation
```

Interviewers are evaluating

engineering judgement,

not vocabulary.

---

# Interview Levels

Questions are organized by engineering level.

```
Level 1

↓

Level 2

↓

Level 3

↓

Level 4
```

---

# Level 1

## AI Fundamentals

---

### Q1

Why do AI Agents need tools?

Expected Answer

LLMs possess knowledge,

but they do not possess access.

Tools provide

live,

private,

enterprise information.

Without tools,

the model guesses.

With tools,

the model reasons over observations.

---

### Q2

Tool vs Function?

Expected Answer

A Function

is implementation.

A Tool

is a capability

with

- description
- schema
- validation
- execution
- observation.

---

### Q3

Function Calling

vs

Tool Calling?

Expected Answer

Function Calling

is one mechanism

used to invoke

a tool.

Tools

represent

capabilities.

---

### Q4

Why use JSON Schema?

Expected Discussion

Validation

Contracts

Reliability

Automation

---

### Q5

Can an LLM execute Python?

Expected Answer

No.

The application

executes software.

The LLM proposes actions.

---

# Level 2

## Application Engineering

---

### Q6

How does Tool Selection work?

Expected Discussion

The model reasons over

- tool names
- descriptions
- schemas
- user request
- context

Good metadata improves selection.

---

### Q7

Why validate arguments?

Expected Answer

Never trust generated inputs.

Validation prevents

- missing parameters
- invalid types
- dangerous requests.

---

### Q8

Why normalize Tool Responses?

Expected Discussion

Consistency.

Different APIs

should produce

a common observation format.

---

### Q9

Observation

vs

Response?

Expected Answer

Observation

↓

Internal reasoning input

Response

↓

User-facing output

---

### Q10

Why separate Tool Router

from

Tool Execution?

Expected Answer

Routing decides

where.

Execution performs

the action.

Separation improves

maintainability.

---

# Level 3

## Production AI Systems

---

### Q11

Why should the LLM never access databases directly?

Expected Discussion

Authentication

Authorization

Validation

Auditing

Governance

These belong

outside

the model.

---

### Q12

How do retries work?

Expected Answer

Application

↓

Retry Manager

↓

Backoff

↓

Execution

Never ask

the LLM

to retry.

---

### Q13

What is idempotency?

Expected Discussion

Repeated execution

should not create

duplicate side effects.

Critical

for

payments

refunds

orders.

---

### Q14

Why use Circuit Breakers?

Expected Answer

Protect downstream services.

Prevent cascading failures.

---

### Q15

Why use Tool Registries?

Expected Answer

Discovery

Governance

Versioning

Ownership

Permissions

---

### Q16

What belongs inside a Tool Layer?

Expected Discussion

Router

Registry

Authentication

Authorization

Execution

Observability

Metrics

Logging

---

### Q17

What is MCP?

Expected Answer

A protocol

that standardizes

how AI systems discover

and interact with

tools,

resources

and

prompts.

---

### Q18

MCP

vs

REST?

Expected Discussion

REST

↓

Web APIs

MCP

↓

AI capability protocol

---

### Q19

Why use a Planner?

Expected Answer

Planning

and

execution

are different responsibilities.

Planner

↓

Reasoning

Executor

↓

Action

---

### Q20

Sequential

vs

Parallel Orchestration?

Expected Discussion

Sequential

↓

Dependencies

Parallel

↓

Independent tasks

---

# Level 4

## Architecture

---

### Q21

Design

an Invoice Explainability Agent.

Expected Whiteboard

```
Authentication

↓

Planner

↓

Retriever

↓

Tool Router

↓

Invoice Tool

↓

Pricing Tool

↓

Tax Tool

↓

LLM

↓

Validator

↓

Decision Logs

↓

Response
```

Explain

every box.

---

### Q22

Where should

Authorization

occur?

Correct Answer

Application Layer.

Never

inside

the LLM.

---

### Q23

How would you reduce hallucinations?

Expected Discussion

Retrieval

↓

Tools

↓

Validation

↓

Grounding

↓

Evaluation

---

### Q24

Should every request

use

an LLM?

Expected Answer

No.

Simple requests

may use

SLMs

or

deterministic software.

---

### Q25

How would you monitor

an AI Agent?

Expected Discussion

Latency

Cost

Token Usage

Tool Success Rate

Retries

Hallucination Rate

Evaluation Score

User Satisfaction

---

### Q26

What belongs

inside

Decision Logs?

Expected Answer

Timestamp

User

Prompt Version

Model

Tool Calls

Observations

Policies

Latency

Cost

Response

---

### Q27

Draw

a Production Tool Layer.

Expected Components

```
Agent

↓

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

---

### Q28

What is

the biggest

production challenge

with Tool Calling?

Expected Discussion

Reliability

not

Function Calling.

---

### Q29

How would you deploy

a Production Agent?

Expected Discussion

Prompt Registry

↓

Tool Layer

↓

Evaluation

↓

Monitoring

↓

Rollback

↓

Observability

---

### Q30

What differentiates

an AI Engineer

from

a Prompt Engineer?

Expected Answer

An AI Engineer designs

software systems.

A Prompt Engineer focuses primarily on model behavior.

Enterprise AI requires

both.

---

# Whiteboard Exercises

---

## Exercise 1

Draw

Tool Calling Lifecycle.

```
User

↓

LLM

↓

Function

↓

Execution

↓

Observation

↓

LLM
```

---

## Exercise 2

Draw

Tool Layer.

---

## Exercise 3

Draw

Planner

vs

Executor.

---

## Exercise 4

Draw

Production Agent Architecture.

---

## Exercise 5

Draw

Invoice Explainability Agent.

---

# Common Interview Traps

---

## Trap

"The LLM executes tools."

Reality

Applications execute tools.

---

## Trap

"Function Calling is JSON."

Reality

JSON

is

transport.

Function Calling

is

workflow.

---

## Trap

"Tool Calling solves hallucinations."

Reality

Grounded retrieval,

validation,

and

good observations

reduce hallucinations.

---

## Trap

"One tool is enough."

Reality

Enterprise workflows

usually require

multiple tools.

---

## Trap

"MCP replaces APIs."

Reality

MCP standardizes AI interactions.

REST APIs still exist underneath.

---

# Engineering Thought Process

When answering

always explain

```
Problem

↓

Architecture

↓

Trade-offs

↓

Reliability

↓

Production
```

This demonstrates

engineering maturity.

---

# Chapter 3 Self-Assessment

Can you explain

without notes

✅ Tool Anatomy

✅ Function Calling

✅ Tool Selection

✅ Tool Execution

✅ Tool Responses

✅ Tool Orchestration

✅ MCP

✅ Production Tool Layer

✅ Production Agent Architecture

If yes,

you are ready

for

Chapter 4

Agent Loop & State Management.

---

# Recommended Whiteboard Challenge

Design

a production-ready

Invoice Explainability Agent

using

everything

from

Chapters

1–3.

Explain

every component

and

why it exists.

If you can do this confidently,

you have developed a solid foundation in modern AI Agent Engineering.

---

# Further Reading

- LangGraph Documentation
- LangChain Documentation
- OpenAI Function Calling Documentation
- Anthropic Tool Use Documentation
- Model Context Protocol (MCP) Specification
- Microsoft Agent Framework Documentation
- OWASP Top 10 for LLM Applications
- ByteByteGo articles on AI Agents and System Design
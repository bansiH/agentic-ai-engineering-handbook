# Chapter 4 Interview Guide

> **Agent Runtime**

> *Think Like an AI Runtime Engineer*

---

# How To Use This Guide

Do **not** memorize answers.

Instead,

answer using

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

The interviewer wants to understand

how

you think.

---

# Interview Levels

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

## Agent Fundamentals

---

### Q1

Why do AI Agents need loops?

Expected Answer

Many enterprise problems require multiple reasoning and execution steps.

The Agent Loop allows the system to gather observations,

update state,

and continue until the objective is achieved.

---

### Q2

What is an Agent Loop?

Expected Answer

A repeated cycle of

```
Observe

↓

Reason

↓

Plan

↓

Act

↓

Observe
```

until the task is complete.

---

### Q3

Chatbot

vs

Agent?

Expected Discussion

Chatbot

↓

Single inference.

Agent

↓

Iterative reasoning,

tools,

state,

memory.

---

### Q4

Why isn't one LLM call sufficient?

Expected Discussion

Complex business tasks require

multiple observations,

tool executions,

and planning.

---

### Q5

What terminates an Agent Loop?

Expected Answer

Possible stopping conditions include:

- Goal achieved
- Maximum iterations
- Human approval required
- Timeout
- Policy decision

---

# Level 2

## Runtime Engineering

---

### Q6

What is Agent State?

Expected Answer

The complete execution snapshot of the workflow.

---

### Q7

State

vs

Short-term Memory?

Expected Answer

State

↓

Current execution.

Short-term Memory

↓

Current conversation.

---

### Q8

Short-term Memory

vs

Long-term Memory?

Expected Answer

Short-term

↓

Conversation continuity.

Long-term

↓

Persistent user knowledge.

---

### Q9

Long-term Memory

vs

RAG?

Expected Answer

Long-term Memory

↓

User-specific knowledge.

RAG

↓

Organizational knowledge.

---

### Q10

Why use Reflection?

Expected Answer

Reflection evaluates intermediate or final outputs and may trigger improvement before returning a response.

---

### Q11

Reflection

vs

Validation?

Expected Discussion

Reflection

↓

Probabilistic reasoning.

Validation

↓

Deterministic rule enforcement.

---

### Q12

Why separate Planner

from

Executor?

Expected Answer

Planner reasons.

Executor performs deterministic actions.

---

# Level 3

## Production Runtime

---

### Q13

What belongs inside

the Agent Runtime?

Expected Discussion

Planner

State Manager

Memory Manager

Context Builder

Tool Router

Reflection

Policy Engine

Decision Logs

Observability

---

### Q14

Why is State Management important?

Expected Answer

State allows workflows to continue,

recover,

and coordinate multiple reasoning steps.

---

### Q15

Why use Checkpointing?

Expected Discussion

Crash recovery.

Replay.

Durable execution.

---

### Q16

What is Durable Execution?

Expected Answer

The ability for long-running workflows to survive restarts without losing progress.

---

### Q17

Human-in-the-Loop

vs

Human-on-the-Loop?

Expected Answer

HITL

↓

Approval before execution.

HOTL

↓

Monitoring after execution.

---

### Q18

Where should Human Approval occur?

Expected Discussion

Within the runtime,

after policy evaluation,

before high-risk execution.

---

### Q19

How do you prevent infinite Agent Loops?

Expected Answer

- Maximum iterations
- Timeout
- Completion criteria
- Reflection limits

---

### Q20

How would you replay

an Agent execution?

Expected Discussion

Checkpoint

↓

State

↓

Decision Logs

↓

Replay

---

# Level 4

## Architecture

---

### Q21

Draw

a Production Agent Runtime.

Expected Whiteboard

```
User

↓

Planner

↓

State

↓

Memory

↓

Retriever

↓

Tools

↓

Reflection

↓

Policy

↓

Response
```

---

### Q22

Why shouldn't

the LLM

manage state?

Expected Answer

State belongs to deterministic runtime software.

The LLM reasons over state but does not own it.

---

### Q23

Where does Memory fit?

Expected Discussion

Memory is a context source.

The Memory Manager retrieves relevant information for the current reasoning step.

---

### Q24

Why separate

State

Memory

RAG?

Expected Answer

They solve different engineering problems.

State

↓

Execution.

Memory

↓

Personalization.

RAG

↓

Enterprise knowledge.

---

### Q25

How would you recover

after

a runtime crash?

Expected Discussion

Checkpoint

↓

Restore State

↓

Resume

---

### Q26

What metrics would you monitor?

Examples

- Latency
- Iterations
- Token usage
- Reflection rate
- Tool failures
- Cost
- Completion rate

---

### Q27

What belongs inside

Decision Logs?

Expected Answer

Planner decisions

Tool calls

Observations

State transitions

Policies

Human approvals

Latency

Cost

---

### Q28

When would you

disable Reflection?

Expected Discussion

Low-risk,

low-latency,

high-throughput workloads.

---

### Q29

When would you

switch from

HOTL

to

HITL?

Expected Discussion

Incident

↓

Higher risk

↓

Manual approval

---

### Q30

Explain

the complete

Agent Runtime

using

Invoice Explainability Agent.

Expected Answer

Candidate should explain every runtime component and justify why it exists.

---

# Whiteboard Exercises

---

## Exercise 1

Draw

Agent Loop.

---

## Exercise 2

Draw

Planner

vs

Executor.

---

## Exercise 3

Draw

State Management.

---

## Exercise 4

Draw

Memory Architecture.

---

## Exercise 5

Draw

Production Runtime.

---

# Common Interview Traps

---

## Trap

State

=

Memory.

Reality

State

↓

Execution.

Memory

↓

Persistence.

---

## Trap

Reflection

=

Validation.

Reality

Reflection

↓

Reasoning.

Validation

↓

Rules.

---

## Trap

The LLM

owns

the runtime.

Reality

Runtime

owns

the LLM.

---

## Trap

Every workflow

needs

Human Approval.

Reality

Approval

depends

on

risk.

---

## Trap

Checkpointing

is

optional.

Reality

Long-running enterprise workflows generally require durable execution.

---

# Engineering Thought Process

Always answer using

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

Never answer

with

definitions only.

---

# Chapter 4 Self-Assessment

Can you explain

without notes

✅ Agent Loop

✅ Planning

✅ State

✅ Short-term Memory

✅ Long-term Memory

✅ Reflection

✅ HITL

✅ HOTL

✅ Production Runtime

If yes,

you are ready

for

Chapter 5

Retrieval-Augmented Generation (RAG).

---

# Staff Engineer Challenge

Design

a Production Agent Runtime

that supports

- Planning
- State
- Memory
- Reflection
- Checkpointing
- Human Approval
- Replay
- Observability

Explain

why

every component

exists.

Discuss

failure modes,

trade-offs,

and

scalability.

---

# Further Reading

- LangGraph Documentation (StateGraph, durable execution)
- Microsoft Agent Framework Documentation
- ReAct paper
- Reflexion paper
- NIST AI Risk Management Framework
- ByteByteGo articles on workflow engines, distributed systems and AI architecture
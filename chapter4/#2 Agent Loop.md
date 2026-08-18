# Agent Loop

> **Chapter 4 – Agent Runtime**

> *The execution engine behind modern AI Agents.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain the Agent Loop.
- Understand Observe → Reason → Plan → Act.
- Explain iterative reasoning.
- Understand why Agent Loops enable autonomy.
- Design production agent loops.
- Explain Agent Loops in interviews.

---

# Why Agent Loops Matter

Traditional software follows

```
Input

↓

Logic

↓

Output
```

Traditional chatbots follow

```
Question

↓

LLM

↓

Answer
```

AI Agents follow

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

↓

Repeat
```

This continuous cycle allows the system to solve problems that cannot be completed in a single step.

---

# First Principle

An Agent Loop is

> **A repeated cycle in which an AI Agent observes the current state of the world, reasons about it, performs an action, observes the result, and decides what to do next until the objective is complete.**

The important word is

**until**.

Agents continue reasoning until they reach a stopping condition.

---

# Mental Model

Imagine assembling furniture.

You don't

```
Read instructions

↓

Immediately finish
```

Instead

```
Read

↓

Build

↓

Check

↓

Adjust

↓

Continue
```

Every action creates

new observations.

Those observations influence

future actions.

That is

the Agent Loop.

---

# Evolution

Static Assistant

```
User

↓

LLM

↓

Answer
```

Tool-enabled Assistant

```
User

↓

LLM

↓

Tool

↓

Answer
```

Autonomous Agent

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

↓

Reason

↓

Act

↓

Finish
```

Notice

reasoning happens

multiple times.

---

# The Core Loop

The fundamental execution cycle

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

Every production agent

implements

some variation

of this loop.

---

# Stage 1 — Observe

The agent gathers information.

Possible sources

- User
- Tool
- Database
- Retriever
- Memory
- Previous State

Architecture

```
Question

↓

Observation
```

Observation becomes

the input

for reasoning.

---

# Stage 2 — Reason

The model asks

```
Do I know enough?

Do I need another tool?

Should I stop?

Should I ask a question?
```

Reasoning determines

the next action.

---

# Stage 3 — Plan

The planner transforms

a goal

into

steps.

Example

```
Explain invoice

↓

Retrieve invoice

↓

Retrieve pricing

↓

Retrieve tax

↓

Explain
```

The plan may change

as new observations arrive.

---

# Stage 4 — Act

Possible actions

- Tool Call
- Database Query
- RAG Retrieval
- Ask User
- Save Memory
- Escalate
- Finish

Actions change

the world

or

the agent's understanding.

---

# Stage 5 — Observe Again

After every action

the agent receives

new information.

Example

```
Invoice

↓

Observation

↓

Need Pricing?

↓

Yes
```

The loop continues.

---

# Loop Termination

Every loop requires

a stopping condition.

Examples

```
Goal Complete
```

```
Maximum Iterations
```

```
Human Approval Required
```

```
Timeout
```

```
No More Tools
```

Without termination

agents can loop forever.

---

# Agent Runtime

The loop lives

inside

the runtime.

```
User

↓

Planner

↓

Loop

↓

State

↓

Observation

↓

Decision

↓

Continue?
```

The runtime controls

execution.

The LLM performs reasoning.

---

# Running Case Study

Invoice Explainability Agent

```
Question

↓

Reason

↓

Invoice Tool

↓

Observation

↓

Need Pricing?

↓

Pricing Tool

↓

Observation

↓

Need Tax?

↓

Tax Tool

↓

Observation

↓

Explain

↓

Finish
```

Notice

the agent

does not answer

until

it has enough evidence.

---

# Agent State

The loop continuously updates state.

```
State 0

↓

Invoice Retrieved

↓

State 1

↓

Pricing Retrieved

↓

State 2

↓

Tax Retrieved

↓

State 3

↓

Complete
```

The state evolves

throughout execution.

---

# Planner–Executor

Responsibilities differ.

Planner

↓

Reasons

Executor

↓

Acts

Architecture

```
Planner

↓

Execution Plan

↓

Executor

↓

Observation

↓

Planner
```

This separation improves reliability.

---

# Reflection

Many agents

perform

one additional step.

```
Answer

↓

Review

↓

Improve

↓

Return
```

Reflection is

another loop.

---

# Multi-Step Example

User

```
Plan my business trip.
```

Loop

```
Observe

↓

Calendar

↓

Flights

↓

Hotels

↓

Company Policy

↓

Budget

↓

Plan

↓

Answer
```

One prompt

cannot reliably perform

this workflow.

---

# Engineering Perspective

The Agent Loop transforms

```
LLM

↓

Inference
```

into

```
LLM

↓

Decision Making

↓

Software Execution

↓

Learning From Observations
```

This is why modern agents behave differently from chatbots.

---

# Production Insight

Production runtime

```
Planner

↓

State Manager

↓

Loop

↓

Tools

↓

Observations

↓

State Update

↓

Continue?

↓

Finish
```

Every iteration

is logged.

---

# Failure Modes

## Infinite Loop

Mitigation

```
Maximum Iterations
```

---

## Repeated Tool Calls

Mitigation

```
State Inspection

↓

Avoid Duplicate Actions
```

---

## Premature Exit

Mitigation

```
Completion Criteria
```

---

## Wrong Plan

Mitigation

```
Reflection

↓

Replanning
```

---

# Engineering Notebook

Experiment.

Create

an agent

that can only call

one tool.

Now

allow

multiple iterations.

Observe

how

the solution quality

changes.

Question

When should

the loop stop?

---

# Common Misconceptions

## "Every Agent needs dozens of loops."

False.

Some tasks complete in one iteration.

---

## "The loop belongs inside the LLM."

False.

The runtime owns the loop.

---

## "Reasoning and execution are the same."

False.

Reasoning decides.

Execution acts.

---

## "Planning happens only once."

False.

Modern agents frequently replan after new observations.

---

# Best Practices

✅ Keep reasoning separate from execution.

✅ Update state after every observation.

✅ Define explicit stopping conditions.

✅ Log every iteration.

✅ Prevent infinite loops.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Single-step FAQ | One LLM call | Simpler |
| Tool-dependent workflow | Agent Loop | Iterative reasoning |
| Long-running workflow | Planner + Loop | Better control |
| High-risk actions | Loop + Human Approval | Governance |

---

# Engineering Decision Record (EDR)

## Problem

Need an agent capable of solving multi-step enterprise tasks.

## Options

1. Single prompt

2. Tool Calling

3. Agent Loop

## Decision

Agent Loop with explicit planning, observations and termination criteria.

## Trade-offs

Pros

- Handles complex workflows
- Adapts to new information
- Supports multiple tools

Cons

- Higher latency
- More orchestration
- State management complexity

## Recommendation

Use Agent Loops for workflows that require iterative reasoning rather than one-shot responses.

---

# Key Takeaways

- Agent Loops enable iterative reasoning.
- Observations drive future decisions.
- Planning and execution are separate responsibilities.
- Every loop requires termination conditions.
- Modern AI Agents are runtime systems, not single model calls.

---

# Interview Questions

### Q1

What is an Agent Loop?

---

### Q2

Why do AI Agents require loops?

---

### Q3

Explain Observe → Reason → Plan → Act.

---

### Q4

Who owns the Agent Loop?

---

### Q5

Why is state important?

---

### Q6

How do agents know when to stop?

---

### Q7

Planner vs Executor?

---

### Q8

Draw an Agent Loop architecture.

---

# Hands-on Exercise

## Objective

Build your first iterative AI Agent.

### Step 1

Create an agent that retrieves invoice information.

### Step 2

After each observation, decide whether another tool is required.

### Step 3

Continue until the explanation is complete.

### Step 4

Log every iteration and the reason for each decision.

### Expected Outcome

The final explanation should be based on accumulated observations rather than a single prompt. You should also be able to explain why each iteration occurred and what triggered the termination of the loop.

---

# Production Readiness Checklist

☑ Explicit agent loop

☑ Planner integrated

☑ State updates

☑ Observation logging

☑ Maximum iteration limit

☑ Completion criteria

☑ Reflection support

☑ Monitoring

☑ Decision logs

---

# Further Reading

- ReAct: Synergizing Reasoning and Acting in Language Models
- LangGraph Documentation
- Microsoft Agent Framework Documentation
- Model Context Protocol (MCP) Specification
- ByteByteGo articles on AI Agent architecture
# Planning

> **Chapter 4 – Agent Runtime**

> *From Goals to Executable Plans*

---

# Learning Objectives

After completing this section you should be able to:

- Explain why AI Agents need planning.
- Understand goal decomposition.
- Differentiate planning from execution.
- Explain Planner–Executor architectures.
- Understand dynamic replanning.
- Design production planning systems.
- Explain planning during interviews.

---

# Why Planning Exists

Suppose a user asks

```
Explain why my invoice increased
compared to last month and recommend
ways to reduce future costs.
```

Could one tool solve this?

No.

The agent must first determine

- what information is required
- where to obtain it
- in what order to retrieve it
- when enough information has been collected

This process is planning.

---

# First Principle

Planning answers

> **How do I achieve the user's goal?**

Execution answers

> **Carry out the next step.**

Planning and execution are different responsibilities.

---

# Engineering Mental Model

Imagine travelling from Chennai to Bengaluru.

You don't immediately start driving.

Instead you decide

```
Destination

↓

Route

↓

Fuel

↓

Stops

↓

Drive
```

Planning occurs

before

execution.

AI Agents behave similarly.

---

# Goal vs Plan

Goal

```
Explain invoice.
```

Plan

```
Retrieve invoice

↓

Retrieve pricing rules

↓

Retrieve tax rules

↓

Compare

↓

Explain
```

One goal.

Multiple steps.

---

# Architecture

```
Goal

↓

Planner

↓

Execution Plan

↓

Executor

↓

Observations

↓

Planner

↓

Updated Plan

↓

Done
```

The planner continuously evaluates progress.

---

# Why LLMs Need Planning

LLMs are excellent at reasoning,

but complex business tasks require

multiple coordinated actions.

Without planning,

agents may

- call unnecessary tools
- miss required information
- repeat work
- stop too early

Planning reduces these problems.

---

# Planning Pipeline

```
User Goal

↓

Understand Intent

↓

Identify Required Information

↓

Choose Tools

↓

Determine Execution Order

↓

Execute

↓

Evaluate Progress

↓

Continue?

↓

Finish
```

Planning is iterative.

---

# Static Planning

The plan is created once.

Example

```
Invoice

↓

Pricing

↓

Tax

↓

Answer
```

Advantages

- Simple
- Predictable
- Easy to debug

Limitations

Cannot adapt if new information changes the workflow.

---

# Dynamic Planning

The plan evolves.

Example

```
Invoice Missing

↓

Search Customer

↓

Retrieve Invoice

↓

Continue
```

The planner reacts to observations.

This is common in production agents.

---

# Planner–Executor Pattern

Separate

thinking

from

doing.

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

Planner responsibilities

- determine next action
- evaluate observations
- decide completion

Executor responsibilities

- call tools
- validate inputs
- return observations

---

# Task Decomposition

Large goals become smaller tasks.

Example

```
Goal

↓

Understand Invoice

↓

Retrieve Invoice

Retrieve Pricing

Retrieve Tax

Compare

Explain
```

Each task should be independently executable.

---

# Dependencies

Not every task can execute immediately.

Example

```
Retrieve Invoice

↓

Invoice ID

↓

Retrieve Pricing
```

Pricing depends on invoice information.

Dependency management is part of planning.

---

# Sequential Planning

Tasks execute one after another.

```
Task A

↓

Task B

↓

Task C
```

Useful when later tasks depend on earlier observations.

---

# Parallel Planning

Independent tasks execute simultaneously.

```
Invoice

Pricing

Tax

↓

Merge

↓

Reason
```

Benefits

- lower latency
- improved throughput

---

# Conditional Planning

Business rules influence the plan.

Example

```
Invoice > ₹50,000 ?

↓

YES

↓

Approval Workflow

NO

↓

Continue
```

Planning is driven by both observations and policies.

---

# Replanning

Sometimes

new information invalidates the original plan.

Example

```
Invoice Not Found

↓

Search Alternative Account

↓

Retrieve Invoice

↓

Continue
```

The planner updates the workflow instead of failing immediately.

---

# Running Case Study

Invoice Explainability Agent

Initial Goal

```
Explain invoice.
```

Plan

```
Invoice Tool

↓

Pricing Tool

↓

Tax Tool

↓

Policy Tool

↓

Reason

↓

Explain
```

Observation

```
Pricing Rule Missing
```

Updated Plan

```
Search Pricing Policy

↓

Retrieve Pricing

↓

Continue
```

The plan adapts.

---

# Planning vs Workflow

Workflow

↓

Predefined sequence.

Planning

↓

Sequence generated dynamically.

Enterprise AI systems often combine both.

---

# Planning Horizon

Short Horizon

```
Next Action Only
```

Long Horizon

```
Entire Workflow
```

Different applications require different planning horizons.

---

# Engineering Perspective

Planning is

decision making.

Execution is

task completion.

Separating these responsibilities improves maintainability and observability.

---

# Production Insight

Production planning architecture

```
User

↓

Planner

↓

Task Graph

↓

Executor

↓

Observations

↓

Planner

↓

Completion Check

↓

Response
```

Notice

planning continues until the objective is satisfied.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| Poor initial plan | Replanning |
| Infinite planning | Maximum planning depth |
| Missing dependency | Dependency validation |
| Wrong execution order | Task graph validation |
| Premature completion | Completion criteria |

---

# Engineering Notebook

Experiment

Goal

```
Explain my invoice.
```

Create

Plan A

Sequential.

Create

Plan B

Parallel.

Measure

- latency
- complexity
- correctness

Question

Which plan better matches the business workflow?

---

# Common Misconceptions

## "Planning and execution are the same."

False.

Planning determines

what

should happen.

Execution performs

the action.

---

## "Planning happens only once."

False.

Production agents often replan after new observations.

---

## "Every task requires dynamic planning."

False.

Many workflows are predictable and benefit from predefined execution.

---

## "The LLM should execute the workflow."

False.

Execution belongs to deterministic application components.

---

# Best Practices

✅ Separate planner from executor.

✅ Decompose goals into tasks.

✅ Support replanning.

✅ Track task dependencies.

✅ Define completion criteria.

✅ Log planning decisions.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Predictable workflow | Static planning | Simpler |
| Dynamic investigation | Dynamic planning | Adaptability |
| Independent tasks | Parallel planning | Lower latency |
| Enterprise agents | Planner–Executor | Separation of concerns |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable execution of multi-step invoice analysis.

## Options

1. No planning.

2. Static workflow.

3. Planner–Executor with replanning.

## Decision

Planner–Executor with dynamic replanning.

## Trade-offs

Pros

- Better adaptability
- Handles incomplete information
- Easier workflow evolution

Cons

- Additional orchestration
- More state management

## Recommendation

Treat planning as a first-class capability of the agent runtime.

---

# Key Takeaways

- Planning transforms goals into executable tasks.
- Planning and execution are separate concerns.
- Dynamic replanning enables adaptation.
- Dependencies influence execution order.
- Planner–Executor architectures improve maintainability.

---

# Interview Questions

### Q1

Why do AI Agents need planning?

---

### Q2

Planning vs Execution?

---

### Q3

What is a Planner–Executor architecture?

---

### Q4

What is task decomposition?

---

### Q5

Static planning vs Dynamic planning?

---

### Q6

When should an agent replan?

---

### Q7

How do dependencies affect planning?

---

### Q8

Draw a production planning architecture.

---

# Hands-on Exercise

## Objective

Build a Planner for the Invoice Explainability Agent.

### Requirements

The planner should:

- Identify required information.
- Select the appropriate tools.
- Determine execution order.
- Update the plan when observations change.

### Deliverable

Produce a task graph for:

```
Explain why my invoice increased compared to last month.
```

### Expected Outcome

The agent should generate a plan, execute it step by step, adapt when new observations arrive, and terminate only after sufficient evidence has been collected.

---

# Production Readiness Checklist

☑ Planner implemented

☑ Task decomposition

☑ Dependency tracking

☑ Replanning support

☑ Completion criteria

☑ Planning metrics

☑ Decision logs

☑ Workflow visualization

---

# Further Reading

- LangGraph Documentation
- Microsoft Agent Framework Documentation
- ReAct: Synergizing Reasoning and Acting in Language Models
- HTN (Hierarchical Task Network) Planning
- Workflow orchestration patterns
- ByteByteGo articles on workflow engines and distributed systems
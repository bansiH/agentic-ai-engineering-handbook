# Chapter 4 Labs

> **Agent Runtime**

> *Building Stateful, Durable and Governable AI Agents*

---

# Lab Philosophy

Every lab follows the same engineering workflow.

```
Problem

↓

Hypothesis

↓

Architecture

↓

Implementation

↓

Measurement

↓

Observation

↓

Improvement
```

Do not simply "finish" the lab.

Measure the behavior.

Understand the trade-offs.

---

# Lab 1

## Build Your First Agent Loop

Difficulty

⭐

Time

20 Minutes

---

## Objective

Convert a single-request assistant into an iterative agent.

Architecture

```
Question

↓

Reason

↓

Act

↓

Observe

↓

Need Another Step?

↓

Yes

↓

Repeat
```

---

## Build

Implement

```
Observe

↓

Reason

↓

Act
```

loop.

---

## Measure

Count

- iterations
- tool calls
- completion time

---

## Expected Outcome

The agent performs multiple reasoning cycles instead of one LLM call.

---

# Lab 2

## Planner

Difficulty

⭐⭐

---

Objective

Separate

planning

from

execution.

Architecture

```
Goal

↓

Planner

↓

Execution Plan

↓

Executor
```

---

Measure

Can the planner change

the execution order

without modifying the executor?

---

Expected

Cleaner architecture.

---

# Lab 3

## State Manager

Difficulty

⭐⭐

---

Create

runtime state.

Example

```yaml
goal:
  Explain invoice

status:
  running

invoice:
  loaded

pricing:
  pending
```

---

Update state

after every observation.

---

Measure

Can execution resume

using only

the saved state?

---

Expected

State drives execution.

---

# Lab 4

## Short-term Memory

Difficulty

⭐⭐⭐

---

Implement

conversation memory.

Strategies

- Sliding Window
- Conversation Summary

---

Compare

- token usage
- answer quality
- continuity

---

Expected

Summaries reduce token usage while preserving important context.

---

# Lab 5

## Long-term Memory

Difficulty

⭐⭐⭐

---

Store

user preferences.

Example

```
Preferred Language

↓

English
```

Retrieve

during future conversations.

---

Measure

Personalization quality.

---

Expected

The agent remembers user preferences across sessions.

---

# Lab 6

## Reflection

Difficulty

⭐⭐⭐

---

Pipeline

```
Draft

↓

Reflect

↓

Improve

↓

Return
```

---

Measure

- completeness
- latency
- token usage

---

Question

Does one reflection step improve quality?

---

Expected

Improved quality at the cost of additional latency.

---

# Lab 7

## Human-in-the-Loop

Difficulty

⭐⭐⭐⭐

---

Build

approval workflow.

Example

```
Refund > ₹50,000

↓

Manager Approval

↓

Execute
```

---

Measure

Workflow latency.

---

Expected

Approval becomes a runtime state.

---

# Lab 8

## Human-on-the-Loop

Difficulty

⭐⭐⭐⭐

---

Create

monitoring dashboard.

Track

- hallucinations
- latency
- cost
- tool failures

Configure

alerts.

---

Expected

Humans supervise

instead of approving every action.

---

# Lab 9

## Checkpointing

Difficulty

⭐⭐⭐⭐

---

Save

runtime state.

```
Checkpoint

↓

Crash

↓

Restart

↓

Resume
```

---

Measure

Recovery time.

---

Expected

The agent resumes

instead of restarting.

---

# Lab 10

## Production Agent Runtime

Difficulty

⭐⭐⭐⭐⭐

---

Build

Version 4

of

Invoice Explainability Agent.

Architecture

```
User

↓

Planner

↓

State Manager

↓

Short-term Memory

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

Reflection

↓

Policy Engine

↓

Decision Logs

↓

Response
```

---

Features

✓ Agent Loop

✓ Planner

✓ State

✓ Memory

✓ Reflection

✓ Human Approval

✓ Decision Logs

✓ Observability

---

Deliverable

Production Runtime.

---

# Bonus Lab

## Runtime Replay

Objective

Replay

a completed workflow

from

stored checkpoints.

Architecture

```
Checkpoint

↓

Replay

↓

Inspect

↓

Debug
```

---

Expected

Developers can replay the execution history to understand why the agent made each decision.

---

# Engineering Notebook

Every lab should document

```
Problem

↓

Hypothesis

↓

Architecture

↓

Implementation

↓

Metrics

↓

Observation

↓

Conclusion

↓

Next Iteration
```

This notebook becomes

the engineering journal

for the runtime.

---

# Evaluation Rubric

| Category | Points |
|-----------|-------:|
| Planner | 10 |
| State Management | 15 |
| Memory | 15 |
| Reflection | 10 |
| Runtime Design | 20 |
| Documentation | 10 |
| Observability | 10 |
| Engineering Notebook | 10 |

Total

100

---

# Chapter Completion Checklist

By the end of Chapter 4 you should be able to

✅ Explain the Agent Loop

✅ Design a Planner

✅ Manage State

✅ Implement Short-term Memory

✅ Implement Long-term Memory

✅ Add Reflection

✅ Add Human Approval

✅ Build a Production Agent Runtime

---

# Capstone Challenge

Build

Invoice Explainability Agent

Version 4.

Requirements

```
Agent Loop

+

Planner

+

State Manager

+

Memory

+

Reflection

+

Policy Engine

+

Human Approval

+

Decision Logs

+

Observability
```

Goal

Create

a

stateful

production-style

AI Agent.

---

# Reflection Questions

1.

Why is state

more important

than prompts

for long-running agents?

---

2.

What should

be stored

in

Long-term Memory?

---

3.

When should

Reflection

be enabled?

---

4.

When should

Human-in-the-Loop

replace

Human-on-the-Loop?

---

5.

Which runtime component

was

most difficult

to implement?

Explain

why.

---

# Production Engineering Challenge

Refactor your implementation into independent modules.

```
Planner

↓

State Manager

↓

Memory Manager

↓

Reflection Engine

↓

Tool Router

↓

Runtime Controller

↓

Decision Logs
```

No module should depend directly on the LLM except through well-defined interfaces.

---

# Success Criteria

A successful implementation should be able to:

- Resume after a restart.
- Explain every decision using logged observations.
- Separate reasoning from execution.
- Maintain conversational continuity.
- Enforce policies before executing high-risk actions.
- Support replay for debugging and auditing.
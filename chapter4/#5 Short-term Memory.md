# Short-term Memory

> **Chapter 4 – Agent Runtime**

> *Maintaining conversational continuity during execution.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain what Short-term Memory is.
- Differentiate Short-term Memory from Long-term Memory.
- Understand conversation state.
- Understand memory windows.
- Explain conversation summarization.
- Design memory strategies for production AI systems.
- Build memory-aware AI agents.

---

# Why Short-term Memory Exists

Imagine the following conversation.

User

```
Explain my invoice.
```

Agent

```
Here is your invoice explanation.
```

User

```
What about last month?
```

Question

How does the agent know

what

"last month"

refers to?

Without memory,

it does not.

---

# First Principle

Short-term Memory is

> **The temporary information required to continue the current conversation or workflow.**

It exists only while the interaction is active.

---

# Mental Model

Imagine writing on a whiteboard.

```
Conversation

↓

Whiteboard

↓

Current Discussion
```

As the discussion continues,

new notes are added.

When the meeting ends,

the whiteboard is erased.

That is

Short-term Memory.

---

# Conversation Without Memory

```
User

↓

LLM

↓

Answer

↓

Forget Everything
```

Every message becomes

an isolated request.

---

# Conversation With Memory

```
User

↓

Conversation Memory

↓

Prompt Builder

↓

LLM
```

The model receives

relevant conversation history

with every request.

---

# Short-term Memory vs Context Window

These concepts are related,

but different.

| Short-term Memory | Context Window |
|-------------------|----------------|
| Conversation management | Model limitation |
| Application responsibility | Model capability |
| Can be summarized | Fixed by model design |
| Persists across requests | Exists only during inference |

The application chooses what to include in the context window.

---

# What Belongs in Short-term Memory?

Examples

- Previous user questions
- Previous agent responses
- Clarifications
- Current task
- Intermediate observations
- Pending actions

Short-term Memory should contain only information relevant to the current interaction.

---

# Memory Window

The simplest strategy is

a sliding window.

```
Conversation

↓

Last N Messages

↓

Prompt
```

Advantages

- Simple
- Fast

Limitations

Older context disappears.

---

# Conversation Summarization

Instead of keeping every message,

summarize older interactions.

Architecture

```
Conversation

↓

Summarizer

↓

Summary

↓

Prompt
```

This preserves important information while reducing token usage.

---

# Hybrid Memory

A common production pattern

```
Recent Messages

+

Conversation Summary

↓

Prompt
```

The model receives

both

recent detail

and

longer-term context.

---

# Memory Updates

Every interaction updates memory.

```
Question

↓

Response

↓

Memory Update

↓

Next Request
```

Memory evolves with the conversation.

---

# Running Case Study

Invoice Explainability Agent

Conversation

```
User

Explain invoice.
```

↓

Memory

```
Current invoice discussed
```

User

```
Compare with last month.
```

The agent already knows

which invoice

is being discussed.

---

# Memory Policies

Production systems define rules such as:

- Maximum conversation length
- Summary frequency
- Sensitive data retention
- Memory expiration

Memory should not grow indefinitely.

---

# Token Budget

Every message consumes tokens.

Long conversations eventually exceed the available context window.

Possible solutions

- Sliding window
- Summaries
- Selective memory
- Retrieval of relevant conversation

---

# Engineering Perspective

Short-term Memory is

**conversation state**.

It helps the agent maintain continuity,

not permanent knowledge.

---

# Production Insight

Enterprise memory pipeline

```
Conversation

↓

Memory Manager

↓

Summarizer

↓

Prompt Builder

↓

LLM
```

The Memory Manager decides

what information should remain active.

---

# Failure Modes

## Memory Overflow

↓

Summarize

---

## Irrelevant Memory

↓

Prune

---

## Missing Context

↓

Ask Clarifying Question

---

## Stale Conversation

↓

Reset Session

---

# Engineering Notebook

Experiment.

Create a conversation of

20 turns.

Compare:

1. No memory.
2. Sliding window.
3. Sliding window + summary.

Measure:

- Token usage
- Response quality
- Continuity

Observe how different memory strategies affect the conversation.

---

# Common Misconceptions

## "The LLM remembers everything."

False.

The application supplies conversation history.

---

## "Memory is infinite."

False.

Every production system has practical limits.

---

## "Conversation history should always be included."

False.

Irrelevant history increases cost and may reduce answer quality.

---

## "Short-term Memory replaces Long-term Memory."

False.

They solve different problems.

---

# Best Practices

✅ Keep only relevant conversation.

✅ Summarize older interactions.

✅ Respect token budgets.

✅ Expire stale sessions.

✅ Separate conversation memory from business knowledge.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Short chat | Sliding window | Simplicity |
| Long conversation | Summary + recent history | Balance detail and cost |
| Enterprise support | Memory Manager | Better control |
| Sensitive domains | Memory expiration | Privacy |

---

# Engineering Decision Record (EDR)

## Problem

Need conversational continuity.

## Options

1. No memory.

2. Sliding window.

3. Summary + sliding window.

## Decision

Summary + sliding window.

## Trade-offs

Pros

- Better continuity
- Lower token usage
- Longer conversations

Cons

- Summaries may omit details
- Additional processing

## Recommendation

Treat Short-term Memory as a managed runtime component.

---

# Key Takeaways

- Short-term Memory maintains conversational continuity.
- It is managed by the application, not the LLM.
- Memory windows and summaries help control token usage.
- Conversation state is different from long-term knowledge.
- Production systems manage memory explicitly.

---

# Interview Questions

### Q1

What is Short-term Memory?

---

### Q2

Short-term Memory vs Context Window?

---

### Q3

Why use conversation summaries?

---

### Q4

What is a sliding memory window?

---

### Q5

Why shouldn't entire conversations always be included?

---

### Q6

How would you manage memory for long conversations?

---

### Q7

What belongs in Short-term Memory?

---

### Q8

Draw a production Short-term Memory architecture.

---

# Hands-on Exercise

## Objective

Implement a simple Memory Manager.

### Step 1

Maintain the last five conversation turns.

### Step 2

Summarize older turns.

### Step 3

Include both the summary and recent messages in the runtime prompt.

### Step 4

Compare the result with using the entire conversation.

### Expected Outcome

You should observe that summarization preserves continuity while reducing token usage and improving scalability.

---

# Production Readiness Checklist

☑ Sliding window implemented

☑ Conversation summarization

☑ Memory pruning

☑ Token budget management

☑ Session expiration

☑ Memory metrics

☑ Conversation logging

☑ Privacy policy applied

---

# Further Reading

- LangGraph Documentation
- LangChain Memory concepts
- Microsoft Agent Framework Documentation
- Conversation summarization techniques
- ByteByteGo articles on AI Agent architecture
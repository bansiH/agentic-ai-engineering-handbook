# Tool Selection

> **Chapter 3 – Tools & Function Calling**

> *Choosing the right capability at the right time.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain how AI Agents choose tools.
- Understand Tool Routing.
- Explain Tool Selection strategies.
- Design Tool Registries.
- Build deterministic tool-routing pipelines.
- Explain Tool Selection in interviews.

---

# Why Tool Selection Exists

Suppose an AI Agent has access to

```
get_invoice()

calculate_tax()

book_ride()

cancel_ride()

weather()

calendar()

email()
```

User asks

```
Why is my invoice ₹12,540?
```

Which tool should be called?

Choosing correctly

is

Tool Selection.

---

# First Principle

The LLM does **not**

search

your codebase.

It only reasons over

the tool descriptions,

schemas,

and

the user's request.

If you expose poor tool descriptions,

you should expect poor tool selection.

---

# Engineering Mental Model

Imagine a help desk.

Many specialists exist.

```
Finance

IT

HR

Legal
```

A request arrives.

```
Reset my password.
```

The help desk routes it to

IT,

not

Finance.

Tool Selection works similarly.

---

# Tool Selection Pipeline

```
User Request

↓

Intent Understanding

↓

Candidate Tools

↓

Ranking

↓

Tool Selection

↓

Argument Generation

↓

Validation

↓

Execution
```

Notice

selection happens

before

execution.

---

# How the Model Selects a Tool

The model typically considers:

- Tool name
- Tool description
- Input schema
- User request
- Conversation context
- Previous observations

These signals influence which tool is proposed.

---

# Example

Available tools

```
get_invoice()

get_customer()

weather()

book_ride()
```

User

```
Explain my invoice.
```

Expected selection

```
get_invoice()
```

Not

```
weather()
```

The quality of descriptions strongly affects this outcome.

---

# Tool Descriptions Matter

Poor description

```
Gets information.
```

Good description

```
Retrieve invoice details by invoice ID, including
taxes,
discounts,
currency,
pricing rules
and line items.
```

Which description is easier for the model to understand?

---

# Candidate Tool Filtering

Production systems rarely expose every tool.

Instead

```
User Request

↓

Intent Classifier

↓

Relevant Tool Set

↓

LLM
```

Reducing the number of candidate tools often improves selection quality.

---

# Tool Registry

Enterprise systems usually maintain

```
Tool Registry

↓

Metadata

↓

Descriptions

↓

Schemas

↓

Permissions
```

The LLM reasons over

registered tools,

not arbitrary functions.

---

# Intent → Tool Mapping

Example

| Intent | Tool |
|----------|------|
| Invoice | get_invoice() |
| Refund | refund_payment() |
| Weather | weather() |
| Calendar | create_event() |

Some systems perform deterministic routing before involving the LLM.

---

# Deterministic vs LLM-Based Routing

### Deterministic

```
Intent

↓

Router

↓

Tool
```

Pros

- Predictable
- Fast
- Easy to test

Cons

- Less flexible

---

### LLM-Based

```
User

↓

LLM

↓

Tool
```

Pros

- Flexible
- Handles natural language

Cons

- Requires good tool definitions
- May choose incorrectly

---

# Hybrid Routing

Many enterprise systems combine both approaches.

```
Intent Detection

↓

Known Intent?

──────────────

YES

↓

Deterministic Router

──────────────

NO

↓

LLM Tool Selection
```

This balances reliability and flexibility.

---

# Tool Ranking

Suppose

20 tools are available.

Instead of exposing all 20,

rank the most relevant.

```
20 Tools

↓

Ranker

↓

Top 5

↓

LLM
```

Benefits

- Better accuracy
- Lower prompt size
- Reduced ambiguity

---

# Multiple Tools

Sometimes

one tool

is not enough.

Example

```
Invoice

↓

Pricing

↓

Tax

↓

LLM
```

The agent may need multiple observations before producing a final answer.

Tool orchestration is covered later in this chapter.

---

# Running Case Study

Invoice Explainability Agent

User

```
Why is my invoice ₹12,540?
```

Pipeline

```
Question

↓

Intent

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

The agent selects only the tools required for this task.

---

# Engineering Perspective

Tool Selection is a routing problem.

The objective is

not

"find every tool."

The objective is

"find the smallest set of relevant tools."

---

# Production Insight

Production architecture

```
User

↓

Intent Classifier

↓

Tool Registry

↓

Candidate Tools

↓

LLM

↓

Function Request
```

Notice

the registry and router reduce ambiguity before the LLM reasons.

---

# Failure Modes

## Wrong Tool

↓

Improve descriptions

↓

Improve routing

---

## Too Many Tools

↓

Candidate filtering

---

## Ambiguous Tool Names

↓

Rename tools

↓

Improve metadata

---

## Missing Tool

↓

Fallback

↓

Ask clarifying question

---

# Engineering Notebook

Experiment

Create

three invoice tools

with

poor descriptions.

Observe

selection quality.

Now

improve

the descriptions.

Observe again.

Conclusion

Tool metadata strongly influences model behaviour.

---

# Common Misconceptions

## "The model understands my code."

False.

The model understands

the interface

you expose.

---

## "More tools are always better."

False.

Too many candidate tools increase ambiguity.

---

## "Tool names don't matter."

False.

Names and descriptions guide model selection.

---

## "Selection should always be LLM-driven."

False.

Many enterprise systems combine deterministic routing with LLM reasoning.

---

# Best Practices

✅ Give tools descriptive names.

✅ Write precise descriptions.

✅ Use schemas.

✅ Limit candidate tools.

✅ Prefer hybrid routing where appropriate.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Small tool set | LLM selection | Simpler |
| Hundreds of tools | Candidate filtering | Scalability |
| High-risk actions | Deterministic routing | Reliability |
| Enterprise platform | Hybrid routing | Balance flexibility and control |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable invoice tool selection.

## Options

1. Expose every tool.

2. Deterministic routing.

3. Hybrid routing.

## Decision

Hybrid routing with Tool Registry.

## Trade-offs

Pros

- Better scalability
- Better reliability
- Lower prompt size

Cons

- Additional routing component
- Registry maintenance

## Recommendation

Separate tool discovery from tool execution.

---

# Key Takeaways

- Tool Selection is an engineering routing problem.
- The LLM relies on tool metadata, not source code.
- Candidate filtering improves reliability.
- Hybrid routing is common in enterprise AI systems.
- Tool Registries improve governance and maintainability.

---

# Interview Questions

### Q1

How does an LLM choose a tool?

---

### Q2

Why are tool descriptions important?

---

### Q3

Why shouldn't you expose every available tool?

---

### Q4

Deterministic routing vs LLM routing?

---

### Q5

What is a Tool Registry?

---

### Q6

What is candidate filtering?

---

### Q7

How would you improve poor tool selection?

---

### Q8

Draw an enterprise Tool Selection architecture.

---

# Hands-on Exercise

## Objective

Design a Tool Registry.

### Step 1

Create five tools.

### Step 2

Write descriptive metadata.

### Step 3

Classify user intents.

### Step 4

Implement candidate filtering.

### Step 5

Observe whether the correct tool is selected.

### Expected Outcome

Improved descriptions and candidate filtering should increase tool selection accuracy while reducing unnecessary tool proposals.

---

# Further Reading

- LangChain Tools Documentation
- LangGraph Documentation
- OpenAI Function Calling Documentation
- Anthropic Tool Use Documentation
- Model Context Protocol (MCP)
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI Agent architecture
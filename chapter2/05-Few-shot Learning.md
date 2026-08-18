# Few-shot Learning

> **Chapter 2 – Prompt & Context Engineering**

> *Teach the model by showing examples, not just by giving instructions.*

---

# Learning Objectives

After completing this section, you should be able to:

- Explain Zero-shot, One-shot and Few-shot prompting.
- Understand why examples improve model behavior.
- Select the appropriate prompting strategy.
- Design reusable example libraries.
- Build enterprise prompt examples.
- Avoid common few-shot mistakes.

---

# Why Few-shot Learning Exists

Suppose you ask an LLM:

```
Explain this invoice.
```

The model can certainly answer.

Now suppose you instead show the model

three

high-quality invoice explanations

before asking the question.

What happens?

Usually,

the model begins to imitate

the structure,

style,

and level of detail

of the examples.

That is

Few-shot Learning.

---

# First Principle

LLMs learn

during training.

But they also adapt

during inference

using examples.

The examples

are not changing the model.

They are changing

the context

available to the model.

---

# The Three Prompting Modes

```
Zero-shot

↓

One-shot

↓

Few-shot
```

Each increases the amount of guidance provided.

---

# Zero-shot Prompting

No examples.

Only instructions.

Architecture

```text
Instructions

↓

LLM

↓

Answer
```

Example

```
Explain this invoice.
```

Advantages

- Simple
- Fast
- Low token usage

Limitations

The model chooses

its own

style

format

reasoning.

---

# One-shot Prompting

Provide

one

example.

Architecture

```text
Example

↓

Instructions

↓

Question

↓

LLM
```

The example demonstrates the expected format.

---

# Example

```
Invoice A

↓

Explanation A

↓

Invoice B

↓

Explain
```

The model imitates

Explanation A.

---

# Few-shot Prompting

Provide

multiple

examples.

Architecture

```text
Example 1

↓

Example 2

↓

Example 3

↓

Question

↓

LLM
```

The examples establish

patterns,

not rules.

---

# Engineering Mental Model

Imagine onboarding a new engineer.

Option A

```
Do the task.
```

Option B

```
Here are three completed examples.

Now perform the task.
```

Most people perform better with examples.

LLMs behave similarly.

---

# Why Examples Work

Examples demonstrate:

- structure
- terminology
- output format
- reasoning style
- level of detail

The model infers

the desired behavior

from context.

---

# Invoice Example

Example 1

```
Invoice

↓

Professional explanation
```

Example 2

```
Invoice

↓

Professional explanation
```

User Question

```
Invoice

↓

LLM
```

The response usually resembles

the previous explanations.

---

# Architecture

```
Examples

↓

Prompt Builder

↓

Context

↓

LLM

↓

Consistent Output
```

Examples become part of the runtime prompt.

---

# Example Libraries

Production systems rarely hardcode examples.

Instead

they maintain

Example Libraries.

Architecture

```text
Examples

↓

Prompt Library

↓

Prompt Builder

↓

Runtime Prompt
```

This allows examples to evolve independently.

---

# Selecting Examples

Good examples should be

- correct
- representative
- concise
- diverse
- relevant

Poor examples produce poor outputs.

---

# Static vs Dynamic Examples

Static

```
Stored examples
```

Dynamic

```
Retriever

↓

Similar examples

↓

Prompt
```

Dynamic few-shot prompting adapts examples to the current task.

---

# Enterprise Example

Invoice Explainability Agent.

Instead of using

the same examples

for every customer,

retrieve

similar invoice explanations

based on:

- invoice type
- customer
- geography
- language

This improves consistency while remaining relevant.

---

# Cost Considerations

Examples consume tokens.

Architecture

```text
Examples

↓

More Tokens

↓

Higher Cost
```

Benefits

↓

Better consistency

Engineering trade-off

↓

Quality

vs

Cost

---

# Few-shot vs Fine-tuning

Examples

exist

inside

the prompt.

Fine-tuning

changes

model parameters.

Comparison

| Few-shot | Fine-tuning |
|----------|-------------|
| Fast | Slow |
| No training | Requires training |
| Flexible | Persistent |
| Higher runtime tokens | Lower runtime tokens (for learned behaviors) |

Use the simplest approach that satisfies your requirements.

---

# Running Case Study

Invoice Explainability Agent.

Zero-shot

```
Explain invoice.
```

Few-shot

```
Example A

↓

Explanation

Example B

↓

Explanation

Current Invoice

↓

LLM
```

Notice

the response

becomes

more consistent.

---

# Engineering Perspective

Few-shot prompting is

context engineering,

not model training.

Examples are temporary.

Every request

can use different examples.

---

# Production Insight

A production system might look like:

```text
User Question

↓

Example Retriever

↓

Top 3 Similar Examples

↓

Prompt Builder

↓

LLM
```

This enables adaptive prompting while keeping the model unchanged.

---

# Common Misconceptions

## "Few-shot changes the model."

False.

It changes the context.

---

## "More examples are always better."

False.

Too many examples increase token cost and may distract the model.

---

## "Examples replace instructions."

False.

Instructions and examples complement each other.

---

## "Few-shot eliminates hallucinations."

False.

Examples improve consistency but do not replace grounding or retrieval.

---

# Best Practices

✅ Use representative examples.

✅ Keep examples concise.

✅ Remove outdated examples.

✅ Version your example library.

✅ Evaluate examples periodically.

---

# Architecture Decision Matrix

| Situation | Recommendation | Reason |
|-----------|----------------|--------|
| Simple FAQ | Zero-shot | Lowest cost |
| Consistent formatting | One-shot | Demonstrates output style |
| Domain-specific explanations | Few-shot | Better consistency |
| High-volume production | Dynamic example retrieval | Adaptive and maintainable |

---

# Engineering Decision Record (EDR)

## Problem

Need consistent invoice explanations.

## Options

1. Zero-shot.

2. One-shot.

3. Few-shot.

## Decision

Few-shot with representative invoice examples.

## Trade-offs

Pros

- More consistent output
- Better formatting
- Easier adaptation

Cons

- Higher token usage
- Requires example management

## Recommendation

Treat examples as reusable engineering assets.

---

# Key Takeaways

- Few-shot Learning uses examples to guide model behavior.
- Examples influence context, not model parameters.
- Good examples improve consistency and formatting.
- Dynamic example retrieval scales better than static examples.
- Examples should be managed like production artifacts.

---

# Interview Questions

### Q1

What is Few-shot Learning?

---

### Q2

How is Few-shot different from Fine-tuning?

---

### Q3

When would you choose Zero-shot?

---

### Q4

Why do examples improve consistency?

---

### Q5

Can Too Many examples reduce quality?

Why?

---

### Q6

How would you manage examples in a production system?

---

### Q7

Would you retrieve examples dynamically?

Explain.

---

# Hands-on Exercise

## Objective

Compare Zero-shot, One-shot and Few-shot prompting.

### Step 1

Prepare a synthetic invoice.

### Step 2

Run:

- Zero-shot
- One-shot
- Few-shot (3 examples)

### Step 3

Measure:

- Output consistency
- Structure
- Hallucination tendency
- Token usage

### Step 4

Document your observations.

### Expected Outcome

Few-shot prompting should generally produce more consistent and structured responses, while increasing prompt size and token cost.

---

# Further Reading

- LangChain Prompt Templates
- LangGraph Documentation
- OpenAI Prompt Engineering Guide
- Anthropic Prompt Engineering Guide
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI architecture
- Brown et al., *Language Models are Few-Shot Learners* (GPT-3)
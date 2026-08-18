# Chapter 2 Interview Guide

> **Prompt & Context Engineering**

> *Think like an AI Systems Engineer, not a Prompt Engineer.*

---

# How To Use This Guide

Do **not** memorize answers.

Instead,

learn to answer using

```
Problem

↓

Why

↓

Architecture

↓

Trade-offs

↓

Production Example
```

Interviewers are evaluating

engineering judgement,

not definitions.

---

# Interview Levels

This guide is divided into

```
Level 1

↓

Level 2

↓

Level 3

↓

Level 4
```

Each level corresponds roughly to increasing engineering responsibility.

---

# Level 1

## Fundamentals

---

### Q1

What is Prompt Engineering?

Good Answer

Prompt Engineering is the discipline of designing instructions that reliably guide a language model toward a desired behavior.

Prompt Engineering alone is insufficient for enterprise AI because applications also require context, retrieval, validation, and governance.

---

### Q2

What is Context Engineering?

Good Answer

Context Engineering is the process of selecting, assembling and delivering the information that the model needs to reason over.

Prompts define **behavior**.

Context provides **knowledge**.

---

### Q3

Prompt vs Context?

Expected Answer

| Prompt | Context |
|----------|----------|
| Behavior | Information |
| Usually static | Usually dynamic |
| Defines how | Defines what |

---

### Q4

What is a Prompt Template?

Expected Answer

A reusable prompt structure with variables.

It separates prompt logic from runtime data.

---

### Q5

Why shouldn't prompts be hardcoded?

Expected Discussion

Maintenance

Reuse

Versioning

Testing

PromptOps

---

# Level 2

## Application Engineering

---

### Q6

What is a Prompt Builder?

Expected Answer

A software component that assembles

- template
- variables
- context
- examples
- conversation

into a runtime prompt.

---

### Q7

Why separate System Prompt and User Prompt?

Expected Discussion

Permanent behavior

↓

System Prompt

Runtime question

↓

User Prompt

Cleaner architecture.

---

### Q8

What belongs inside Context?

Expected Discussion

Invoices

Policies

Retriever Results

Tool Outputs

Conversation

---

### Q9

Why use Few-shot Learning?

Expected Answer

Examples improve consistency by demonstrating the expected structure and style.

---

### Q10

Few-shot vs Fine-tuning?

Expected Answer

Few-shot

↓

Context

Fine-tuning

↓

Model parameters

---

### Q11

Why use Structured Outputs?

Expected Answer

Enterprise software consumes structured data more reliably than free text.

---

### Q12

JSON vs Function Calling?

Expected Answer

JSON

↓

Machine-readable response

Function Calling

↓

Machine-readable action

---

# Level 3

## Production AI Systems

---

### Q13

What is PromptOps?

Expected Answer

The operational discipline of managing prompts throughout their lifecycle.

Includes:

- versioning
- testing
- deployment
- monitoring
- rollback

---

### Q14

Why version prompts?

Expected Discussion

Regression detection

Rollback

A/B testing

Governance

---

### Q15

What is Prompt Injection?

Expected Answer

A security attack in which untrusted instructions attempt to influence model behavior.

---

### Q16

How do you defend against Prompt Injection?

Expected Discussion

Layered security

↓

Input validation

↓

Context separation

↓

Output validation

↓

Policy Engine

↓

Human approval

---

### Q17

What are Guardrails?

Expected Answer

Deterministic controls that govern AI behavior before, during and after inference.

---

### Q18

Prompt Guardrails vs Policy Engine?

Prompt Guardrails

↓

Prompt behavior

Policy Engine

↓

Business rules

---

### Q19

How do you reduce hallucinations?

Expected Discussion

- Retrieval
- Better context
- Validation
- Guardrails
- Tool use
- Evaluation

---

### Q20

How do you evaluate prompts?

Possible metrics

- correctness
- groundedness
- hallucination rate
- JSON validity
- latency
- cost

---

# Level 4

## Architecture

---

### Q21

Design a production Prompt Architecture.

Expected Whiteboard

```
Authentication

↓

Prompt Builder

↓

Retriever

↓

Prompt Template

↓

LLM

↓

Validation

↓

Response
```

---

### Q22

Where does Context Engineering happen?

Correct Answer

Before

the LLM.

---

### Q23

Should prompts contain business rules?

Expected Discussion

Permanent behavior

↓

Prompt

Business policy

↓

Policy Engine

---

### Q24

Should prompts enforce authorization?

Expected Answer

No.

Authorization belongs to deterministic application logic.

---

### Q25

Why shouldn't the model decide everything?

Expected Discussion

Reliability

Security

Auditability

Compliance

Repeatability

---

### Q26

Prompt Templates vs Prompt Registry?

Prompt Template

↓

Reusable structure

Prompt Registry

↓

Lifecycle management

---

### Q27

Why separate Prompt Builder from Prompt Template?

Prompt Template

↓

Definition

Prompt Builder

↓

Assembly

---

### Q28

What is Prompt Architecture?

Expected Answer

The collection of software components responsible for constructing runtime prompts.

---

### Q29

Why do enterprise systems use PromptOps?

Expected Discussion

Prompts evolve.

They require

testing,

review,

deployment,

monitoring.

---

### Q30

Prompt Engineering vs Software Engineering?

Expected Answer

Prompt Engineering becomes Software Engineering once prompts are versioned, tested, deployed and monitored as production assets.

---

# Whiteboard Exercises

---

## Exercise 1

Draw

Prompt Lifecycle.

```
Developer

↓

Template

↓

Prompt Builder

↓

Runtime Prompt

↓

LLM
```

---

## Exercise 2

Draw

Prompt Architecture.

---

## Exercise 3

Draw

Prompt Injection Defense.

---

## Exercise 4

Draw

Context Assembly Pipeline.

---

## Exercise 5

Draw

Invoice Explainability Prompt Pipeline.

---

# Common Interview Traps

---

## Trap

Prompt Engineering

=

writing prompts.

Reality

Prompt Engineering

=

designing reliable AI behavior.

---

## Trap

Longer prompts

are better.

Reality

Better architecture

beats

longer prompts.

---

## Trap

Few-shot

changes

the model.

Reality

Few-shot

changes

the context.

---

## Trap

JSON

guarantees

correctness.

Reality

JSON

guarantees

structure.

Not

truth.

---

## Trap

Prompt Injection

is solved

by prompts.

Reality

Prompt Injection

requires

architecture.

---

# Engineering Thought Process

When answering,

always explain

```
Problem

↓

Options

↓

Trade-offs

↓

Decision

↓

Production Impact
```

This demonstrates engineering maturity.

---

# Chapter 2 Self-Assessment

Can you explain

without notes

✅ Prompt Anatomy

✅ Prompt Templates

✅ Context Engineering

✅ Few-shot Learning

✅ Structured Outputs

✅ Prompt Injection

✅ Prompt Guardrails

✅ PromptOps

✅ Production Prompt Architecture

If yes,

you are ready

for

Chapter 3

Tools & Function Calling.

---

# Further Reading

- LangChain Documentation
- LangGraph Documentation
- OpenAI Prompt Engineering Guide
- Anthropic Prompt Engineering Guide
- Microsoft Agent Framework Documentation
- OWASP Top 10 for LLM Applications
- NIST AI Risk Management Framework
- ByteByteGo articles on AI Engineering
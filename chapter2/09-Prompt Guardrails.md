# Prompt Guardrails

> **Chapter 2 – Prompt & Context Engineering**

> *Design AI systems that remain safe, reliable and governable.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain what AI guardrails are.
- Understand why prompts alone are insufficient.
- Design layered guardrail architectures.
- Build input, retrieval, reasoning and output guardrails.
- Explain governance and policy enforcement.
- Apply human approval to high-risk actions.
- Design enterprise-safe AI systems.

---

# Why Guardrails Exist

Imagine building an autonomous car.

Would you rely only on the steering wheel?

Of course not.

You also need:

- brakes
- seat belts
- airbags
- collision avoidance
- speed limits
- cameras
- sensors

Guardrails in AI serve the same purpose.

They reduce risk.

---

# First Principle

A guardrail is

> **A deterministic control that limits, validates or governs AI behaviour.**

Notice

guardrails belong to

the application,

not

the model.

---

# Engineering Mental Model

Without guardrails

```
User

↓

LLM

↓

Action
```

With guardrails

```
User

↓

Input Guardrails

↓

LLM

↓

Output Guardrails

↓

Policy Engine

↓

Action
```

The LLM becomes

one component

inside a controlled system.

---

# Guardrail Layers

A production AI system usually contains multiple layers.

```
Input

↓

Context

↓

Reasoning

↓

Output

↓

Execution
```

Each layer protects a different part of the workflow.

---

# Layer 1 — Input Guardrails

Purpose

Protect the application

before

the LLM is called.

Examples

- Prompt Injection detection
- PII detection
- Malware scanning
- File validation
- Language detection
- Rate limiting

Architecture

```text
User

↓

Input Filter

↓

Prompt Builder
```

---

# Layer 2 — Context Guardrails

Purpose

Protect retrieved information.

Examples

- trusted sources only
- remove duplicated documents
- remove expired policies
- detect malicious retrieved instructions
- verify document ownership

Architecture

```text
Retriever

↓

Context Validator

↓

Prompt Builder
```

---

# Layer 3 — Reasoning Guardrails

Purpose

Limit what the model is allowed to do.

Examples

- allowed tools
- maximum payment amount
- restricted APIs
- no SQL DELETE
- no money transfer

Architecture

```text
LLM

↓

Proposed Action

↓

Policy Engine
```

Notice

The model

proposes.

The application

decides.

---

# Layer 4 — Output Guardrails

Purpose

Validate the generated response.

Examples

- JSON validation
- Schema validation
- Toxicity detection
- PII detection
- Hallucination checks
- Citation validation
- Language policy

Architecture

```text
LLM

↓

Output Validator

↓

Safe Response
```

---

# Layer 5 — Execution Guardrails

The highest-risk layer.

Example

```
Transfer ₹50,000

↓

Human Approval

↓

Execute
```

Never allow

high-risk actions

to execute

without authorization.

---

# Guardrails vs Prompts

Prompt

```
Please don't reveal private information.
```

Guardrail

```
Output Validator

↓

Reject

↓

Mask

↓

Escalate
```

The second approach is deterministic.

---

# Rule-Based Guardrails

Many guardrails

are ordinary software.

Examples

```
IF

Amount > ₹50000

↓

Require Manager Approval
```

No LLM required.

---

# AI-Based Guardrails

Some organizations also use AI models to classify risk.

Examples

- harmful content
- prompt injection
- toxicity
- jailbreak attempts

These complement deterministic rules,

not replace them.

---

# Human-in-the-Loop

High-risk workflows often pause for approval.

Architecture

```text
LLM

↓

Policy Engine

↓

Human Approval

↓

Execute
```

Examples

- payments
- refunds
- contract approval
- legal advice

---

# Human-on-the-Loop

The system acts automatically,

but humans monitor it.

Architecture

```text
Agent

↓

Execution

↓

Monitoring

↓

Human Intervention
```

Suitable for

low-risk,

high-volume tasks.

---

# Running Case Study

Invoice Explainability Agent.

Question

```
Show another company's invoices.
```

Pipeline

```text
User

↓

Authorization

↓

Policy Engine

↓

LLM

↓

Denied
```

Notice

The LLM never decides

whether access is allowed.

---

# Governance

Guardrails are a governance mechanism.

They enforce

- compliance
- privacy
- security
- business rules
- auditability

The model itself should not own these responsibilities.

---

# Policy Engine

Think of the Policy Engine as

the organization's rulebook.

Examples

```
Payment Limit

↓

Approval Required
```

```
Customer Data

↓

Mask PII
```

```
Restricted User

↓

Reject Request
```

The Policy Engine should be deterministic and testable.

---

# Decision Logs

Every important decision should be recorded.

Example

```text
Timestamp

User

Prompt ID

Context Version

Model

Tool Calls

Policy Decision

Validator Result

Final Response
```

Decision logs support:

- auditing
- debugging
- compliance
- incident response

---

# Engineering Perspective

Good AI systems separate:

```
Reasoning

↓

LLM
```

from

```
Control

↓

Software
```

This separation improves reliability and maintainability.

---

# Production Architecture

```
User

↓

Authentication

↓

Authorization

↓

Input Guardrails

↓

Retriever

↓

Context Guardrails

↓

Prompt Builder

↓

LLM

↓

Output Guardrails

↓

Policy Engine

↓

Human Approval

↓

Decision Logs

↓

Response
```

This is a typical layered architecture for enterprise AI.

---

# Engineering Notebook

## Experiment

Create three requests:

1. Normal invoice explanation.
2. Prompt injection attempt.
3. Request containing another customer's data.

Observe:

- Which guardrail blocks the request?
- Does the LLM or the application make the decision?

Conclusion:

Critical safety decisions should be enforced by deterministic application logic.

---

# Common Misconceptions

## "Guardrails are prompts."

False.

Prompts are only one layer.

---

## "The LLM should enforce security."

False.

Security belongs in application logic.

---

## "Validation happens after deployment."

False.

Validation should occur on every request.

---

## "Human approval is always required."

False.

Only high-risk actions generally require approval.

---

# Best Practices

✅ Layer guardrails.

✅ Separate reasoning from control.

✅ Validate inputs.

✅ Validate outputs.

✅ Log policy decisions.

✅ Require human approval for high-risk workflows.

---

# Architecture Decision Matrix

| Situation | Recommended Guardrail | Why |
|-----------|----------------------|-----|
| File Upload | Malware + Content Scan | Prevent unsafe inputs |
| RAG | Source Validation | Prevent context poisoning |
| Tool Calls | Policy Engine | Prevent unauthorized actions |
| JSON Output | Schema Validation | Reliable integration |
| High-value Payments | Human Approval | Reduce financial risk |

---

# Engineering Decision Record (EDR)

## Problem

Need safe invoice explanations.

## Options

1. Prompt only.

2. Prompt + Validation.

3. Layered Guardrail Architecture.

## Decision

Layered Guardrail Architecture.

## Trade-offs

Pros

- Stronger security
- Better governance
- Easier auditing
- Lower operational risk

Cons

- More engineering effort
- Additional infrastructure
- Slightly higher latency

## Recommendation

Treat guardrails as architecture,

not prompt engineering.

---

# Key Takeaways

- Guardrails are deterministic controls around the LLM.
- Layer guardrails across the entire request lifecycle.
- Separate reasoning from policy enforcement.
- Human approval belongs in high-risk workflows.
- Governance is an architectural concern.

---

# Interview Questions

### Q1

What are AI guardrails?

---

### Q2

Why aren't prompts sufficient?

---

### Q3

What are the five major guardrail layers?

---

### Q4

Where should authorization be enforced?

---

### Q5

What is the role of a Policy Engine?

---

### Q6

Human-in-the-Loop vs Human-on-the-Loop?

---

### Q7

Why are Decision Logs important?

---

### Q8

Draw a layered guardrail architecture.

---

# Hands-on Exercise

## Objective

Design a secure invoice explanation pipeline.

### Requirements

- Input validation
- Prompt injection detection
- Context validation
- JSON validation
- Policy Engine
- Human Approval
- Decision Logs

### Deliverable

Draw the architecture and explain the responsibility of each layer.

### Expected Outcome

You should demonstrate that safety comes from layered controls rather than relying on prompt wording alone.

---

# Production Readiness Checklist

☑ Authentication implemented

☑ Authorization implemented

☑ Input validation

☑ Prompt injection detection

☑ Context validation

☑ Output schema validation

☑ Policy Engine

☑ Human Approval workflow

☑ Decision Logs

☑ Monitoring and alerting

---

# Further Reading

- OWASP Top 10 for LLM Applications
- NIST AI Risk Management Framework
- LangChain Documentation
- LangGraph Documentation
- OpenAI Documentation
- Anthropic Documentation
- Microsoft Agent Framework Documentation
- ByteByteGo articles on AI system security and architecture
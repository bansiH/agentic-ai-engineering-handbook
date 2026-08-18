# Human-in-the-Loop (HITL)

> **Chapter 4 – Agent Runtime**

> *Keeping humans in control of high-risk AI decisions.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain Human-in-the-Loop (HITL).
- Understand why AI systems require human approval.
- Identify workflows that require human intervention.
- Design approval pipelines.
- Explain HITL in regulated industries.
- Build production approval architectures.

---

# Why Human-in-the-Loop Exists

Suppose an AI Agent decides

```
Refund ₹500,000
```

Should it immediately execute?

No.

Now imagine

```
Terminate Employee

↓

Approve Loan

↓

Transfer Money

↓

Approve Insurance Claim
```

Would you trust

a probabilistic model

to execute these actions

without review?

Most organizations would not.

---

# First Principle

Human-in-the-Loop is

> **A workflow where an AI Agent pauses execution and waits for human review or approval before continuing.**

The key word is

**pause**.

The workflow cannot continue

until

a human decision is received.

---

# Mental Model

Think about

GitHub Pull Requests.

```
Developer

↓

Code

↓

Review

↓

Approval

↓

Merge
```

The developer writes code.

The reviewer approves.

AI systems operate similarly.

---

# Agent Without HITL

```
Question

↓

Reason

↓

Execute

↓

Done
```

Fast.

Risky.

---

# Agent With HITL

```
Question

↓

Reason

↓

Human Review

↓

Approved?

↓

YES

↓

Execute

↓

Done
```

Slower.

Safer.

---

# Where HITL Fits

Enterprise Agent Runtime

```
Observe

↓

Reason

↓

Plan

↓

Act?

↓

Human Approval

↓

Execute
```

Notice

the approval

occurs

before

the action.

---

# Why HITL Matters

Humans contribute

things the model cannot guarantee.

Examples

- Business judgement
- Ethical review
- Legal interpretation
- Regulatory compliance
- Organizational context

AI assists.

Humans decide.

---

# Typical Approval Scenarios

Common examples include

- Financial refunds
- Contract approval
- Loan decisions
- Medical recommendations
- Legal advice
- Customer compensation
- Production deployments

Not every action needs approval.

Risk determines the workflow.

---

# Approval Policy

Example

```
Refund < ₹5,000

↓

Automatic
```

```
Refund ≥ ₹5,000

↓

Human Approval
```

Business policy,

not the LLM,

defines the threshold.

---

# Approval Workflow

```
Agent

↓

Draft Decision

↓

Policy Engine

↓

Approval Required?

↓

YES

↓

Human

↓

Approve / Reject

↓

Continue
```

---

# Approval Interface

Humans should receive

- proposed action
- supporting evidence
- retrieved documents
- confidence (if available)
- reasoning summary
- audit information

The reviewer should not need to inspect raw prompts.

---

# Running Case Study

Invoice Explainability Agent

Question

```
Refund this invoice.
```

Pipeline

```
Planner

↓

Refund Tool

↓

Policy Engine

↓

Amount > ₹50,000?

↓

YES

↓

Manager Approval

↓

Refund

↓

Decision Log
```

The policy,

not the LLM,

controls escalation.

---

# Approval Outcomes

Possible decisions

```
Approve
```

↓

Continue

---

```
Reject
```

↓

Stop

---

```
Request More Information
```

↓

Agent continues

↓

Collect Evidence

↓

Return

---

```
Delegate
```

↓

Forward

↓

Another reviewer

---

# Human Feedback

The reviewer can improve

future behavior.

Example

```
Rejected

↓

Reason

↓

Evaluation Dataset

↓

Prompt Improvement
```

Human feedback becomes a valuable source for evaluation and system improvement.

---

# Engineering Perspective

Human approval

is

a workflow state.

It is

not

an LLM capability.

The runtime controls

waiting,

timeouts,

notifications,

and

resumption.

---

# State Transition

```
Executing

↓

Waiting for Approval

↓

Approved

↓

Executing

↓

Completed
```

The runtime persists state

while waiting.

---

# Timeout Handling

Suppose

the approver

never responds.

Possible policies

- Escalate
- Cancel
- Notify another approver
- Expire request

Waiting forever is rarely acceptable.

---

# Audit Trail

Every approval should record

- Request ID
- User
- Approver
- Timestamp
- Decision
- Comments
- Evidence reviewed

These records support compliance.

---

# Production Insight

Enterprise approval architecture

```
Agent

↓

Policy Engine

↓

Approval Queue

↓

Human

↓

Decision

↓

State Update

↓

Continue
```

The Approval Queue is a runtime service,

not part of the LLM.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| No approver available | Escalation |
| Approval timeout | Expiration policy |
| Missing evidence | Request more information |
| Conflicting approvals | Multi-level approval policy |
| Lost workflow state | Checkpointing |

---

# Engineering Notebook

Experiment.

Create a mock workflow.

```
Refund ₹1,000
```

↓

Auto-approved.

Now

```
Refund ₹100,000
```

↓

Manager approval required.

Observe

how

workflow states

change.

---

# Common Misconceptions

## "Every AI action needs human approval."

False.

Approval should be proportional to risk.

---

## "The LLM decides whether approval is required."

False.

The Policy Engine determines approval requirements.

---

## "Human approval replaces validation."

False.

Validation,

policy enforcement,

and approval

solve different problems.

---

## "Approval belongs inside prompts."

False.

Approval is runtime workflow logic.

---

# Best Practices

✅ Define approval thresholds.

✅ Separate approval from reasoning.

✅ Persist workflow state while waiting.

✅ Capture reviewer comments.

✅ Audit every approval.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| FAQ chatbot | No HITL | Low risk |
| Invoice explanation | Usually no HITL | Informational |
| Refund approval | HITL | Financial risk |
| Contract generation | HITL | Legal risk |
| Medical recommendation | HITL | Patient safety |

---

# Engineering Decision Record (EDR)

## Problem

Need safe execution of high-risk actions.

## Options

1. Automatic execution.

2. Human approval for all actions.

3. Risk-based Human-in-the-Loop.

## Decision

Risk-based Human-in-the-Loop.

## Trade-offs

Pros

- Better governance
- Regulatory compliance
- Reduced operational risk

Cons

- Higher latency
- Additional workflow complexity
- Human resource requirements

## Recommendation

Require Human-in-the-Loop only where business risk justifies additional review.

---

# Key Takeaways

- Human-in-the-Loop pauses execution until a person reviews the proposed action.
- HITL is controlled by deterministic workflow logic.
- Approval thresholds are business decisions.
- Workflow state must persist during approval.
- Decision logs are essential for governance.

---

# Interview Questions

### Q1

What is Human-in-the-Loop?

---

### Q2

Why do enterprise AI systems use HITL?

---

### Q3

Who decides whether approval is required?

---

### Q4

What information should be presented to an approver?

---

### Q5

How does HITL differ from validation?

---

### Q6

What happens if approval times out?

---

### Q7

Where should workflow state be stored during approval?

---

### Q8

Draw a production Human-in-the-Loop architecture.

---

# Hands-on Exercise

## Objective

Implement a Human Approval workflow.

### Step 1

Define a policy:

- Refunds under ₹5,000 → automatic.
- Refunds ₹5,000 and above → manager approval.

### Step 2

Pause the workflow for approval.

### Step 3

Resume execution after approval.

### Step 4

Record the decision in an audit log.

### Expected Outcome

The agent should pause safely, persist its state, resume correctly after approval, and maintain a complete audit trail.

---

# Production Readiness Checklist

☑ Approval policy defined

☑ Policy Engine integrated

☑ Workflow pause/resume

☑ State persistence

☑ Approval queue

☑ Timeout policy

☑ Audit logging

☑ Notification workflow

☑ Escalation strategy

---

# Further Reading

- NIST AI Risk Management Framework
- Microsoft Agent Framework Documentation
- LangGraph Documentation
- Human-AI Interaction research
- OWASP Top 10 for LLM Applications
- ByteByteGo articles on workflow engines and enterprise architecture
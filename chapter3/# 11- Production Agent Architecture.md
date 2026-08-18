# Production Agent Architecture

> **Chapter 3 – Tools & Function Calling**

> *Designing Enterprise-Grade AI Agents*

---

# Learning Objectives

After completing this section you should be able to:

- Design a production-ready AI Agent.
- Explain every major component in an enterprise agent.
- Understand the Agent Runtime.
- Explain Planner–Executor architecture.
- Understand governance and observability.
- Design for scalability, reliability and security.
- Explain production agent architecture during interviews.

---

# Why Production Agent Architecture Exists

A prototype agent often looks like

```
User

↓

LLM

↓

Answer
```

This is useful for demonstrations.

Enterprise systems require much more.

---

# First Principle

An AI Agent is **not** an LLM.

An AI Agent is

> **A software system that uses an LLM as one component to achieve business goals.**

The LLM reasons.

The application governs.

---

# Enterprise Agent Architecture

```text
                          User
                            │
                            ▼
                  API Gateway / UI
                            │
                            ▼
             Authentication & Authorization
                            │
                            ▼
                    Agent Runtime
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
  Prompt Builder      Planner / Router     Conversation State
        │                   │                   │
        └───────────────┬───┴───────────────────┘
                        ▼
                  Context Builder
        ┌───────────────┼────────────────────┐
        ▼               ▼                    ▼
     Retriever      Tool Router         Memory Store
        │               │                    │
        ▼               ▼                    ▼
   Vector DB      Enterprise Tools     Conversation Memory
                        │
                        ▼
                 Model Router
               (SLM / LLM Selection)
                        │
                        ▼
                      LLM
                        │
                        ▼
               Structured Response
                        │
                        ▼
            Validation & Policy Engine
                        │
         ┌──────────────┼──────────────┐
         ▼                             ▼
 Human Approval                 Auto Execution
         │                             │
         └──────────────┬──────────────┘
                        ▼
                 Response Builder
                        │
                        ▼
       Decision Logs • Metrics • Evaluation
                        │
                        ▼
                       User
```

This architecture separates reasoning from deterministic software responsibilities.

---

# Component 1 – User Interface

Possible entry points:

- Web application
- Mobile application
- Slack / Teams
- Voice assistant
- REST API
- Internal enterprise portal

The interface should remain independent of the agent implementation.

---

# Component 2 – Authentication & Authorization

The application must determine:

- **Who is the user?**
- **What are they allowed to do?**

The LLM should never decide permissions.

---

# Component 3 – Agent Runtime

The Agent Runtime orchestrates the complete workflow.

Responsibilities include:

- Planning
- Context assembly
- Tool execution
- State updates
- Error handling
- Policy enforcement

Think of it as the operating system for the agent.

---

# Component 4 – Prompt Builder

Builds the runtime prompt from:

- System prompt
- Prompt templates
- Retrieved context
- Tool observations
- Conversation history

Prompts should be assembled programmatically rather than written inline.

---

# Component 5 – Planner

The planner decides:

- Is a tool required?
- Which tool?
- In what order?
- Is more information needed?

The planner reasons about the workflow rather than directly executing tools.

---

# Component 6 – Context Builder

The Context Builder gathers information from:

- Retrieval (RAG)
- Tool outputs
- User profile
- Conversation history
- Policies

It supplies the LLM with relevant information while respecting context limits.

---

# Component 7 – Retriever

The retriever performs semantic search over enterprise knowledge.

Typical pipeline:

```text
Question

↓

Embedding

↓

Vector Search

↓

Top-k Documents

↓

Prompt Builder
```

---

# Component 8 – Tool Router

Receives tool requests from the planner.

Responsibilities:

- Route requests
- Validate inputs
- Enforce permissions
- Execute tools
- Collect observations

The LLM proposes tool use; the Tool Router governs execution.

---

# Component 9 – Memory

Different kinds of memory may be used.

- Conversation memory
- User preferences
- Session state

Long-term organizational knowledge belongs in systems such as RAG, not in conversation memory.

---

# Component 10 – Model Router

Different requests may use different models.

Example:

```
Simple FAQ

↓

SLM
```

```
Complex Invoice Analysis

↓

LLM
```

Routing improves cost efficiency.

---

# Component 11 – The LLM

Responsibilities:

- Reasoning
- Planning
- Explanation
- Tool selection

Not responsible for:

- Authorization
- Policy enforcement
- Database access
- Business rule execution

---

# Component 12 – Validation

Validate:

- JSON schema
- Required fields
- Business rules
- Safety constraints

Validation is deterministic.

---

# Component 13 – Policy Engine

Policies define:

- Spending limits
- Approval rules
- Privacy restrictions
- Compliance requirements

Policies should be versioned and testable.

---

# Component 14 – Human Oversight

Two common patterns:

### Human-in-the-Loop

The workflow pauses until a human approves.

### Human-on-the-Loop

The workflow proceeds automatically while humans monitor outcomes.

Choose based on business risk.

---

# Component 15 – Response Builder

Combines:

- Tool observations
- Retrieved evidence
- LLM reasoning
- Structured outputs

Returns the final response to the user.

---

# Component 16 – Observability

Collect metrics such as:

- Latency
- Token usage
- Cost
- Tool success rate
- Retrieval quality
- Error rate

Without observability, operating the agent becomes difficult.

---

# Component 17 – Decision Logs

Every significant decision should be recorded.

Suggested fields:

- Timestamp
- User
- Request ID
- Model
- Tools used
- Policies evaluated
- Latency
- Final response

Decision logs support debugging, governance and compliance.

---

# Running Case Study

Invoice Explainability Agent

```text
User

↓

Authentication

↓

Planner

↓

Invoice Tool

↓

Pricing Tool

↓

Tax Tool

↓

Retriever

↓

Prompt Builder

↓

LLM

↓

Structured Output

↓

Policy Engine

↓

Decision Logs

↓

Response
```

Notice that the LLM is only one step in the overall workflow.

---

# Engineering Perspective

A production AI Agent is:

- Distributed
- Observable
- Governed
- Testable
- Secure

The architecture matters as much as the model.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| Tool timeout | Retry / fallback |
| Invalid prompt | Prompt validation |
| Retrieval failure | Safe fallback |
| Hallucination | Grounding + validation |
| Policy violation | Reject / human approval |
| Model unavailable | Model fallback |

---

# Production Insight

Think of the agent as **three layers**:

```
Reasoning Layer

↓

Control Layer

↓

Execution Layer
```

The LLM belongs in the reasoning layer.

Everything else ensures reliable operation.

---

# Common Misconceptions

## "The LLM is the agent."

False.

The LLM is a component within the agent.

---

## "The LLM should enforce business rules."

False.

Business rules belong in deterministic software.

---

## "Memory replaces RAG."

False.

Conversation memory and knowledge retrieval solve different problems.

---

## "Monitoring is optional."

False.

Production systems require continuous monitoring.

---

# Best Practices

✅ Separate reasoning from execution.

✅ Keep business logic outside the LLM.

✅ Validate every external interaction.

✅ Log decisions.

✅ Monitor continuously.

✅ Evaluate regularly.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Enterprise AI | Layered architecture | Separation of concerns |
| Financial workflows | Human approval | Risk reduction |
| High traffic | Model routing | Cost optimization |
| Regulated domains | Decision logs | Compliance |

---

# Engineering Decision Record (EDR)

## Problem

Need a production-grade Invoice Explainability Agent.

## Options

1. Prompt-only chatbot.

2. Tool-enabled assistant.

3. Layered production agent.

## Decision

Layered production architecture.

## Trade-offs

Pros

- Reliable
- Governable
- Scalable
- Observable

Cons

- Additional infrastructure
- Higher engineering effort

## Recommendation

Treat AI Agents as enterprise software systems, not chatbot wrappers.

---

# Key Takeaways

- Production AI Agents are software systems.
- The LLM is only one component.
- Separate reasoning, control and execution.
- Governance and observability are first-class concerns.
- Layered architectures improve reliability and maintainability.

---

# Interview Questions

### Q1

Draw a production AI Agent architecture.

---

### Q2

What belongs inside the Agent Runtime?

---

### Q3

Why separate Prompt Builder from Context Builder?

---

### Q4

What is the role of the Planner?

---

### Q5

Why use a Model Router?

---

### Q6

Where should business rules be enforced?

---

### Q7

What belongs in Decision Logs?

---

### Q8

How would you design an enterprise Invoice Explainability Agent?

---

# Hands-on Exercise

## Objective

Design a production-ready Invoice Explainability Agent.

### Requirements

- Authentication
- Prompt Builder
- Context Builder
- Retriever
- Tool Router
- Planner
- Model Router
- Validation
- Policy Engine
- Human Approval
- Decision Logs
- Observability

### Deliverable

Produce a component diagram and explain the responsibility of every component.

### Expected Outcome

You should be able to justify each architectural decision and explain why the LLM is only one part of the overall system.

---

# Production Readiness Checklist

☑ Authentication

☑ Authorization

☑ Prompt Builder

☑ Context Builder

☑ Retriever

☑ Tool Router

☑ Planner

☑ Model Router

☑ Validation

☑ Policy Engine

☑ Human Approval

☑ Decision Logs

☑ Observability

☑ Evaluation Pipeline

---

# Further Reading

- LangGraph Documentation
- LangChain Documentation
- Microsoft Agent Framework Documentation
- Model Context Protocol (MCP) Specification
- NIST AI Risk Management Framework
- OWASP Top 10 for LLM Applications
- ByteByteGo articles on distributed systems and AI architecture
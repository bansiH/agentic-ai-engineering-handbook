# Model Context Protocol (MCP)

> **Chapter 3 – Tools & Function Calling**

> *Standardizing how AI models interact with tools, resources and prompts.*

---

# Learning Objectives

After completing this section you should be able to:

- Explain what MCP is.
- Explain why MCP was created.
- Understand MCP Clients and MCP Servers.
- Understand Tools, Resources and Prompts.
- Compare MCP with REST APIs and Function Calling.
- Design an MCP-enabled enterprise architecture.
- Explain MCP during interviews.

---

# Why MCP Exists

Imagine building an AI Agent.

Today you may connect it to

- Jira
- Slack
- GitHub
- Gmail
- SQL
- Salesforce
- Internal APIs

Each integration uses

different APIs,

different authentication,

different data models,

different SDKs.

As the number of integrations grows,

the engineering effort grows rapidly.

A common protocol simplifies this problem.

---

# First Principle

Model Context Protocol (MCP) is

> **A standardized protocol that allows AI applications to discover and interact with external capabilities in a consistent way.**

Notice

MCP is

not

an LLM,

not

a framework,

not

a programming language.

It is a protocol.

---

# Engineering Analogy

Think about web browsers.

Without HTTP

every website

would require

its own communication protocol.

Instead

```
Browser

↓

HTTP

↓

Any Website
```

MCP attempts to provide a similar standard for AI applications.

---

# Before MCP

```
AI Agent

├── GitHub SDK

├── Slack SDK

├── Gmail SDK

├── SQL Driver

├── CRM SDK

└── ERP SDK
```

Every integration is different.

---

# With MCP

```
AI Agent

↓

MCP Client

↓

MCP Servers

├── GitHub

├── Slack

├── Gmail

├── SQL

└── ERP
```

The interaction pattern becomes more consistent.

---

# Core Architecture

```
LLM

↓

Agent

↓

MCP Client

↓

MCP Server

↓

External Capability
```

The LLM reasons.

The MCP Client communicates.

The MCP Server exposes capabilities.

---

# MCP Components

An MCP ecosystem typically consists of:

```
Client

↓

Protocol

↓

Server

↓

Capability
```

Each component has a specific responsibility.

---

# MCP Client

The client is embedded inside the AI application.

Responsibilities

- Discover available capabilities
- Send requests
- Receive responses
- Maintain protocol communication

The client does **not**

implement business logic.

---

# MCP Server

The server exposes capabilities.

Examples

```
GitHub

↓

MCP Server

↓

Repository Search
```

```
SQL

↓

MCP Server

↓

Database Query
```

```
Calendar

↓

MCP Server

↓

Events
```

The server provides a standard interface to those capabilities.

---

# MCP Tools

Tools perform actions.

Examples

```
create_issue()

send_email()

get_invoice()

book_ride()
```

Tools may

- read
- write
- execute

depending on permissions.

---

# MCP Resources

Resources provide information.

Examples

- Documents
- Files
- Databases
- Wikis
- Knowledge Bases

Resources are often read by the model but not modified.

---

# MCP Prompts

Some MCP servers expose reusable prompts.

Examples

```
Invoice Analysis

Travel Approval

Customer Summary
```

Instead of rewriting prompts,

applications can reuse standardized prompt assets.

---

# MCP Communication Flow

```
User

↓

Agent

↓

MCP Client

↓

MCP Server

↓

Tool

↓

Observation

↓

Agent

↓

LLM

↓

Response
```

Notice

the protocol standardizes communication,

not reasoning.

---

# MCP vs Function Calling

Function Calling

↓

One interaction

between

LLM

and

application.

MCP

↓

Standard protocol

for exposing

multiple tools,

resources

and

prompts.

Think of Function Calling as

one capability

that can exist

inside

an MCP-based architecture.

---

# MCP vs REST APIs

REST

```
Client

↓

HTTP

↓

API
```

MCP

```
Agent

↓

Protocol

↓

Capabilities
```

REST focuses on web services.

MCP focuses on AI application interoperability.

The two are complementary.

Many MCP servers will internally call REST APIs.

---

# MCP vs Plugins

Earlier plugin systems often required vendor-specific implementations.

MCP aims to reduce this fragmentation by defining a common protocol.

This encourages portability across supporting AI applications.

---

# Running Case Study

Invoice Explainability Agent

Without MCP

```
Agent

↓

Invoice SDK

↓

Pricing SDK

↓

Tax SDK
```

With MCP

```
Agent

↓

MCP Client

↓

Invoice Server

↓

Pricing Server

↓

Tax Server
```

The interaction model becomes more uniform.

---

# Security

Every MCP capability should still enforce:

- Authentication
- Authorization
- Audit Logging
- Policy Enforcement

MCP standardizes communication.

It does **not** replace security.

---

# Governance

Treat every MCP Server as an enterprise service.

Track:

- Owner
- Version
- Permissions
- Available Tools
- Available Resources
- Change History

This improves maintainability.

---

# Production Insight

Enterprise MCP architecture

```
User

↓

Authentication

↓

Agent

↓

MCP Client

↓

Tool Registry

↓

MCP Servers

↓

Business Systems

↓

Observations

↓

LLM

↓

Response
```

Notice

MCP complements,

rather than replaces,

Tool Registries and Policy Engines.

---

# Failure Modes

| Failure | Mitigation |
|----------|------------|
| Server unavailable | Retry / fallback |
| Permission denied | Safe error |
| Capability not found | Discovery refresh |
| Invalid response | Schema validation |
| Slow server | Timeout |

---

# Engineering Notebook

Experiment.

Imagine your organization has

20 internal services.

Question

Would you rather maintain

20 different SDK integrations

or

one protocol

used consistently?

Write down:

- Advantages
- Disadvantages
- Operational considerations

---

# Common Misconceptions

## "MCP replaces REST."

False.

Many MCP servers internally call REST APIs.

---

## "MCP is an LLM."

False.

It is a protocol.

---

## "MCP provides security."

False.

Security still requires authentication, authorization and policy enforcement.

---

## "MCP automatically builds agents."

False.

Agents use MCP.

MCP does not replace agent orchestration.

---

# Best Practices

✅ Treat MCP Servers as production services.

✅ Version exposed capabilities.

✅ Validate all requests and responses.

✅ Apply authorization before execution.

✅ Monitor latency and availability.

✅ Log every interaction.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Single internal API | Direct integration | Simpler |
| Many heterogeneous systems | MCP | Standardized integration |
| Enterprise platform | MCP + Tool Registry | Governance |
| High-risk workflows | MCP + Policy Engine | Security |

---

# Engineering Decision Record (EDR)

## Problem

Need consistent access to many enterprise systems.

## Options

1. Direct SDK integrations

2. REST APIs only

3. MCP-based integration layer

## Decision

Adopt MCP where a standardized capability layer provides long-term operational value.

## Trade-offs

Pros

- Consistent integration model
- Easier capability discovery
- Better portability
- Reduced integration complexity

Cons

- Additional infrastructure
- Protocol adoption effort

## Recommendation

Treat MCP as an interoperability layer, not as a replacement for application architecture.

---

# Key Takeaways

- MCP is a protocol, not a framework.
- MCP standardizes access to tools, resources and prompts.
- MCP Clients consume capabilities.
- MCP Servers expose capabilities.
- Security and governance remain application responsibilities.
- MCP complements Function Calling, Tool Registries and enterprise architectures.

---

# Interview Questions

### Q1

What is Model Context Protocol (MCP)?

---

### Q2

Why was MCP created?

---

### Q3

What is the difference between an MCP Client and an MCP Server?

---

### Q4

What are MCP Tools?

---

### Q5

What are MCP Resources?

---

### Q6

How is MCP different from REST APIs?

---

### Q7

How is MCP different from Function Calling?

---

### Q8

Draw an enterprise MCP architecture.

---

# Hands-on Exercise

## Objective

Design an MCP-enabled Invoice Explainability Agent.

### Requirements

- MCP Client
- Invoice MCP Server
- Pricing MCP Server
- Tax MCP Server
- Authentication
- Authorization
- Tool Registry
- Decision Logs

### Deliverable

Draw the architecture and describe the responsibility of each component.

### Expected Outcome

You should demonstrate how a protocol-based integration layer simplifies access to multiple enterprise capabilities while preserving security and governance.

---

# Production Readiness Checklist

☑ MCP Client configured

☑ MCP Servers documented

☑ Authentication implemented

☑ Authorization enforced

☑ Tool discovery validated

☑ Schema validation enabled

☑ Audit logging configured

☑ Monitoring and metrics enabled

☑ Versioning strategy defined

---

# Further Reading

- Model Context Protocol (MCP) Specification
- LangGraph Documentation
- LangChain Documentation
- OpenAI API Documentation
- Anthropic Documentation
- Microsoft Agent Framework Documentation
- ByteByteGo articles on distributed systems and AI architecture
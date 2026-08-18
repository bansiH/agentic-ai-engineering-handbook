# Structured Outputs

> **Chapter 2 – Prompt & Context Engineering**

> *From Natural Language to Reliable Machine Contracts*

---

# Learning Objectives

After completing this section you should be able to

- Explain why enterprise systems require structured outputs.
- Differentiate free text from structured responses.
- Understand JSON Mode.
- Understand JSON Schema.
- Understand Function Calling.
- Understand schema validation.
- Build reliable downstream AI workflows.
- Design AI APIs that integrate with software systems.

---

# Why Structured Outputs Exist

Suppose we ask an LLM

```
Explain this invoice.
```

Possible response

```
Your invoice increased because
fuel surcharges changed.
```

Great.

Humans understand it.

Computers don't.

---

# Engineering Problem

Suppose another application needs

Invoice Total.

How should it find it?

Option A

Regular Expressions

Option B

String Parsing

Option C

Hope the wording never changes.

Terrible.

---

# First Principle

Enterprise software prefers

**contracts**

over

free text.

Instead of

```
Invoice Total is ₹12,540.
```

Return

```json
{
  "invoice_total": 12540,
  "currency": "INR"
}
```

Now software can consume it directly.

---

# Mental Model

Think of an API.

REST APIs don't return

paragraphs.

They return

structured data.

LLMs should behave similarly when interacting with software.

---

# Free Text vs Structured Output

Free Text

```
Your invoice increased because fuel prices increased.
```

Structured

```json
{
  "reason": "Fuel surcharge",
  "confidence": 0.94
}
```

The second format is easier to validate, store and automate.

---

# Architecture

```
User

↓

Prompt

↓

LLM

↓

JSON

↓

Validator

↓

Application
```

Notice

Validation happens **after** generation.

---

# JSON Mode

Many modern LLM APIs support a mode that encourages JSON output.

Conceptually:

```
Prompt

↓

LLM

↓

JSON
```

This improves reliability compared with asking for "JSON-like" text, but applications should still validate the response.

---

# JSON Schema

JSON answers one question:

> What data was returned?

JSON Schema answers another:

> What data is allowed?

Example

```json
{
  "type": "object",
  "properties": {
    "invoice_total": {
      "type": "number"
    },
    "currency": {
      "type": "string"
    }
  },
  "required": [
    "invoice_total",
    "currency"
  ]
}
```

The schema becomes the contract.

---

# Engineering Analogy

Think of a database.

The schema defines

- columns
- types
- constraints

Applications rely on that schema.

LLMs should also produce outputs that conform to an agreed structure.

---

# Function Calling

Sometimes

the output

isn't meant for humans.

It is meant to trigger

software.

Example

```
User

↓

LLM

↓

book_ride()

↓

Application
```

The model selects the function.

The application executes it.

---

# Structured Output vs Function Calling

Structured Output

↓

Machine-readable response.

Function Calling

↓

Machine-readable action.

Examples

```
JSON

↓

Display
```

versus

```
Function

↓

Execute
```

---

# Validation

Never trust generated output without validation.

Architecture

```
LLM

↓

JSON

↓

Schema Validation

↓

Valid?

↓

YES

↓

Application

──────────

NO

↓

Retry

↓

Repair
```

Validation protects downstream systems.

---

# Repair Loop

Sometimes

the response is almost correct.

Example

Missing field.

Instead of failing,

many production systems repair the response.

```
LLM

↓

Invalid JSON

↓

Repair Prompt

↓

Valid JSON
```

Repair is often cheaper than restarting the entire workflow.

---

# Strong Typing

Modern AI applications increasingly use typed models.

Example (Python)

```python
from pydantic import BaseModel

class InvoiceExplanation(BaseModel):
    invoice_total: float
    currency: str
    reason: str
```

Instead of parsing text,

the application validates the response against the model.

---

# Enterprise Example

Invoice Explainability Agent

Expected response

```json
{
  "invoice_id": "INV-123",
  "invoice_total": 12540,
  "currency": "INR",
  "reason": "Fuel surcharge",
  "confidence": 0.96
}
```

Every downstream service understands this structure.

---

# Why Engineers Care

Structured outputs improve

- automation
- testing
- monitoring
- reliability
- integration

Free text is difficult to automate.

Structured contracts are easier to evolve and validate.

---

# Production Architecture

```
Prompt Builder

↓

LLM

↓

Structured Output

↓

Schema Validator

↓

Business Validation

↓

Application

↓

Database
```

Notice

there are two layers of validation.

---

# Business Validation

Schema validation checks

```
type

required fields
```

Business validation checks

```
Invoice exists?

Currency supported?

User authorized?
```

Both are necessary.

---

# Running Case Study

Invoice Explainability Agent

Prompt

↓

LLM

↓

JSON

↓

Validator

↓

Policy Engine

↓

Decision Logs

↓

API Response
```

The LLM never writes directly to production systems.

---

# Engineering Perspective

Treat structured outputs

like

public APIs.

Version them.

Document them.

Validate them.

Monitor them.

---

# Common Misconceptions

## "JSON means the response is correct."

False.

It may still contain incorrect facts.

Schema validity is different from factual correctness.

---

## "Structured outputs eliminate hallucinations."

False.

They reduce integration problems,

not reasoning errors.

Grounding and retrieval remain essential.

---

## "Validation belongs inside the LLM."

False.

Validation should be deterministic application logic.

---

## "Free text is sufficient."

Usually not.

Enterprise systems often require predictable machine-readable outputs.

---

# Best Practices

✅ Define schemas before prompts.

✅ Validate every response.

✅ Separate schema validation from business validation.

✅ Retry or repair invalid outputs.

✅ Version contracts.

---

# Architecture Decision Matrix

| Situation | Recommendation | Why |
|-----------|----------------|-----|
| Human-only chat | Free text | Maximum flexibility |
| API integration | JSON Schema | Reliable contracts |
| Triggering actions | Function Calling | Deterministic execution |
| Financial systems | Structured Output + Validation | Safety and auditability |

---

# Engineering Decision Record (EDR)

## Problem

Need reliable invoice responses.

## Options

1. Free text.

2. JSON.

3. JSON Schema + Validation.

## Decision

JSON Schema with deterministic validation.

## Trade-offs

Pros

- Reliable integration
- Easier testing
- Better automation

Cons

- Less flexibility
- Additional engineering effort

## Recommendation

Treat structured outputs as API contracts.

---

# Key Takeaways

- Structured outputs enable reliable software integration.
- JSON is a format; schemas define contracts.
- Validation is mandatory.
- Function Calling is different from structured output.
- Production systems should never rely solely on free text.

---

# Interview Questions

### Q1

Why do production systems prefer structured outputs?

---

### Q2

What is the difference between JSON and JSON Schema?

---

### Q3

What is Function Calling?

---

### Q4

Why is validation necessary?

---

### Q5

What is the difference between schema validation and business validation?

---

### Q6

Would you store free text directly in an automated workflow?

Why or why not?

---

### Q7

How would you recover from invalid JSON?

---

### Q8

Why should structured outputs be versioned?

---

# Hands-on Exercise

## Objective

Build a schema-driven invoice explanation.

### Step 1

Define a JSON Schema.

### Step 2

Prompt the model to produce structured output.

### Step 3

Validate the response.

### Step 4

Introduce an invalid field.

### Step 5

Implement a repair step.

### Expected Outcome

You should observe that schema validation catches structural errors while business validation ensures the data is meaningful.

---

# Further Reading

- JSON Schema Specification
- OpenAPI Specification
- Pydantic Documentation
- LangChain Structured Output Documentation
- LangGraph Documentation
- OpenAI Structured Outputs Documentation
- Anthropic Documentation
- Microsoft Agent Framework Documentation
- ByteByteGo articles on API and AI architecture
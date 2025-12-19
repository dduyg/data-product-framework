# ✅ Data Validation

This document outlines common patterns for validating data structures using libraries like Pydantic, Cerberus, or custom logic. The goal is to ensure data correctness, enforce contracts, and reduce downstream errors.

## 🎯 Goals of Validation
- Catch malformed or incomplete data early
- Enforce schema and business logic consistency
- Prevent silent failures in pipelines
- Enable clearer debugging and logging


## 🧱 Pydantic – Basic Example
```python
from pydantic import BaseModel, EmailStr, conint
from typing import Optional

class User(BaseModel):
    id: int
    email: EmailStr
    age: Optional[conint(ge=0, le=120)]  # Optional, must be 0–120 if present
```

## 🧩 Nested Data Structures
```python
class Address(BaseModel):
    city: str
    postal_code: str

class Customer(BaseModel):
    name: str
    address: Address
```

## ⏱ Runtime Validation Pattern
```python
def validate_payload(payload: dict) -> User:
    try:
        return User(**payload)
    except ValidationError as e:
        log.warning(f"Invalid input: {e}")
        raise
```

Use this pattern in:
- Streaming jobs
- API ingestion endpoints
- Pre-model transformation checks

## 🧰 Alternatives to Pydantic

| Tool                  | Strengths                                |
|-----------------------|-----------------------------------------|
| **Cerberus**          | Lightweight, schema-based                |
| **Marshmallow**       | Supports (de)serialization + validation |
| **Voluptuous**        | Easy syntax for custom rules             |
| **Great Expectations**| In-database or runtime validation        |

## 🔍 Where to Apply Validation

| Stage           | Purpose                                |
|-----------------|---------------------------------------|
| Ingestion       | Validate schema, types, nulls         |
| Transformation  | Assert expected relationships, values |
| Output Serving  | Ensure required fields before exposure|


## ✅ Best Practices
- Use shared schemas across systems
- Keep validation logic close to data entry points
- Prefer declarative schema definitions over ad hoc checks
- Log failures with full traceability
- Write tests for critical validation logic

## 🧪 Schema Example (YAML/Cerberus-style)
```yaml
user:
  type: dict
  schema:
    id:
      type: integer
      required: true
    email:
      type: string
      regex: '^[\\w\\.-]+@[\\w\\.-]+\\.\\w+$'
    age:
      type: integer
      min: 0
      max: 120
```

> Validation isn't just about catching errors — it's about building trust in your data flows.

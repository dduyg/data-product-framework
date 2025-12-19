# 📊 Events Table Schema

This document describes the typical schema structure for an `events` table used in analytics systems.

## 🧱 Table Overview
The `events` table captures discrete actions taken by users or systems (e.g., clicks, views, sign-ins).

## 🧾 Core Fields

| Field Name        | Type      | Description                                  |
|-------------------|-----------|----------------------------------------------|
| `event_id`        | UUID      | Unique identifier for each event             |
| `event_type`      | STRING    | Type of event (e.g., `click`, `purchase`)    |
| `event_timestamp` | TIMESTAMP | When the event occurred                      |
| `user_id`         | STRING    | ID of the user who triggered the event       |
| `session_id`      | STRING    | Session context for the event                |
| `source`          | STRING    | Source of the event (e.g., web, mobile)      |
| `page_url`        | STRING    | URL where the event happened                 |
| `user_agent`      | STRING    | Browser/device info                          |
| `ip_address`      | STRING    | Origin IP address                            |


## 📦 Metadata Fields
| Field Name         | Type      | Description                        |
|--------------------|-----------|------------------------------------|
| `created_at`       | TIMESTAMP | When the event was ingested        |
| `ingestion_batch_id` | STRING | ID for tracing ingestion source    |


## 🔍 Optional Custom Fields
Depending on your product or domain, the following may apply:

| Field Name      | Type   | Description                               |
|-----------------|--------|-------------------------------------------|
| `product_id`    | STRING | Associated product/item                   |
| `experiment_id` | STRING | For A/B testing or experimentation flags  |
| `revenue`       | FLOAT  | Monetary value tied to the event          |


## 🧪 Example Record
```json
{
  "event_id": "a1b2c3d4",
  "event_type": "page_view",
  "event_timestamp": "2025-07-29T10:45:00Z",
  "user_id": "user_123",
  "session_id": "sess_456",
  "source": "web",
  "page_url": "https://example.com/home",
  "user_agent": "Mozilla/5.0",
  "ip_address": "203.0.113.42",
  "created_at": "2025-07-29T10:46:00Z"
}
```

## ✅ Best Practices
- Ensure event_id is globally unique
- Standardize event_type values with a reference table
- Prefer UTC timestamps
- Avoid NULLs in core fields when possible
- Consider column naming conventions (e.g., snake_case)

## 🧭 Purpose
This schema supports behavioral analytics, session tracking, funnel analysis, retention metrics, and more.

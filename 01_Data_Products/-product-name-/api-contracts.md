
# 🔌 API Contracts

This document defines the external interfaces (APIs) through which this data product communicates. It includes endpoint descriptions, input/output schemas, and access control mechanisms.

---

## 1. Overview

| Endpoint               | Method | Purpose                        | Auth Required | Status   |
|------------------------|--------|--------------------------------|----------------|----------|
| `/data/summary`        | GET    | Retrieve processed data        | Yes            | Live     |
| `/data/events`         | POST   | Ingest new raw data            | Yes            | Planned  |
| `/status/healthcheck`  | GET    | Check service availability     | No             | Live     |

> Add/adjust endpoints based on your product.

---

## 2. Input Schema

### `POST /data/events`

**Purpose**: Accepts raw event or transaction records to be processed by the pipeline.

**Request Body Schema**:
```json
{
  "id": "string",
  "timestamp": "ISO8601 datetime",
  "type": "string",
  "source": "string",
  "payload": {
    "field_1": "value",
    "field_2": "value"
  }
}
```

**Validation Rules**:
- `timestamp` must not be in the future.
- `id` must be unique per source.
- `payload` must match expected data types.

**Error Codes**:
- `400` – Invalid input format
- `409` – Duplicate event
- `500` – Processing failure

---

## 3. Output Schema

### `GET /data/summary`

**Purpose**: Returns aggregated or transformed data for consumer use.

**Query Parameters**:
- `date`: optional (default: yesterday)
- `filter`: optional (e.g., category, region)

**Response Format**:
```json
[
  {
    "category": "string",
    "total": "number",
    "count": "integer",
    "timestamp": "ISO8601 datetime"
  }
]
```

**Example Response**:
```json
[
  {
    "category": "electronics",
    "total": 18420.75,
    "count": 120,
    "timestamp": "2025-07-31T00:00:00Z"
  }
]
```

---

## 4. Authentication & Authorization

- **Auth Type**: Token-based (e.g., API key or bearer token)
- **Required Headers**:
  - `Authorization: Bearer <token>`
- **Access Levels**:
  - Read-only: Internal dashboards, data consumers
  - Write access: Trusted services, ingestion pipelines
- **Token Rotation**: Recommended every 30 days

---

## 5. Versioning

- Current version: `v1`
- Semantic versioning: Major changes require `/v2/` and deprecation schedule
- Backward-compatible changes do **not** change version

---

## 6. SLA & Performance Expectations

| Endpoint              | Expected Latency | Availability | Notes                     |
|-----------------------|------------------|--------------|---------------------------|
| `/data/summary`       | < 500ms (P95)     | 99.9%        | Cached for repeat queries |
| `/data/events`        | < 1s (P95)        | 99.5%        | Retry-on-failure enabled  |
| `/status/healthcheck` | < 100ms           | 99.99%       | No auth required          |

---

## 7. Testing & Sandbox

- A sandbox environment is available for consumer testing.
- Test tokens and sample payloads are available on request.
- Contract tests are run automatically before deployment.

---

## 8. Change Management

- All contract changes must be approved and documented.
- Deprecations require a 30-day communication window.
- Consumers will be notified via changelog and stakeholder channels.

---

*This document serves as the source of truth for all external interfaces to this data product. Keep it updated and reviewed regularly.*

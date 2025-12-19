# 🧱 dbt Model Layer

A well-structured dbt model layer enables scalable, modular, and understandable data transformations.

---

## 📐 Core Model Layers

Use a layered approach to separate responsibilities:

| Layer         | Prefix  | Description                                             | Example             |
|---------------|---------|---------------------------------------------------------|---------------------|
| Staging       | `stg_`  | Raw data cleanup, renaming, and type casting            | `stg_users.sql`     |
| Intermediate  | `int_`  | Logic-heavy joins, calculations, and enrichments        | `int_user_metrics`  |
| Fact Tables   | `fct_`  | Event- or transaction-level facts                       | `fct_sales`         |
| Dimension     | `dim_`  | Lookup tables for consistent attributes                 | `dim_customer`      |
| Snapshots     | `snap_` | Slowly changing dimensions or historical versions       | `snap_user_status`  |

---

## 🗂️ Folder Structure

Organize models into directories by layer:

```
models/
├── staging/
│   └── stg_users.sql
├── intermediate/
│   └── int_user_metrics.sql
├── marts/
│   ├── fct/
│   │   └── fct_sales.sql
│   └── dim/
│       └── dim_customer.sql
├── snapshots/
│   └── snap_user_status.sql
```

---

## 🔗 Model Relationships

Data typically flows:

`source → staging → intermediate → marts → BI tools`

**Example:**

```plaintext
raw.users
   ↓
stg_users
   ↓
int_user_metrics
   ↓
fct_user_activity
```

Reference models using `ref()` to maintain DAG integrity:

```sql
SELECT *
FROM {{ ref('int_user_metrics') }}
```

---

## 📋 Example: A Staging Model

```sql
-- models/staging/stg_users.sql

WITH source AS (
  SELECT * FROM {{ source('app_db', 'users') }}
),

renamed AS (
  SELECT
    id AS user_id,
    email,
    created_at,
    is_active
  FROM source
)

SELECT * FROM renamed
```

---

## ✅ Best Practices

- One transformation goal per model
- Use `ref()` for all inter-model dependencies
- Apply naming conventions (`stg_`, `int_`, `fct_`, `dim_`)
- Keep models short and readable
- Add YAML metadata for `description`, `tests`, and `tags`
- Avoid complex subqueries in a single model — break into layers

---

## 📌 Summary

Layering models in dbt makes your DAG clearer, logic reusable, and transformations easier to test and evolve.

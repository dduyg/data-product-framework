# ✅ dbt Tests

Testing in dbt helps ensure data quality, integrity, and trustworthiness throughout your pipelines.

---

## 🔍 Types of dbt Tests

### 1. **Built-in (Generic) Tests**

| Test        | Purpose                                | Example                            |
|-------------|----------------------------------------|------------------------------------|
| `unique`    | Ensure no duplicates                   | `id` fields                        |
| `not_null`  | Ensure required fields are not null    | `email`, `created_at`              |
| `accepted_values` | Validate categorical fields      | `status in ('active', 'inactive')`|
| `relationships`   | Enforce foreign key integrity     | `user_id` links to `users.id`     |

**Example (YAML):**
```yaml
version: 2

models:
  - name: users
    columns:
      - name: id
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['active', 'inactive']
```

---

### 2. **Custom Tests**

Defined in `.sql` files in your `tests/` directory.

**Example:**
```sql
-- tests/no_future_dates.sql

SELECT *
FROM {{ ref('orders') }}
WHERE order_date > CURRENT_DATE
```

Add to YAML:

```yaml
- name: orders
  tests:
    - no_future_dates
```

---

## 🧠 When to Use Tests

- **Staging Models (`stg_*`)**: Focus on `not_null`, `unique`, and `relationships`.
- **Fact Tables (`fct_*`)**: Add data integrity checks (e.g., positive values).
- **Dimension Tables (`dim_*`)**: Ensure clean categorical values.
- **Snapshots**: Use custom tests for SCD correctness.

---

## 🛠️ Test Execution

Run all tests:

```bash
dbt test
```

Run specific tests:

```bash
dbt test --select users
```

---

## 🚦 Best Practices

- Always test primary keys (`unique` + `not_null`)
- Use `relationships` to enforce joins
- Use `accepted_values` to catch dirty data early
- Document and group tests by model
- Add descriptive failure messages to custom tests

---

## 📌 Summary

Testing is a first-class citizen in dbt. Use it to catch problems before they cascade downstream and to document assumptions about your data.

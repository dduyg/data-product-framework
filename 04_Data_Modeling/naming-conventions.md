# 🏷️ Naming Conventions
Standardized naming conventions for tables, fields, and schema layers to improve readability, consistency, and maintainability in data projects.

## 📁 Layer Prefixes
Use prefixes to indicate the modeling layer and purpose of tables.

| Prefix   | Meaning                          | Example               |
|----------|----------------------------------|------------------------|
| `stg_`   | Staging / raw cleaned data       | `stg_orders`          |
| `int_`   | Intermediate transformations     | `int_customer_orders` |
| `dim_`   | Dimension tables                 | `dim_product`         |
| `fct_`   | Fact tables                      | `fct_sales`           |
| `snap_`  | Snapshots of slow-changing data  | `snap_user_status`    |
| `tmp_`   | Temporary or testing tables      | `tmp_validation_test` |


## 🧱 Table Naming Rules
- Use `snake_case`
- Be descriptive but concise
- Avoid reserved words
- Use consistent verb-noun or noun-only patterns

✅ Examples:
- Good: `dim_user_profile`, `fct_site_visits`
- Avoid: `table1`, `data_dump`, `temp_123`


## 🔠 Field Naming Conventions

| Rule                              | Example             |
|-----------------------------------|---------------------|
| snake_case                        | `order_date`        |
| Avoid spaces and special chars    | ✅ `user_id` ⛔ `User ID` |
| Use consistent suffixes for types| `_id`, `_ts`, `_flag` |

### Common Suffixes

| Suffix   | Meaning                 |
|----------|-------------------------|
| `_id`    | Identifier (foreign/primary keys) |
| `_ts`    | Timestamp               |
| `_cnt`   | Count                   |
| `_amt`   | Monetary amount         |
| `_flag`  | Boolean (true/false)    |


## 🧭 Naming Patterns by Context
| Context       | Pattern                         |
|---------------|----------------------------------|
| Events        | `event_<noun>_<action>`         |
| Snapshots     | `snap_<entity>_<granularity>`   |
| Temporary     | `tmp_<purpose>_<user/branch>`   |
| Aggregates    | `agg_<metric>_by_<dimension>`   |


## ✅ Best Practices
- Stick to one naming strategy across all models
- Document custom patterns in a team wiki
- Enforce via code review or linting tools (e.g., `sqlfluff`)
- Make names searchable and self-descriptive

>💡 *Consistent naming accelerates onboarding, reduces bugs, and improves cross-team communication.*

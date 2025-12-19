# 🧱 SQL Modeling Patterns
This document outlines key data modeling approaches such as the star schema and snowflake schema, plus guidance on best practices for scalable, maintainable SQL models.


## ⭐ Star Schema
A central **fact table** connected to multiple **dimension tables**.

### Example:
```
          +------------+
          | dim_date   |
          +------------+
               |
+-----------+  |   +--------------+
| dim_store |--+-->|  fact_sales  |
+-----------+      +--------------+
               |
          +-------------+
          | dim_product |
          +-------------+
```

### Benefits:
- Simple joins
- Good performance in BI tools
- Clear separation of facts and context

## ❄️ Snowflake Schema
A normalized version of star schema. Dimensions are split into sub-dimensions.

### Example:
- `dim_product` → `dim_brand`, `dim_category`
- `dim_location` → `dim_region`

### Tradeoffs:
- More joins → slower queries
- Better storage efficiency
- Enforces data normalization


## 🧰 Common Modeling Layers (e.g., dbt)

| Layer      | Purpose                               |
|------------|----------------------------------------|
| **Staging**| One-to-one raw data sources with renaming/typing |
| **Intermediate** | Transformed logic, joins, filters |
| **Marts**  | Business-level, ready for consumption  |


## 🧪 Naming Conventions
- Use `dim_` prefix for dimension tables
- Use `fact_` prefix for fact tables
- Avoid ambiguous names like `data`, `info`, or `temp`

## 🧱 Fact Table Design
- Use surrogate keys, not natural keys
- Store atomic-level data (one row per event/transaction)
- Include:
  - `date_id`
  - `entity_id` (e.g., product, user, store)
  - metrics (e.g., `revenue`, `units_sold`)


## 📏 Dimension Table Design
- Use slowly changing dimensions (SCD) if attributes evolve over time
- Keep dimension tables thin: just descriptive data
- Common dims: `date`, `product`, `user`, `region`, `channel`


## ✅ Best Practices
- Prefer **wide fact tables**, but **thin dimension tables**
- Use **ISO date formats** and standardized timestamp columns
- Implement **surrogate keys** to manage integrity and evolution
- Avoid circular joins or overlapping dimensions
- Optimize with materializations and indexes (where possible)


> 💡 *Good modeling anticipates how people will query data — model for clarity and consistency, not just storage.*

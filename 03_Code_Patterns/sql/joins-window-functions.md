# 🧩 SQL Patterns
This document outlines practical SQL patterns used in analytics engineering and data transformation pipelines — especially for joins and window functions.


## 🔗 Common JOIN Types

### ✅ INNER JOIN
Returns matching rows from both tables.

```sql
SELECT o.order_id, c.customer_name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id;
```

### 🌐 LEFT JOIN
Returns all rows from the left table, and matched rows from the right.
```sql
SELECT o.order_id, c.customer_name
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id;
```

Use case: Keep all events even if no user is matched.

### ❓ FULL OUTER JOIN
Returns rows when there is a match in either table.
```sql
SELECT a.id, a.value AS a_val, b.value AS b_val
FROM table_a a
FULL OUTER JOIN table_b b ON a.id = b.id;
```

Use case: Compare table sync completeness or audit mismatches.


### 🧱 CROSS JOIN
Cartesian product — all combinations.
```sql
SELECT *
FROM users
CROSS JOIN dates;
```
Use case: Create a calendar grid or fill gaps.

### 🔍 Anti-Join (Exclude Matches)
```sql
SELECT a.*
FROM table_a a
LEFT JOIN table_b b ON a.id = b.id
WHERE b.id IS NULL;
```

Use case: Find unmatched records or detect missing references.


## 🪟 Window Functions

### 🔢 ROW_NUMBER()
```sql
SELECT *,
  ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY created_at DESC) AS rownum
FROM transactions;
```

Use case: Keep latest record per customer.

### 📊 RANK() / DENSE_RANK()
```sql
SELECT *,
  RANK() OVER (PARTITION BY category ORDER BY sales DESC) AS rank
FROM products;
```

Use case: Rank items within segments.

### 📈 LAG() / LEAD()
```sql
SELECT 
  user_id,
  event_date,
  LAG(event_d```sqlate) OVER (PARTITION BY user_id ORDER BY event_date) AS prev_event
FROM user_events;
```

Use case: Time between events, churn analysis.

### 📐 SUM() OVER ()
```sql
SELECT 
  customer_id,
  order_amount,
  SUM(order_amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS cumulative_spend
FROM orders;
```

Use case: Running totals or cumulative metrics.

# ✅ Best Practices
- Always specify PARTITION BY and ORDER BY for clarity in window functions.
- Be explicit with JOIN conditions to avoid cross joins.
- Use CTEs for readability with layered logic.
- Validate row counts before and after joins.

> 💡 Window functions are like SQL’s answer to stateful transformations — lean on them when aggregating with memory across rows.

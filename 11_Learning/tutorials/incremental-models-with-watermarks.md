# 💧 Incremental Models with Watermarks

Learn how to implement incremental models using **watermark strategies** to efficiently process only new or changed data.

## 🧠 Why Use Incremental Models?

In large data environments, recomputing entire datasets is inefficient. Incremental models improve performance by:

- Reducing compute cost
- Minimizing I/O
- Supporting near-real-time updates

---

## 🗓️ What is a Watermark?

A **watermark** is a marker—typically a timestamp or unique ID—that helps determine where to resume processing.

Watermarks are used to:

- Track the "last seen" data
- Prevent duplication
- Maintain idempotency

---

## 🏗️ Example Use Case: dbt Incremental Model with Watermark

```sql
{{ config(
    materialized='incremental',
    unique_key='event_id'
) }}

SELECT *
FROM raw.events
{% if is_incremental() %}
  WHERE event_timestamp > (SELECT MAX(event_timestamp) FROM {{ this }})
{% endif %}
```

📝 In this example:

- `event_timestamp` is the watermark
- Only rows with newer timestamps are processed

---

## ✅ Best Practices

- **Choose the right watermark**: Use a timestamp, ID, or version that is always increasing and reliable.
- **Backfill carefully**: Always validate your logic before changing a watermark to avoid data gaps or duplication.
- **Log watermark progress**: Store it in a control table or metadata store.
- **Combine with deduplication**: Use `ROW_NUMBER()` or similar techniques to avoid overlap.

---

## 🔐 Watermarks with CDC (Change Data Capture)

For CDC-enabled systems, you may need to use:

- `op_ts` (operation timestamp)
- `updated_at` fields
- `sys_change_version` (SQL Server)

Example:

```sql
SELECT *
FROM source_data
{% if is_incremental() %}
  WHERE updated_at > (SELECT MAX(updated_at) FROM {{ this }})
{% endif %}
```

---

## ⚠️ Common Pitfalls

- Clock skew or time zone issues
- Non-monotonic timestamps (e.g. updates with old dates)
- Late-arriving data (consider buffer windows)
- Watermark drift if data is missed or skipped

---

## 🧪 Test Strategy

- Validate against full refresh
- Use assertions for monotonicity and completeness
- Compare row counts before/after watermark

---

## 📚 References

- [dbt Docs — Incremental Models](https://docs.getdbt.com/docs/build/incremental-models)
- [Apache Beam — Watermarks](https://beam.apache.org/documentation/programming-guide/#watermarks)
- [Delta Lake — Incremental Processing](https://docs.delta.io/latest/delta-update.html)

> Stay fresh, stay incremental 🚰
>

# 🪜 Incremental Data Loading with Merge (Upsert) Logic in SQL

## Overview

Efficiently ingesting incremental data while keeping your target tables up-to-date is key in data engineering. This tutorial covers a robust SQL pattern for performing incremental loads with merge/upsert semantics, handling inserts, updates, and deletes in a single atomic operation.

## 🤔 Why Use Merge (Upsert)?
- Avoid full table rewrites.
- Handle changes in source data (new rows, updates, deletes).
- Maintain data consistency and reduce downtime.

## 🌐 Core Concept
Use SQL `MERGE` (or equivalent UPSERT statement) to:

- Match source and target rows on a unique key.
- Update target rows when source data changes.
- Insert new rows from the source.
- Optionally delete or archive rows missing in the source.

## Example SQL Pattern

```sql
MERGE INTO target_table AS tgt
USING staging_table AS src
ON tgt.id = src.id
WHEN MATCHED AND (
    tgt.col1 <> src.col1 OR
    tgt.col2 <> src.col2
) THEN
    UPDATE SET
        col1 = src.col1,
        col2 = src.col2,
        updated_at = CURRENT_TIMESTAMP
WHEN NOT MATCHED THEN
    INSERT (id, col1, col2, created_at)
    VALUES (src.id, src.col1, src.col2, CURRENT_TIMESTAMP)
WHEN NOT MATCHED BY SOURCE THEN
    DELETE;

```

## 🗝 Key Points
- **Match condition:** Use the primary key or unique identifier.
- **Change detection:** Compare columns to avoid unnecessary updates.
- **Insert new:** Add rows present in source but missing in target.
- **Delete or archive:** Remove or flag rows missing from source, if needed.

## 🧬 Adaptations

- For data warehouses without `MERGE`, simulate with transactional `UPDATE` + `INSERT` statements.
- Use soft deletes (e.g., `is_active` flag) instead of physical deletes.
- Batch incremental loads during off-peak times for heavy data.

## 📍 Summary

Implementing incremental loading with merge/upsert logic minimizes data staleness and optimizes pipeline efficiency, especially critical for large and frequently changing datasets.

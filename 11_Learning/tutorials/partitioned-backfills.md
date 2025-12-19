# 🧩 Partitioned Backfills
Partitioned backfills are an essential data engineering strategy for correcting or recomputing historical data in a scalable and safe way.


## Why Partitioned Backfills?
Instead of reprocessing the entire dataset (which can be inefficient and expensive), we selectively backfill data by logical partitions—such as by date, customer_id, region, or other dimensions.


## 🗂️ Common Use Cases
- Reprocessing due to schema changes (e.g. new field added)
- Fixing bugs in transformations
- Backfilling newly onboarded data sources
- Adjusting for late-arriving data


## 🏗️ How to Design a Partitioned Backfill

### Step 1: Identify the Partition Key

- Most commonly: `event_date`, `ingestion_date`, or `batch_id`
- Must be available and indexable

### Step 2: Determine the Backfill Range

- Use audit logs, freshness dashboards, or anomaly reports
- Prefer the smallest effective range

### Step 3: Automate Backfill by Partition

For example, using dbt:
```bash
dbt run --select my_model --vars '{"start_date": "2024-01-01", "end_date": "2024-01-15"}'
```

Or using Airflow with dynamic task mapping:
```python
@task
def backfill_partition(date_str):
    return f"Backfilling for {date_str}"

dates = [str(d) for d in pd.date_range("2024-01-01", "2024-01-15")]

backfill_partition.expand(date_str=dates)
```

## ✅ Best Practices
- Backfill oldest to newest to maintain temporal consistency
- Monitor compute cost if partitions are large
- Use audit tables to validate success
- Separate DAG or script for backfills to reduce interference with production

## ⚠️ Watch Out For
- Downstream dependencies triggering unintended loads
- Missing partitions causing silent failures
- Conflicting historical data when joining with non-partitioned datasets

## 🧪 Bonus Tip: Verify with Checksums
After a backfill, compare checksums or row counts across partitions to ensure consistency:
```sql
SELECT 
  partition_date, 
  COUNT(*) AS row_count, 
  SUM(hash_column) AS checksum
FROM my_model
GROUP BY partition_date;
```

## 📚 References

- [dbt Docs: Using variables in backfills](https://docs.getdbt.com/docs/build/customizing-your-models/using-variables)
- [Airflow Docs: Dynamic Task Mapping](https://airflow.apache.org/docs/apache-airflow/stable/concepts/dynamic-task-mapping.html)
- [Best Practices for Managing Backfills](https://medium.com/@your-data-team/best-practices-for-backfills-in-data-pipelines-123abc456def)

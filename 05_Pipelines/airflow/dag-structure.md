# 🛠️ Airflow DAG Structure

This document outlines conventions and best practices for defining and maintaining DAGs (Directed Acyclic Graphs) in Apache Airflow.

---

## 📐 DAG Structure Overview

A DAG should reflect logical task groupings, readable dependencies, and consistent naming conventions.

```
ETL_DAG
├── extract_data
├── transform_data
├── load_data
└── notify_completion
```

---

## 🧩 Task Dependencies

Define clear upstream/downstream relationships:

```python
extract_data >> transform_data >> load_data >> notify_completion
```

Use `TaskGroup` for logical grouping and readability:

```python
with TaskGroup("transform_tasks") as transform_group:
    clean = PythonOperator(...)
    enrich = PythonOperator(...)
    clean >> enrich
```

---

## 🏷️ Naming Conventions

| Element        | Pattern                        | Example                    |
|----------------|--------------------------------|----------------------------|
| DAG ID         | `team_domain_purpose`          | `data_team_daily_etl`      |
| Task ID        | `verb_object`                  | `extract_orders`           |
| Filename       | `dag_<name>.py`                | `dag_user_metrics.py`      |
| Variables/XCom | snake_case                     | `last_successful_run`      |

---

## ⏱️ Scheduling Patterns

| Schedule Interval | Use Case                  |
|-------------------|---------------------------|
| `@hourly`         | Near-real-time sync       |
| `@daily`          | Daily batch pipelines     |
| `@weekly`         | Reports, slow-moving data |
| Custom cron       | Business-aligned schedules|

Use `start_date` and `catchup=False` to control backfills.

---

## ⚙️ Default DAG Arguments

Include common args like:

```python
default_args = {
    'owner': 'data-team',
    'depends_on_past': False,
    'retries': 2,
    'retry_delay': timedelta(minutes=5),
    'email_on_failure': True,
}
```

---

## ✅ Best Practices

- Keep DAG files small and modular
- Use custom operators/hooks where reusable
- Validate DAGs with `airflow dags list` and `airflow tasks test`
- Tag DAGs for observability and ownership
- Always test DAGs locally before deployment

---

## 🧭 Summary

A well-structured DAG enhances readability, scalability, and reliability. Use consistent patterns, clear dependencies, and thoughtful scheduling to build maintainable Airflow workflows.

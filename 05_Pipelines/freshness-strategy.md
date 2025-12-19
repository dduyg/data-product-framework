# ⏱️ Data Freshness Strategy

This document outlines how we ensure data remains up-to-date, timely, and fit for operational or analytical use.

## 🎯 Goals
- Minimize latency between source and warehouse
- Define expectations for data update frequency
- Detect and alert on freshness issues proactively

## 🔁 Refresh Cadence

| Data Type         | Frequency    | Example Source               |
|-------------------|--------------|-------------------------------|
| Real-time events  | Streaming    | Kafka, Pub/Sub               |
| Operational data  | Hourly       | CRM, production DBs          |
| Analytical facts  | Daily        | Sales, marketing dashboards  |
| External sources  | Weekly/Monthly | APIs, third-party exports |

## 📐 Freshness SLAs
Define acceptable data staleness per domain or table.

| Table Name        | SLA Max Age | Criticality  |
|-------------------|-------------|--------------|
| `fact_orders`     | 1 hour      | High         |
| `dim_users`       | 1 day       | Medium       |
| `ext_weather_api` | 1 week      | Low          |


## 🔍 Monitoring Freshness
- **Airflow SLAs** on tasks and DAGs
- **dbt sources** with `loaded_at_field` + `freshness` checks:
  
```yaml
sources:
  - name: crm
    tables:
      - name: customers
        freshness:
          warn_after: { count: 6, period: hour }
          error_after: { count: 12, period: hour }
        loaded_at_field: _loaded_at
```

- Dashboards: Custom visualizations to track staleness
- Alerts: Slack/Email notifications for missed updates

## ⚠️ Common Issues

| Symptom              | Possible Cause                  |
|----------------------|----------------------------------|
| No new data in table | Ingestion pipeline failed        |
| Outdated timestamp   | Source system is lagging         |
| Partial refresh      | Late-arriving or dropped data    |


## 🛠️ Best Practices
- Use `_loaded_at` or ingestion timestamp fields
- Schedule based on data availability, not just time
- Document expected lags and tolerances
- Make freshness visible to consumers
- Backfill late-arriving data if needed

## ✅ Summary
A robust freshness strategy improves trust in your data. By defining SLAs, implementing checks, and alerting early, we can ensure our data stays timely and reliable.

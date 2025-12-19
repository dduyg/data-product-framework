# ❌ Failures and Retries

Guidelines for handling task failures and implementing retry policies in Airflow DAGs.

---

## 🔁 Retry Policy Basics

Use Airflow’s built-in retry settings in `default_args` or task-level overrides:

```python
default_args = {
    'retries': 3,
    'retry_delay': timedelta(minutes=5),
    'retry_exponential_backoff': True,
    'max_retry_delay': timedelta(minutes=30),
}
```

| Setting                   | Description                              |
|---------------------------|------------------------------------------|
| `retries`                | Number of retry attempts                 |
| `retry_delay`            | Time between retries                     |
| `retry_exponential_backoff` | Uses exponential wait time between retries |
| `max_retry_delay`        | Caps the exponential delay               |

---

## ⚠️ Common Failure Types

| Failure Type       | Example                                   | Response Strategy              |
|--------------------|-------------------------------------------|--------------------------------|
| Transient Error    | Network blip, timeout                     | Retry with backoff             |
| Persistent Error   | Code bug, wrong path                      | Fail fast, notify              |
| External System    | Downstream API failure                    | Sensor or alert, retry later   |
| Resource Limit     | Memory or compute quota exceeded          | Auto-retry or resource tuning  |

---

## 🛡️ Failure Handling Strategies

- **Fail Fast:** Fail immediately on known critical errors
- **Graceful Degradation:** Skip or log non-blocking failures
- **Timeouts:** Use `execution_timeout` to limit runtime
- **Alerts:** Use `on_failure_callback` to trigger alerts (Slack, Email, PagerDuty)
- **Retries with Alerts:** Combine retries with alert escalation on final failure

---

## 🔔 Notification Hooks

Example using Slack alert on failure:

```python
def notify_slack(context):
    task = context.get('task_instance')
    slack_message = f"Task {task.task_id} failed in DAG {task.dag_id}"
    send_slack_message(slack_message)

default_args = {
    'on_failure_callback': notify_slack
}
```

---

## ✅ Best Practices

- Use retries for transient, not logical, failures
- Always log reasons for failure and retry
- Use alerting for critical path failures
- Test retry scenarios locally
- Monitor failure frequency to identify flaky tasks

---

## 📌 Summary

A strong retry and failure handling strategy ensures workflow resilience and improves data reliability. Use exponential backoff, failure callbacks, and targeted retries for robust DAGs.

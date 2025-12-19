# 🧾 Logging Patterns

Consistent and structured logging strategies that support observability, debugging, and compliance across data applications.

## 🧱 Logging Basics
Use Python's built-in `logging` module for flexibility and compatibility.

```python
import logging

logger = logging.getLogger(__name__)
logger.setLevel(logging.INFO)

handler = logging.StreamHandler()
formatter = logging.Formatter(
    '%(asctime)s — %(name)s — %(levelname)s — %(message)s'
)
handler.setFormatter(formatter)
logger.addHandler(handler)
```

## 🧪 Log Levels

| Level     | Use Case                          |
|-----------|-----------------------------------|
| DEBUG     | Detailed internal diagnostics     |
| INFO      | General app flow (e.g. steps)     |
| WARNING   | Recoverable issues                |
| ERROR     | Failures that don't stop app      |
| CRITICAL  | System-wide failure               |

Use levels intentionally — avoid using `print()` in production code.


## 🧬 Structured Logging
Format logs as JSON for better parsing and integration with tools like Datadog, ELK, or CloudWatch.

```python
import json
import logging

class JsonFormatter(logging.Formatter):
    def format(self, record):
        return json.dumps({
            "timestamp": self.formatTime(record),
            "level": record.levelname,
            "logger": record.name,
            "message": record.getMessage()
        })

handler = logging.StreamHandler()
handler.setFormatter(JsonFormatter())
logger.addHandler(handler)
```

## 🔁 Contextual Logging
Inject dynamic context such as job IDs, DAG names, or request IDs.
```python
logger.info("Job started", extra={"job_id": job_id})
```
Or use contextual loggers via structlog, loguru, or similar libraries.

## 📚 File vs Stream Logging
- StreamHandler: logs to stdout (useful for containers)
- FileHandler: logs to local files (useful for debugging or audits)
- Use RotatingFileHandler for log file rotation

## 🪤 Logging in Pipelines
Log start/end of every major step:
```python
logger.info("Starting data load from source X")
# load data
logger.info("Finished loading X — rows: %d", len(df))
```

Use consistent keys for metrics: status, duration, row_count, etc.

## ✅ Best Practices
- Centralize log config in one module (logging_config.py)
- Include correlation/request IDs where possible
- Avoid logging sensitive data
- Use structured formats for searchability
- Emit logs in UTC timestamps

> 💡 Logs are not just for debugging — they’re core signals for monitoring, alerting, and auditability.

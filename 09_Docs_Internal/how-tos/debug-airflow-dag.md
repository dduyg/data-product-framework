# 🛠️ How-To: Debug Airflow DAG

A guide to common issues and steps to debug Airflow DAGs efficiently.

---

## 🎯 Objectives

- Quickly identify and resolve DAG execution issues.
- Understand Airflow logs and error messages.
- Ensure stable and reliable pipeline runs.

---

## 🔍 Common Issues

- **Task failures**
  - Code errors or exceptions in task logic.
  - Missing dependencies or environment variables.
- **Scheduler problems**
  - DAG not triggering or stuck in `queued` state.
  - Scheduler not running or overwhelmed.
- **Resource constraints**
  - Insufficient worker slots or memory.
  - Database connection limits reached.
- **Version mismatches**
  - Incompatible Airflow or package versions.
- **Configuration errors**
  - Incorrect DAG scheduling intervals.
  - Misconfigured retries or SLA settings.

---

## 🛠️ Debugging Steps

1. **Check the Airflow UI**
   - Review DAG status, task statuses, and logs.
   - Identify which tasks failed or were skipped.

2. **Examine task logs**
   - Use UI or CLI to access detailed logs.
   - Look for stack traces or error messages.

3. **Verify environment and dependencies**
   - Confirm required environment variables are set.
   - Check package versions and dependencies.

4. **Test task code locally**
   - Run task scripts outside Airflow to isolate issues.

5. **Inspect DAG definition**
   - Review DAG scheduling parameters and dependencies.
   - Ensure correct imports and syntax.

6. **Check Airflow scheduler and worker health**
   - Ensure scheduler and worker services are running.
   - Restart services if needed.

7. **Review resource availability**
   - Monitor CPU, memory, and DB connections.
   - Adjust Airflow configurations if needed.

---

## 🧰 Helpful Tools

- Airflow CLI commands (`airflow tasks test`, `airflow dags list`)
- Log aggregation tools (e.g., ELK stack, Cloud logging)
- Monitoring dashboards for Airflow metrics

---

## 📌 Summary

Systematic debugging using logs, environment checks, and task isolation will help maintain reliable Airflow DAGs and minimize downtime.

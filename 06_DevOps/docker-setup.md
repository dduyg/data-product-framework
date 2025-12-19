# 🐳 Docker Setup

Standard practices for setting up local and production containers using Docker and `docker-compose`.

---

## 🧱 Project Structure

A typical data project might use Docker to containerize Airflow, dbt, or custom Python services.

```
project-root/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── dbt_project/
│   └── ...
├── dags/
│   └── example_dag.py
└── scripts/
    └── bootstrap.sh
```

---

## 📄 Dockerfile Example

```dockerfile
FROM python:3.10-slim

# Set workdir
WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy code
COPY . .

CMD ["python", "main.py"]
```

---

## 🧩 docker-compose.yml Example

```yaml
version: '3.9'

services:
  dbt:
    build: .
    volumes:
      - .:/app
    working_dir: /app/dbt_project
    command: ["dbt", "run"]
    environment:
      - DBT_PROFILE_DIR=/app/.dbt
    depends_on:
      - postgres

  postgres:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: analytics
    ports:
      - "5432:5432"
    volumes:
      - pg_data:/var/lib/postgresql/data

volumes:
  pg_data:
```

---

## 🛠️ Local Dev Tips

- Use `.env` files to manage secrets and credentials.
- Mount local volumes for real-time code updates.
- Set up `entrypoint.sh` or `bootstrap.sh` scripts for bootstrapping services.
- Use `depends_on` for container orchestration but add healthchecks for production robustness.

---

## 🔐 Secrets Management

Avoid hardcoding secrets in Dockerfiles. Use:

- `.env` + `docker-compose` env vars
- Runtime secret injection via CI/CD
- Tools like HashiCorp Vault, AWS SSM, Doppler

---

## ✅ Best Practices

- Keep images slim (`alpine`, `slim`)
- Use multi-stage builds for production
- Pin image versions for consistency
- Cache Python dependencies
- Avoid running as root inside containers

---

## 📌 Summary

Docker standardizes development and deployment environments across teams. A clean setup with `docker-compose` enables fast onboarding, local testing, and smooth CI/CD integration.
# 🔐 Secrets Management

Handling secrets securely across different environments is critical to protect sensitive data like API keys, database passwords, and tokens.

## 🔑 Types of Secrets
- API keys
- Database credentials
- OAuth tokens
- Encryption keys
- Certificates

---

## 🛡️ Best Practices

- **Never commit secrets to version control** (e.g., Git).
- Use environment variables or secret managers to inject secrets at runtime.
- Encrypt secrets at rest and in transit.
- Rotate secrets regularly.
- Use least privilege access controls.

---

## 🔧 Tools & Techniques

| Tool / Method               | Description                                   |
|----------------------------|-----------------------------------------------|
| Environment Variables       | Inject secrets dynamically in CI/CD or runtime|
| Vault (HashiCorp Vault)     | Centralized secret storage with access policies|
| Cloud Secret Managers       | AWS SSM Parameter Store, Azure Key Vault, GCP Secret Manager |
| Encrypted files            | Store encrypted files checked into repos, decrypted at runtime |
| Kubernetes Secrets          | Kubernetes native secret management            |

---

## 📝 Workflow Example

1. Store secrets in a secure vault or cloud secret manager.
2. CI/CD pipeline fetches secrets during build or deploy stages.
3. Inject secrets as environment variables or config files in containers or services.
4. Applications access secrets securely at runtime.
5. Monitor and audit secret access.

---

## ⚠️ Common Pitfalls

- Hardcoding secrets in source code.
- Checking secrets into public or shared repositories.
- Using weak or static passwords.
- Poorly managing secret rotation and expiration.
- Over-permissioning access to secrets.

---

## 🔄 Rotation & Revocation

- Automate secret rotation with tools or scripts.
- Immediately revoke compromised secrets.
- Use short-lived tokens where possible.

---

## 📌 Summary

Effective secrets management reduces risk of data breaches and ensures compliance. Use dedicated tools and follow security best practices to handle secrets safely across all environments.

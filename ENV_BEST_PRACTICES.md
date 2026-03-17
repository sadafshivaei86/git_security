# Environment Variable Best Practices

This document covers how to safely use `.env` files and ensure they never end up in version control.

---

## What Is a `.env` File?

A `.env` file stores **environment-specific configuration** as key-value pairs. It allows you to change application behaviour (API keys, database URLs, feature flags) without changing code.

```env
DATABASE_URL=postgres://user:password@localhost:5432/mydb
API_KEY=sk-abc123xyz
DEBUG=true
```

---

## ✅ Best Practices

### 1. Always Add `.env` to `.gitignore`
This is the most important step. Do it **before your first commit**.

```gitignore
.env
.env.*
!.env.example
```

The `!.env.example` exception allows a **safe template** to be committed (see below).

---

### 2. Create a `.env.example` Template
Commit a `.env.example` file with **placeholder values only** — never real secrets. This tells other developers which variables they need to configure.

```env
# .env.example — Copy to .env and fill in real values. NEVER commit .env.
DATABASE_URL=postgres://user:password@localhost:5432/db_name
API_KEY=your_api_key_here
SECRET_KEY=your_secret_key_here
DEBUG=false
```

---

### 3. Never Hardcode Secrets in Source Code
❌ **Bad:**
```python
db = connect("postgres://admin:mysecretpassword@localhost/mydb")
```

✅ **Good:**
```python
import os
db = connect(os.getenv("DATABASE_URL"))
```

---

### 4. Use Different `.env` Files per Environment
Separate configs for each context, and keep them all out of Git.

| File | Purpose |
|---|---|
| `.env` | Local development |
| `.env.test` | Test environment |
| `.env.staging` | Staging server |
| `.env.production` | Production server |

---

### 5. Validate Required Variables at Startup
Fail fast if a required variable is missing. This prevents silent failures from misconfiguration.

```python
import os

required = ["DATABASE_URL", "API_KEY", "SECRET_KEY"]
missing = [v for v in required if not os.getenv(v)]
if missing:
    raise EnvironmentError(f"Missing required env variables: {missing}")
```

---

### 6. Use a Secret Manager in Production
For production environments, avoid `.env` files entirely. Use a dedicated secret store:

| Tool | Provider |
|---|---|
| AWS Secrets Manager | Amazon Web Services |
| Google Secret Manager | Google Cloud |
| Azure Key Vault | Microsoft Azure |
| HashiCorp Vault | Self-hosted / Cloud |
| Doppler | Multi-cloud |

---

### 7. Rotate Secrets Regularly
Even if a secret has never been leaked, rotate API keys and passwords on a regular schedule (e.g., every 90 days) as a standard hygiene practice.

---

## 🚨 If `.env` Was Already Committed

1. **Rotate the secret immediately** — assume it is compromised.
2. **Remove it from Git history** using:
   ```bash
   # Recommended: git-filter-repo
   git filter-repo --path .env --invert-paths

   # Alternative: BFG Repo-Cleaner
   bfg --delete-files .env
   ```
3. **Force-push the cleaned history** and notify all collaborators to re-clone.
4. **Add `.env` to `.gitignore`** before your next commit.

> [!CAUTION]
> Simply deleting `.env` in a new commit is **not enough**. The file remains accessible in Git history.

---

## Quick Reference Checklist

- [ ] `.env` is listed in `.gitignore`
- [ ] A `.env.example` template is committed with placeholder values
- [ ] No secrets are hardcoded in source files
- [ ] Separate `.env` files exist per environment
- [ ] Production uses a secret manager, not a `.env` file
- [ ] Secret rotation is scheduled

# Security Policy: Sensitive Data Protection

This document outlines the types of information that **must never be committed** to version control. Protecting these secrets is critical to preventing unauthorized access, data breaches, and system compromises.

## 🚫 What Never to Push

### 1. Authentication Credentials & Secrets
*   **API Keys:** AWS, Google Cloud, OpenAI, Stripe, GitHub personal access tokens, etc.
*   **Private Keys:** SSH keys (`id_rsa`), SSL/TLS certificates (`.pem`, `.key`), GPG keys.
*   **Passwords:** Database passwords, user account passwords, service account credentials.
*   **Tokens:** OAuth tokens, JWT secrets, session cookies.

### 2. Configuration & Environment Files
*   **Local Environment Files:** `.env`, `.env.local`, `.env.development`, `.env.test`.
*   **Cloud Configuration:** Service account JSON files (e.g., Google Cloud `credentials.json`).
*   **Database Credentials:** Hardcoded connection strings with usernames and passwords.

### 3. Personally Identifiable Information (PII)
*   User email addresses, phone numbers, home addresses.
*   Financial data (credit card numbers, bank account details).
*   Personal identification numbers (SSN, Passport details).

## 🛡️ Best Practices

### Use `.gitignore`
Always ensure that sensitive files are listed in your `.gitignore` file.
```text
# Examples
.env
*.pem
id_rsa
credentials.json
```

### Use Environment Variables
Inject secrets into your application using environment variables rather than hardcoding them.

### Use Secret Management Tools
For production environments, use dedicated secret management services:
*   HashiCorp Vault
*   AWS Secrets Manager
*   Google Secret Manager
*   Azure Key Vault

## ⚠️ If You Accidentally Push a Secret
1.  **Rotate the Secret Immediately:** Change the password or revoke the API key. This is the most important step.
2.  **Remove it from History:** Use tools like `git-filter-repo` or BFG Repo-Cleaner to purge the file from the entire Git history. Simply deleting the file in a new commit is **not enough**.

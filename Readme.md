# Git Security Reference Project

A collection of security documentation and templates to help developers keep sensitive information out of version control.

## Files in This Repository

| File | Description |
|---|---|
| [`SECURITY.md`](SECURITY.md) | What information must never be pushed (API keys, credentials, etc.) |
| [`ENV_BEST_PRACTICES.md`](ENV_BEST_PRACTICES.md) | Best practices for `.env` files and keeping secrets out of Git |
| [`.gitignore`](.gitignore) | Template to exclude sensitive files from version control |

## Quick Rules

1. **Never commit secrets** — API keys, passwords, private keys, tokens.
2. **Always add `.env` to `.gitignore`** before your first commit.
3. **Commit `.env.example`** — a safe template with placeholder values only.
4. **Use a secret manager** in production (AWS Secrets Manager, HashiCorp Vault, etc.).
5. **If a secret leaks** — rotate it immediately, then purge it from Git history.

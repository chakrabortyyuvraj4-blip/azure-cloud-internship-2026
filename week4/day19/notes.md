# Day 19 — Key Vault Fundamentals & Credentials Management
## Key Vault URI
`https://kv-intern-yuvraj-01.vault.azure.net/`
## Secrets Stored
1. `db-password` — Database credentials
2. `processing-key` — Text/plain API key (Expires: 12/31/2026)
3. `zoho-api-key` — Zoho integration API key
## Key Insight: Why Key Vault > Environment Variables
**Environment variables risk:**
- Visible in process listings (`ps aux`)
- Logged in shell history (`.bash_history`)
- Exposed in container images if baked in
- No audit trail of who accessed them
- No automated rotation
- No fine-grained access control (RBAC)
**Key Vault advantages:**
- Secrets never stored locally (fetched at runtime via API)
- Automatic versioning & rotation (no restart needed)
- RBAC per secret (Function App gets only `Get`, not `Set`/`Delete`)
- Full audit log in Azure Monitor (who, what, when)
- Soft delete (90-day recovery window)
- Integrates with Managed Identity (zero secrets in code, even API key is gone)
**Tum-tum principle:** Credentials are NOT config—they're secrets. Never treat them like config variables.

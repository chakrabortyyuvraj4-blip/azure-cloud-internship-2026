# Day 20 Notes — Hosting Plans, Functions vs Logic Apps, Function App Deployment
**Date:** 20 June 2026 (Saturday)

---

## Task 1 — Serverless & Hosting Plans

**Serverless definition:** Provider manages infrastructure, code runs on events, billing is per-execution.

**VM vs Consumption trade-off:** VM has no cold start (always warm); Consumption Function costs ₹0 at rest but adds 5–30s delay on first request after idle.

**Cold start:** Happens on first request after idle — Azure allocates compute, loads runtime, initializes code. Premium Plan eliminates it by keeping instances always warm. Unacceptable for Tum-tum's driver booking endpoint (driver won't wait 8s — churns to competitor).

**Hosting Plans comparison:**

| Point | Consumption | Premium | Dedicated |
|---|---|---|---|
| Billing | Per-execution | Per reserved instance-hour | Per App Service Plan hour |
| Cold start | Yes | No | No |
| Max timeout | 10 min | 30 min | Unlimited |
| Scaling | 0→thousands, auto | Auto within reserved limits | Manual/auto on committed infra |
| VNet | Not native | Full support | Full support |
| Best for | Bursty/async, webhooks | Production APIs, latency-sensitive | Long-running, monoliths |
| Cost (low usage) | ₹0–100/mo | ₹2500–5000/mo | ₹1200–3000+/mo |

**Long-running tasks (>10 min, e.g. 2GB CSV):** Can't process in one invocation — use chunking pattern (split → parallel Process Chunk functions, each <10 min → consolidate) or Durable Functions orchestration.

---

## Task 2 — Functions vs Logic Apps vs App Service

**Why Logic Apps for Salesforce → Zoho:** Pre-built connectors, no API code needed; visual designer lets non-developers build/maintain; built-in retry/orchestration. Declarative beats imperative for integration work.

**Why Functions for File Processor:** Needs custom Python code (Logic Apps' visual designer can't express this computation), direct Key Vault + Application Insights integration, Blob trigger support.

**REST API on Functions:** Yes — HTTP trigger, one endpoint per Function. Auto-scales from zero, ₹0 at rest — good for sparse traffic. Cold starts (3–30s) unacceptable for production Tum-tum endpoints → need Premium (₹2500+/mo, no cold start). App Service costs more upfront but no cold starts, better sustained throughput.

---

## Task 3 — Function App Created (fn-intern-yuvraj-01)

- Runtime: Python 3.11, Linux, Consumption plan, Central India
- Linked Storage Account: **stinternyuvrajlearn** (existing, from Day 12)
- Linked App Insights: **ai-intern-yuvraj-01** (existing, from Day 18)
- URL: `fn-intern-yuvraj-01-hre3g4gvefc3gkh6.centralindia-01.azurewebsites.net`
- `AzureWebJobsStorage` → points to stinternyuvrajlearn (Functions runtime needs this to coordinate/store state, even when idle — it's an always-active infrastructure component)

---

## Task 4 — HttpGreeting Function Deployed

- Created via portal (Develop in portal), HTTP trigger, Auth level: Function
- Function URL: `.../api/HttpGreeting?code=...`
- **All 5 HTTP tests passed:**
  1. No param → default message ✅
  2. `?name=Arjun` → personalized greeting ✅
  3. `?name=Yuvraj` → personalized greeting ✅
  4. POST + JSON body → personalized greeting ✅
  5. Invalid URL → 404 ✅

---

## Task 5 — Monitoring: Streaming Logs vs Application Insights

- Streaming logs: real-time only, no history, can't search — good for live debugging.
- App Insights: 90+ day retention, KQL-queryable, correlates requests — good for production investigation.
- **For a 3-day-old error → use Application Insights** (streaming logs are long gone; App Insights has the full historical, searchable record).
- Ran KQL query on `requests` filtered by `cloud_RoleName` — confirmed invocations logged with duration (4–357ms range observed).

---

## Task 6 — File Processor Architecture (Final Project Design)

**Components & resource names:**
- Storage: `stinternyuvrajlearn` (uploads container)
- Function App: `fn-intern-yuvraj-01`
- Key Vault: `kv-intern-yuvraj-01` (secret: processing-key)
- App Insights: `ai-intern-yuvraj-01`
- Log Analytics: `law-intern-yuvraj-01`
- Alert Rule: Function errors > 10 threshold → Action Group (email)

**Secure connector — Managed Identity:** Connects Function App to Key Vault with zero stored credentials. Azure auto-provisions the identity, auto-rotates its token (~8hrs), and Key Vault validates it against an Access Policy (Get-only permission) before releasing the secret.

**Traffic flow (9 steps, .txt upload):** Upload → Blob event → Event Grid → Function trigger → Managed Identity auth → Key Vault secret fetch → file processing → telemetry logged → entry queryable in App Insights.

---

## Next: Days 23–28 — Build the full File Processor project using the architecture above.

# Day 18 — Azure Monitor, KQL Queries, and Alert Rules

## Key Differences: Metrics vs Logs

**Metrics:**
- Numerical time-series data (how much?)
- Auto-collected by Azure, free
- 93-day retention
**Logs:**
- Event-based data (what happened?)
- Requires Diagnostic Settings + Log Analytics Workspace
- $3.22/GB (Central India)
## KQL Queries Written

```kql
-- Query 1: All entries in AzureActivity from last 1 hour
AzureActivity |
where TimeGenerated > ago(1h)
-- Query 2: Count of records grouped by OperationName (top 10)
AzureActivity |
summarize Count = count() by OperationName |
top 10 by Count
-- Query 3: All AzureActivity records where ActivityStatus is "Failed"
AzureActivity |
where ActivityStatus == "Failed"
-- Query 4: AzureActivity records where ResourceGroup contains RG name
AzureActivity |
where ResourceGroup == "rg-yuvraj"
-- Query 5: Count of AzureActivity events per hour over last 24 hours
AzureActivity |
where TimeGenerated > ago(24h) |
summarize count() by bin(TimeGenerated, 1h)
```
## Alert Rules Created

1. **Backend VM CPU Critical** — CPU < 20% for 2 min → Severity 0 (Critical) → Push notification
2. **Backend VM Low Memory** — Available Memory < 512 MB for 3 min → Severity 1 (Error) → Email DevOps
3. **Storage Transactions High** — Transactions > 100k/hour → Severity 2 (Warning) → Email platform team
4. **Azure Function Error Spike** — Error count > 10 in 5 min → Severity 1 (Error) → Slack + PagerDuty
5. **NSG DenyAllInBound Hits** — DenyAllInBound > 100 in 5 min → Severity 2 (Warning) → Slack to Security
## One Thing That Surprised Me

I observed how complicated and fine tuned everything is, still works with so much ease. These features save tons of time as compared to doing everything manually.

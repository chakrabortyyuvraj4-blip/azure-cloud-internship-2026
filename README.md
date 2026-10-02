# Azure Cloud Internship 2026

Internship repository for the Azure Cloud Track at White Placard Systems Pvt. Ltd. (2 June – 8 July 2026), covering hands-on Azure networking, compute, storage, IAM, monitoring, and serverless development.

**Mentor:** Shailendra Garg, Senior Software Engineer
**Environment:** Azure Portal, Azure CLI, Azure Cloud Shell, Ubuntu 24.04 LTS, Python 3.11, Git/GitHub

---

## Overview

The internship was structured as daily hands-on assignments with weekly capstone projects, progressing through:

| Week | Focus | Days |
|---|---|---|
| Week 1 | Azure fundamentals, VM provisioning, Scale Sets | 1–5 |
| Week 2 | Networking — VNets, NSGs, ASGs, Load Balancer, App Gateway, WAF, DNS, Azure Firewall, VNet Peering, Hub-and-Spoke | 6–10 |
| Week 3 | Storage, Azure CLI, ARM Templates/Bicep, IAM & RBAC | 11–16 |
| Week 4 | Monitoring, Application Insights, Key Vault, Azure Functions (HTTP triggers) | 17–22 |

---

## Highlight: HttpGreeting Function (Day 20)

An HTTP-triggered Azure Function (Python 3.11, Linux) built and deployed to test request handling and App Insights integration. All 5 test cases passed on deployment. Code and notes: [`week4/day20/`](./week4/day20).

---

## Notes Coverage

Detailed day-by-day notes exist for **Days 17–20** (`week4/day17` through `week4/day20`), covering monitoring/App Insights, Key Vault concepts, and the `HttpGreeting` HTTP-triggered function.

Earlier weeks (Days 1–16) were completed hands-on in the Azure Portal but were not written up as standalone notes files in this repo. Topics covered are summarized in the week table above.

---

## Repository Structure

azure-cloud-internship-2026/
├── week1/ # VM provisioning, Scale Sets
├── week2/ # VNets, NSGs, Load Balancer, App Gateway, Firewall, Bastion
├── week3/ # Storage, CLI, ARM/Bicep, IAM/RBAC
└── week4/
├── day17-19/ # Monitoring, App Insights, Key Vault
└── day20/ # HttpGreeting function (HTTP trigger)

---

## Key Skills Demonstrated

- **Networking:** VNet/subnet design, NSG/ASG rules, App Gateway + WAF, Azure Firewall, Hub-and-Spoke topology, Azure Bastion
- **Compute:** VM deployment, Scale Sets, Azure Functions (HTTP triggers)
- **Storage:** Blob Storage, redundancy tiers, lifecycle policies, SAS tokens
- **IAM:** Entra ID, RBAC, Managed Identity vs. Service Principal, least-privilege access
- **Monitoring:** Application Insights, Log Analytics (KQL), Azure Monitor alerts
- **IaC & DevOps:** ARM Templates, Bicep, Azure CLI scripting, Git feature-branch workflow

# 🛡️ Microsoft Entra ID Zero Trust Lab Portfolio

Welcome to my auditable, proof-of-work security portfolio. This repository documents hands-on deployments across **29 Microsoft Entra ID SC-300 lab scenarios** inside a dedicated M365 Developer Tenant environment.

---

## 🎯 Portfolio Objective

This portfolio demonstrates practical competency in designing, implementing, and securing Identity and Access Management (IAM) solutions following Microsoft's **Zero Trust Architecture** principles:
1. **Explicit Verification:** Always authenticate and authorize based on all available data points.
2. **Least Privilege:** Limit user access with Just-In-Time (JIT) and Just-Enough-Access (JEA).
3. **Assume Breach:** Minimize blast radius and segment access.

---

## 📐 Environment Architecture

* **Tenant Type:** Microsoft 365 Developer Tenant (E5 Governance & Security)
* **Primary Identity Provider:** Microsoft Entra ID
* **Target Certification Alignment:** Microsoft SC-300 (Identity and Access Administrator)
* **Author:** Darrin (`darrinpor`)

---

## 📂 SC-300 Domain Index

| Domain Folder | Description | Labs Covered | Status |
| :--- | :--- | :---: | :---: |
| [**Domain 1: Identity Governance**](./labs/Domain-1-Identity-Governance/) | Core Identity, PIM, Entitlement Management, Access Reviews, Lifecycle Workflows, & Federation | Labs 01 – 07 | 🟡 In Progress |
| [**Domain 2: Access Management**](./labs/Domain-2-Access-Management/) | MFA, Risk-based Conditional Access, FIDO2, Named Locations, & App SSO | Labs 08 – 16 | 🟡 In Progress |
| [**Domain 3: Identity Protection**](./labs/Domain-3-Identity-Protection/) | Entra ID Protection, Risk Remediation, & Break-Glass Policies | Labs 17 – 22 | ⏳ Scheduled |
| [**Domain 4: Identity Sync & Security**](./labs/Domain-4-Identity-Sync-And-Security/) | Hybrid Sync (Entra Connect), Bulk Provisioning, & Administrative Units | Labs 23 – 28 | ⏳ Scheduled |

---

## 🛠️ Portfolio Standard & Structure

Each lab entry follows a rigorous 6-section documentation standard:
1. **Executive Summary & Objective** — Business context and core security intent.
2. **Architecture & Control Design** — Visual or technical logic flow.
3. **Step-by-Step Implementation** — Portal navigation and PowerShell Graph SDK scripts.
4. **Verification & Audit Evidence** — Annotated portal screenshots and audit logs.
5. **Zero Trust & Security Analysis** — Threat mitigation and risk assessment.
6. **Rollback & Cleanup** — De-provisioning procedures.

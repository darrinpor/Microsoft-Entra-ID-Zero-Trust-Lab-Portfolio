Markdown
# 🛡️ Lab 04: Configure External Collaboration Settings in Microsoft Entra ID

## 📌 Executive Summary & Overview

In this lab, tenant-level external collaboration settings and B2B governance controls were implemented within the custom Microsoft Entra ID tenant (`darrinporidentitylabs.onmicrosoft.com`). The core goal of this phase was establishing least-privilege boundary controls for external guest users, securing authentication methods for third-party partners, and configuring self-service registration workflows in alignment with Microsoft Zero Trust principles.

### Key Objectives Accomplished:
- **Guest Self-Service Sign-Up:** Enabled user flows for external registration workflows to streamline partner onboarding.
- **Secure Authentication:** Configured Email One-Time Passcode (OTP) authentication for guests without existing Microsoft or organizational accounts.
- **Least-Privilege Directory Access:** Restricted guest user visibility to their own directory objects to prevent internal directory enumeration and reconnaissance.
- **Invitation Governance:** Controlled B2B invitation permissions to ensure only authorized member users and administrative roles can invite external partners.

---

## 🏗️ Architecture and Control Design

- **Tenant Domain:** `darrinporidentitylabs.onmicrosoft.com`
- **Administrative Role:** Global Administrator
- **Licensing Tier:** Microsoft Entra ID P2 (Active 30-day Trial)
- **Collaboration Scope:** Tenant-wide B2B governance and external identity boundaries.

---

## 🚀 Implementation Steps

### Exercise 1: Allowing Guest Users to Be Invited into Your Organization

#### Task 1: Enable Guest Users to Perform Self-Service Sign-Up
1. Signed in to the **Microsoft Entra admin center** (`https://entra.microsoft.com`) as a Global Administrator.
2. Navigated to **Entra ID** → **Users** → **All users** → **User settings** → **Manage external user collaboration settings**.
3. Configured **Enable guest self-service sign up via user flows** to **Yes** to allow streamlined external onboarding.

#### Task 2: Configure External Collaboration Settings & Identity Providers
1. Navigated to **Entra ID** → **External Identities** → **All identity providers**.
2. Selected **Email one-time passcode** and verified its active configuration status is set to **Yes**.
3. Returned to **External Identities** → **External collaboration settings** to configure guest access boundaries, invite restrictions, and collaboration domain rules.

---

## 🖼️ Verification & Evidence

### Exercise 1 Screenshots

#### `01-guest-self-service-signup.png`
![01 - Guest Self-Service Sign-Up](./screenshots/01-guest-self-service-signup.png)

- **Visual Annotations & Highlights:**
  * **Red Box:** Highlights the **Enable guest self-service sign up via user flows** toggle set to **Yes**.
- **Technical Note:** Tenant-level enablement allowing external guest users to register themselves into user flows without direct administrative provisioning overhead.

---

#### `02-email-otp-provider.png`
![02 - Email OTP Provider](./screenshots/02-email-otp-provider.png)

- **Visual Annotations & Highlights:**
  * **Red Box:** Highlights **Email one-time passcode for guests** configured to **Yes**.
- **Technical Note:** Email one-time passcodes provide a secure authentication method for guests who do not have a pre-existing Microsoft or organizational account, verifying their email ownership at login.

---

#### `03-external-collaboration-settings.png`
![03 - External Collaboration Settings](./screenshots/03-external-collaboration-settings.png)

- **Visual Annotations & Highlights:**
  * **Top Red Box (Guest User Access):** Highlights **Guest user access is restricted to properties and memberships of their own directory objects (most restrictive)**.
  * **Bottom Red Box (Invite Permissions):** Highlights **Member users and users assigned to specific admin roles can invite guest users including guests with member permissions**.
- **Technical Note:** Enforces least-privilege boundary controls. Restricting guest directory visibility prevents external identities from enumerating internal users, groups, or organizational directory resources.

---

## 🛠️ Troubleshooting Logs & Environment Discrepancies

During the configuration of Lab 04, the following operational behaviors and administrative considerations were analyzed:

### 1. Identity Provider Precedence & Authentication Fallback
- **Observed Behavior:** When inviting external users who possess non-Microsoft/non-Google identities, the tenant defaults to email passcode verification.
- **Technical Insight:** Microsoft Entra automatically prioritizes federated identity providers (such as Google or Facebook) and organizational direct federation before falling back to email one-time passcodes.

### 2. Workload Independence of Collaboration Restrictions
- **Observed Behavior:** Configuring allow/deny domain lists in Entra ID external collaboration settings.
- **Technical Insight:** Tenant-level B2B domain restrictions operate independently from SharePoint Online and OneDrive file-sharing allow/block lists; security administrators must coordinate policies across workloads for comprehensive governance.

---

## 🔒 Security Control Analysis & Governance Impact

| Feature / Mechanism | Default Tenant Setting | Zero Trust Hardened Setting | Security Impact & Mitigation |
| --- | --- | --- | --- |
| **Guest Directory Visibility** | Limited access (can see non-hidden groups) | Restricted to own directory objects | Prevents external accounts from harvesting internal user and group structure reconnaissance data. |
| **B2B Invitation Rights** | Anyone in the organization (including guests) | Members and specific admin roles | Stops unauthorized users or compromised guest accounts from indiscriminately inviting external entities. |
| **Guest Authentication** | Requires Microsoft account / password | Email One-Time Passcode (OTP) | Eliminates password fatigue and weak credentials for third-party partners. |

---

## 🔄 Rollback & Cleanup Procedures

1. Maintained external collaboration settings (Guest least-restrictive access and email OTP) as baseline security parameters required for subsequent Identity Governance and B2B labs.
2. Verified that collaboration domain restriction lists remain unconfigured until specific enterprise partner allow/deny policies are mandated.

---

## 📊 Summary of Completed Lab Artifacts

| File Name | Exercise Focus | Summary of Visual Focus & Annotations |
| --- | --- | --- |
| **`01-guest-self-service-signup.png`** | Self-Service Registration | Red box outlining **Enable guest self-service sign up via user flows** toggle set to **Yes**. |
| **`02-email-otp-provider.png`** | Identity Providers | Red box highlighting **Email one-time passcode for guests** set to **Configured/Yes**. |
| **`03-external-collaboration-settings.png`** | Access & Invite Boundaries | Red boxes high

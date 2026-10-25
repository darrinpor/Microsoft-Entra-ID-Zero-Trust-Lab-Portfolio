# 🛡️ Lab 17: Defender for Cloud Apps Application Discovery and Enforcing Restrictions

## 📋 1. Executive Summary & Objective
Microsoft Defender for Cloud Apps utilizes logs from network traffic to identify the applications that users are accessing across the enterprise. Traffic logs from on-premises firewalls provide snapshot reports on common applications and users, while traffic from managed devices feeds directly into the Defender for Cloud Apps discovery overview dashboard. The objective of this lab is to review cloud app discovery data, inspect risk metrics, and configure application restrictions (unsanctioning) in Microsoft Defender for Cloud Apps to govern shadow IT.

In this scenario, cloud app discovery and restrictions were configured and verified within the custom tenant:
1. **Catalog Navigation & Risk Assessment:** Accessed the **Cloud App Catalog** via the Microsoft Defender portal, filtered by **Cloud storage**, and reviewed risk metrics for applications such as **Dropbox**.
2. **App Governance & Unsanctioning:** Evaluated application states and configured administrative restrictions by marking high-risk cloud services as **Unsanctioned**.
3. **Endpoint Enforcement Integration:** Committed policy updates to automatically block access on devices onboarded to Microsoft Defender for Endpoint, mitigating data exfiltration risks.

---

## 🏗️ 2. Architecture & Control Design

```text
+-------------------------------------------------------------------------------------------------+
|                                 TENANT SECURITY BOUNDARY                                        |
|                                                                                                 |
|    Directory Tenant: DarrinPor Enterprises (DarrinPorIdentityLabs.onmicrosoft.com)              |
|    Prerequisite Licensing: Microsoft Defender for Cloud Apps / Microsoft Entra ID P1/P2          |
+-----------------------------------+-----------------------------------------------------+-------+
                                    |
                                    v
+-------------------------------------------------------------------------------------------------+
|                          MICROSOFT DEFENDER PORTAL & CLOUD APPS                                 |
|                                                                                                 |
|    - Portal Context: Microsoft Defender Security Portal ([https://security.microsoft.com](https://security.microsoft.com))        |
|    - Discovery Blade: Cloud Apps > Cloud App Catalog                                            |
|    - App Category Filter: Cloud storage (Targeting Dropbox)                                     |
|    - Risk Evaluation: Automated risk scoring under General tab (Score metrics)                  |
+-----------------------------------+-----------------------------------------------------+-------+
                                    |
                                    v
+-------------------------------------------------------------------------------------------------+
|                              GOVERNANCE & ENDPOINT ENFORCEMENT                                  |
|                                                                                                 |
|    - Administrative Action: Transition app status from Sanctioned to Unsanctioned               |
|    - Endpoint Integration: Microsoft Defender for Endpoint automated block enforcement          |
|    - Scope of Block: Standard browsers, InPrivate/Incognito sessions, and app store downloads   |
+-------------------------------------------------------------------------------------------------+

```

---

## ⚙️ 3. Step-by-Step Implementation

### Task 1: Discovering Apps in Defender for Cloud Apps

1. Signed in to the **Microsoft Defender portal** at `https://security.microsoft.com` using a Global Administrator account, completing multi-factor authentication (MFA) prompts as required.


2. In the left navigation menu, expanded **Cloud Apps** and selected **Cloud App Catalog**.


3. In the filter bar, set **Category** to **Cloud storage**.


4. From the list of applications, selected **Dropbox**.


5. In the app details pane, reviewed the risk score shown under the **General** tab.


6. Verified external access by opening a new browser tab to Dropbox at `https://www.dropbox.com` and subsequently closed the tab.



### Task 2: Restricting Apps in Defender for Cloud Apps

1. Returned to the **Cloud app catalog** in the Microsoft Defender portal.


2. Located and selected **Dropbox** from the list of applications to open its details pane.


3. Selected **Unsanction** and clicked **Save** to apply the configuration change.



---

## 📸 4. Verification & Audit Evidence

### 1. Cloud App Catalog & Risk Score Inspection
- **File:** `lab17-01-cloud-app-catalog-dropbox.png`
- **Path:** `screenshots/lab17-01-cloud-app-catalog-dropbox.png`
- **Visual Focus:** Outline the **Cloud app catalog** header in a **Red Box** and highlight the **Score** metric under the General tab in a **Green Box**.

![Cloud App Catalog and Risk Score](https://raw.githubusercontent.com/darrinpor/Microsoft-Entra-ID-Zero-Trust-Lab-Portfolio/main/labs/Domain-3-Identity-Protection/Lab-17-Defender-for-Cloud-Apps-application-discovery-and-enforcing-restrictions/screenshots/lab17-01-cloud-app-catalog-dropbox.png)

### 2. App Unsanctioning and Enforcement
- **File:** `lab17-02-app-catalog-unsanction.png`
- **Path:** `screenshots/lab17-02-app-catalog-unsanction.png`
- **Visual Focus:** Outline the successful tag creation confirmation notice in a **Red Box** and highlight the **Unsanctioned app** status badge in a **Green Box**.

![Unsanctioning Dropbox Application](https://raw.githubusercontent.com/darrinpor/Microsoft-Entra-ID-Zero-Trust-Lab-Portfolio/main/labs/Domain-3-Identity-Protection/Lab-17-Defender-for-Cloud-Apps-application-discovery-and-enforcing-restrictions/screenshots/lab17-02-app-catalog-unsanction.png)

---

## 🛡️ 5. Zero Trust & Security Analysis

| Security Control | Threat Mitigation & Risk Assessment |
| --- | --- |
| **Shadow IT Discovery** | Uncovers unmanaged or unauthorized cloud applications utilized across corporate network traffic and device telemetry. |
| **Automated Endpoint Blocking** | Enforces instant access termination across integrated Microsoft Defender for Endpoint devices once an app is marked unsanctioned. |
| **Granular Session Restriction** | Prevents data exfiltration vectors by blocking application access across standard browsers, InPrivate/Incognito modes, and store downloads. |
| **Risk-Based Governance** | Empowers security administrators to evaluate quantitative risk scores and apply strict policy enforcement on high-risk cloud storage. |

---

## 🧹 6. Rollback & Cleanup

To maintain tenant configuration hygiene and remove lab-specific restrictions, execute the following cleanup steps:

### Portal Cleanup

1. **Revert Application Unsanction Status:**
* Navigate to **Microsoft Defender portal** (`https://security.microsoft.com`) > **Cloud Apps** > **Cloud App Catalog**.


* Locate and select **Dropbox**, then click **Sanction** to restore standard app access if returning the tenant to baseline state.


* Click **Save** to commit configuration updates.



---

## 📝 Troubleshooting Logs & Environmental Discrepancies

* **Propagation Latency:** Changes made when sanctioning or unsanctioning an application can take up to 5 minutes to take effect across the environment.


* **Endpoint Integration Dependency:** Client-side blocking relies directly on the integration between Microsoft Defender for Cloud Apps and Microsoft Defender for Endpoint to enforce traffic interception at the device level.


* **Portal Navigation Path:** Ensure you are utilizing the dedicated security portal (`https://security.microsoft.com`) rather than the legacy cloud apps dashboard to access current cloud app catalog filters and risk scorings.

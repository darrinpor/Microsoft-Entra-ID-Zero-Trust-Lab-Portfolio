# 🆔 Lab 06: Add a Federated Identity Provider

## 📋 1. Executive Summary & Objective
This lab implements B2B direct federation between **Microsoft Entra ID** and **Google Identity Services** using OpenID Connect (OIDC) / OAuth 2.0 standards. By configuring upstream OAuth app credentials, setting up OAuth redirect URIs, and authorizing developer credentials, external partners and vendor users can authenticate into organizational resources using their existing native Google accounts.

In this scenario, a test external user (`External User` / `darrinpor.identitylabs@gmail.com`) was onboarded to demonstrate cross-domain single sign-on (SSO). Configuring direct federation eliminates the need for separate credential management or manual one-time passcode (OTP) verification emails, establishing a seamless authentication flow and enforcing Zero Trust cross-tenant identity boundary security.

---

## 🏗️ 2. Architecture & Control Design


```

+-------------------------------------------------------------------------------------------------+
|                                 UPSTREAM IDENTITY PROVIDER                                      |
|                                                                                                 |
|   +-----------------------------------------------------------------------------------------+   |
|   | Google Cloud Platform (Google Auth Platform)                                            |   |
|   |  - Application Name: MyB2BApp                                                            |   |
|   |  - OAuth 2.0 Client Credentials (Client ID & Client Secret)                             |   |
|   |  - Authorized Javascript Origin: [https://microsoftonline.com](https://microsoftonline.com)                            |   |
|   |  - Test Audience User: darrinpor.identitylabs@gmail.com                                 |   |
|   +-----------------------------------+-----------------------------------------------------+   |
|                                       |                                                         |
+---------------------------------------|---------------------------------------------------------+
| OAuth 2.0 / OIDC Trust
v
+-------------------------------------------------------------------------------------------------+
|                                 MICROSOFT ENTRA ID TENANT                                       |
|                                                                                                 |
|   Tenant Name: darrinporidentitylabs.onmicrosoft.com                                            |
|   +-----------------------------------------------------------------------------------------+   |
|   | External Identity Provider: Google (Configured)                                         |   |
|   |  - Authorized Redirect URIs:                                                            |   |
|   |    1. [https://login.microsoftonline.com](https://login.microsoftonline.com)                                                 |   |
|   |    2. [https://login.microsoftonline.com/te/471d4677-22bd-4835-be02-403487c64c01/oauth2/](https://www.google.com/search?q=https://login.microsoftonline.com/te/471d4677-22bd-4835-be02-403487c64c01/oauth2/)  |   |
|   |       authresp                                                                          |   |
|   |    3. [https://login.microsoftonline.com/te/darrinporidentitylabs.onmicrosoft.com/](https://www.google.com/search?q=https://login.microsoftonline.com/te/darrinporidentitylabs.onmicrosoft.com/)       |   |
|   |       oauth2/authresp                                                                   |   |
|   +-----------------------------------+-----------------------------------------------------+   |
|                                       |                                                         |
|                                       v                                                         |
|   +-----------------------------------------------------------------------------------------+   |
|   | B2B Guest Account: External User (darrinpor.identitylabs@gmail.com)                      |   |
|   |  - Identity Issuer: google.com                                                          |   |
|   |  - Access Scope: Assigned Enterprise Apps & Resources                                  |   |
|   +-----------------------------------+-----------------------------------------------------+   |
|                                       |                                                         |
+---------------------------------------|---------------------------------------------------------+
| Federated SSO Token Handshake
v
+-------------------------------------------------------------------------------------------------+
|                                  AUTHENTICATED ENDPOINT                                         |
|                                                                                                 |
|   My Apps Portal: [https://myapps.microsoft.com/](https://myapps.microsoft.com/)                                                 |
+-------------------------------------------------------------------------------------------------+

```

---

## ⚙️ 3. Step-by-Step Implementation

### Task 1: Configure Upstream OAuth Project in Google Cloud Console
1. Signed into the Google Cloud Console (`https://console.developers.google.com`) using the primary administrator account.
2. Created a new project named **MyB2BApp**.
3. Navigated to **APIs & Services** > **Google Auth Platform** (OAuth consent screen) and configured the application parameters:
   * **App Name:** Microsoft Entra ID
   * **User Support Email:** Administrator address
   * **Audience:** External
   * **Developer Contact Information:** `darrinpor.identitylabs@gmail.com`
4. Created an **OAuth 2.0 Client ID** under Web Application settings:
   * **Authorized JavaScript origins:** `https://microsoftonline.com`
   * **Authorized redirect URIs:**
     * `https://login.microsoftonline.com`
     * `https://login.microsoftonline.com/te/471d4677-22bd-4835-be02-403487c64c01/oauth2/authresp`
     * `https://login.microsoftonline.com/te/darrinporidentitylabs.onmicrosoft.com/oauth2/authresp`
5. Extracted and securely saved the generated **Client ID** and **Client Secret**.

### Task 2: Configure Test Audience and Branding
1. Navigated to the **Audience** tab under Google Auth Platform.
2. Added `darrinpor.identitylabs@gmail.com` under the **Test users** section to grant test authorization prior to public application publishing.
3. Selected **Branding** and added `microsoftonline.com` to the **Authorized domains** configuration list.

### Task 3: Configure Google Direct Federation in Microsoft Entra ID
1. Logged into the Microsoft Entra admin center (`https://entra.microsoft.com`) as Global Administrator.
2. Navigated to **Identity** > **External Identities** > **All identity providers**.
3. Selected the **Configure** button inline with the Google identity provider entry.
4. Input the **Client ID** and **Client Secret** retrieved from the Google Cloud Console and saved the configuration.

### Task 4: Invite Guest Account and Validate Federated Authentication
1. Navigated to **Entra ID** > **Users** > **All users** and selected **Invite external user**.
2. Entered identity details:
   * **Name:** External User
   * **Email:** `darrinpor.identitylabs@gmail.com`
3. Sent the invitation and opened an InPrivate/Incognito browser session to log into the Gmail account.
4. Accepted the Microsoft B2B invitation link from the inbox.
5. Successfully authenticated via Google's upstream identity assertions and landed directly on the My Apps portal (`https://myapps.microsoft.com/`).

---

## 📸 4. Verification & Audit Evidence

### 1. Google Provider Configuration in Entra ID

* **File:** `01-google-federation-configured.png`
* **Path:** `Lab-06-Add-a-Federated-Identity-Provider/screenshots/01-google-federation-configured.png`
* **Visual Focus:** Outline the **Google** provider row in a **Red Box** and highlight the **Configured** status badge in a **Green Box**, confirming the upstream OAuth client binding.

### 2. Invited Federated Guest User Object

* **File:** `02-google-guest-user-invited.png`
* **Path:** `Lab-06-Add-a-Federated-Identity-Provider/screenshots/02-google-guest-user-invited.png`
* **Visual Focus:** Outline the user account (`External User` / `darrinpor.identitylabs@gmail.com`) in a **Red Box** and highlight the **Guest** value under the **User type** column in a **Green Box**.

### 3. Successful Federated Sign-In & Portal Landing

* **File:** `03-google-federated-signin-success.png`
* **Path:** `Lab-06-Add-a-Federated-Identity-Provider/screenshots/03-google-federated-signin-success.png`
* **Visual Focus:** Highlight the active profile badge showing `darrinpor.identitylabs@gmail.com` in a **Green Box** and outline the address bar displaying `https://myapps.microsoft.com/` in a **Red Box**.

---

## 🔒 5. Zero Trust & Security Analysis

| Security Control | Threat Mitigation & Risk Assessment |
| --- | --- |
| **Upstream Identity Delegation** | Eliminates shadow credentials and password duplication by relying on Google's native authentication infrastructure and MFA controls. |
| **Explicit OAuth Redirect Handshaking** | Restricts authentication response callbacks strictly to GUID and domain-specific endpoints, preventing token redirection or interception attacks. |
| **Zero-Trust Guest Scoping** | External users are registered strictly as Guest objects, preventing privilege escalation into internal directory tenant read operations. |

---

## 🧹 6. Rollback & Cleanup

To cleanly remove the B2B Google Federation testing artifacts and restore default tenant security configuration, execute the following portal or PowerShell cleanup steps:

### Portal Cleanup

1. **Delete Guest User Object:**
* Navigate to **Entra ID** > **Users** > **All users**.
* Search for `darrinpor.identitylabs@gmail.com`, select the user, and click **Delete**.
* Navigate to **Deleted users** and permanently purge the object.


2. **Remove Google Identity Provider:**
* Navigate to **Identity** > **External Identities** > **All identity providers**.
* Select **Google**, click **Reset configuration** or **Delete**, and confirm removal.


3. **Delete Upstream Google OAuth App:**
* Sign into the Google Cloud Console (`https://console.developers.google.com`).
* Select project **MyB2BApp** and delete the OAuth 2.0 Client ID or shut down the project.

---

## 📝 Troubleshooting Logs & Environmental Discrepancies

* **Google Cloud Console UI Modernization:** The lab guide indicates navigating to the legacy "OAuth consent screen" to locate audience controls. In the updated Google Cloud Platform UI, these controls are under **Google Auth Platform**, split into **Branding**, **Audience**, and **Clients** tabs.
* **Entra ID Provider Configuration Button:** Modernized Entra admin center interface replaces the historic `+ Google` top-level menu button with an inline **Configure** button adjacent to Google in the **All identity providers** list.
* **Direct SSO Redirection Bypass:** During verification, accepting the email invitation redirected directly to `https://myapps.microsoft.com/` without prompting for additional permissions or credentials. This occurs when an active Google browser session exists; Entra ID validates the upstream OAuth assertion instantly and issues a session token without redundant prompt steps.

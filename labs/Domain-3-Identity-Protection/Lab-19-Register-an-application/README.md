# 🛡️ Lab 19: Register an Application

## 📋 1. Executive Summary & Objective
Registering an application in Microsoft Entra ID establishes a unidirectional trust relationship where your application trusts the Microsoft identity platform to issue secure tokens and manage authentication. Applications act as the identity principals for web services, daemons, and client applications. The objective of this lab is to register a web application, configure platform-specific authentication settings, expose a custom API with granular delegated and admin-only scopes, and implement least-privilege administrative governance using custom directory roles for credential management.

In this scenario, application registration and role-based governance were configured and verified within the custom tenant:
1. **App Registration & Platform Configuration:** Registered a single-tenant application (`Demo app`) and configured platform settings with secure web redirect URIs (`https://localhost`).
2. **API Exposure & Scope Definition:** Defined a unique Application ID URI and created custom scopes (`Employees.Read.All` and `Employees.Write.All`) enforcing distinct user and admin consent requirements.
3. **Least-Privilege Custom Roles:** Created a targeted custom directory role (`My custom app role`) restricted strictly to credential management permissions, enforcing separation of duties.

---

## 🏗️ 2. Architecture & Control Design

```text
+-------------------------------------------------------------------------------------------------+
|                                   TENANT SECURITY BOUNDARY                                      |
|                                                                                                 |
|    Directory Tenant: DarrinPor Enterprises (DarrinPorIdentityLabs.onmicrosoft.com)                |
|    Prerequisite Licensing: Microsoft Entra ID P2                                                |
+-----------------------------------+-----------------------------------------------------+-------+
                                    |
                                    v
+-------------------------------------------------------------------------------------------------+
|                         MICROSOFT ENTRA ID & APP REGISTRATIONS                                  |
|                                                                                                 |
|    - Identity Object: Enterprise Web Application ("Demo app")                                   |
|    - Identifiers: Application (client) ID & Directory (tenant) ID assigned                      |
|    - Platform Configuration: Web platform type with https://localhost redirect URI              |
|    - Trust Model: Unidirectional trust between client app and Microsoft identity platform        |
+-----------------------------------+-----------------------------------------------------+-------+
                                    |
                                    v
+-------------------------------------------------------------------------------------------------+
|                               API EXPOSURE & GOVERNANCE ROLES                                   |
|                                                                                                 |
|    - Application ID URI: api://<tenant-domain>/... (Unique namespace prefix)                    |
|    - Scopes Exposed: Employees.Read.All (Admins & Users) & Employees.Write.All (Admins only)    |
|    - Administrative Governance: Custom Role restricting updates to credentials & secrets only   |
+-------------------------------------------------------------------------------------------------+

```

---

## ⚙️ 3. Step-by-Step Implementation

### Task 1: Registering an Application

1. Signed in to the **Microsoft Entra admin center** at `https://entra.microsoft.com` using a Global Administrator account, completing multi-factor authentication (MFA) prompts as required.


2. In the left navigation menu, expanded **Entra ID** and selected **App registrations**.


3. Selected **+ New registration** from the top menu.


4. Configured the application settings:
* **Name:** `Demo app`


* **Supported account types:** Accounts in this organizational directory only (Single tenant)


* **Redirect URI:** Left blank (configured in subsequent steps)




5. Clicked **Register** to establish the application object and land on the overview page.



### Task 2: Configuring Platform Settings

1. From the registered application blade, selected **Authentication** under the *Manage* menu.


2. Selected **+ Add a platform** and chose the **Web** tile.


3. Entered `https://localhost` into the **Redirect URI** field.


4. Clicked **Configure** to save the platform authentication endpoint.



### Tasks 3, 4 & 5: Exposing an API and Defining Scopes

1. Selected **Expose an API** under the *Manage* menu.


2. Set the **Application ID URI** to its unique prefix value (`api://...`) and clicked **Save and continue**.


3. Selected **+ Add a scope** and configured the first user/admin scope:
* **Scope name:** `Employees.Read.All`


* **Who can consent:** Admins and users


* **Admin/User consent display names and descriptions:** Configured for read-only access to employee records.


* **State:** Enabled




4. Selected **+ Add a scope** again and configured the second privileged scope:
* **Scope name:** `Employees.Write.All`


* **Who can consent:** Admins only


* **Admin consent details:** Configured for write access to employee data.


* **State:** Enabled





### Exercise 2: Managing App Registration with a Custom Role

1. In the Microsoft Entra admin center, selected **Roles & admins** from the left navigation menu.


2. Selected **+ New custom role** from the top menu.


3. On the **Basics** tab, named the role `My custom app role` and proceeded to permissions.


4. In the permission search filter, entered `credentials`.


5. Selected the required bare-minimum permissions for credential management:
* `microsoft.directory/servicePrincipals/managePasswordSingleSignOnCredentials`


* `microsoft.directory/servicePrincipals/synchronizationCredentials/manage`




6. Reviewed settings and clicked **Create** to instantiate the custom least-privilege role.



---

## 📸 4. Verification & Audit Evidence

### 1. App Registration Overview
- **File:** `lab19-01-app-registration-overview.png`
- **Path:** `labs/Domain-3-Identity-Protection/Lab-19-Register-an-application/screenshots/lab19-01-app-registration-overview.png`
- **Visual Focus:** Outline the **Demo app** display name in a **Red Box** and highlight the **Application (client) ID** and **Directory (tenant) ID** fields in a **Green Box**.

![App Registration Overview](/labs/Domain-3-Identity-Protection/Lab-19-Register-an-application/screenshots/lab19-01-app-registration-overview.png)

### 2. Platform Authentication Configuration
- **File:** `lab19-02-platform-authentication-config.png`
- **Path:** `labs/Domain-3-Identity-Protection/Lab-19-Register-an-application/screenshots/lab19-02-platform-authentication-config.png`
- **Visual Focus:** Outline the **Web** platform configuration block and the configured `https://localhost` Redirect URI in a **Green Box**.

![Platform Authentication Configuration](/labs/Domain-3-Identity-Protection/Lab-19-Register-an-application/screenshots/lab19-02-platform-authentication-config.png)

### 3. Expose an API Scopes
- **File:** `lab19-03-expose-api-scopes.png`
- **Path:** `labs/Domain-3-Identity-Protection/Lab-19-Register-an-application/screenshots/lab19-03-expose-api-scopes.png`
- **Visual Focus:** Outline the Application ID URI prefix in a **Red Box** and highlight the defined scopes (`Employees.Read.All` and `Employees.Write.All`) along with their consent states in a **Green Box**.

![Expose an API Scopes](/labs/Domain-3-Identity-Protection/Lab-19-Register-an-application/screenshots/lab19-03-expose-api-scopes.png)

### 4. Custom Role Credential Permissions
- **File:** `lab19-04-custom-role-credentials-permissions.png`
- **Path:** `labs/Domain-3-Identity-Protection/Lab-19-Register-an-application/screenshots/lab19-04-custom-role-credentials-permissions.png`
- **Visual Focus:** Outline the search filter box (`credentials`) in a **Red Box** and highlight the selected credential management permission rows in a **Green Box**.

![Custom Role Credential Permissions](/labs/Domain-3-Identity-Protection/Lab-19-Register-an-application/screenshots/lab19-04-custom-role-credentials-permissions.png)

---

## 🛡️ 5. Zero Trust & Security Analysis

| Security Control | Threat Mitigation & Risk Assessment |
| --- | --- |
| **Unidirectional Trust Model** | Enforces secure token issuance where application identities authenticate against the Microsoft identity platform without exposing underlying directory credentials.

 
| **Granular API Scope Consent** | Separates read-only user operations (`Employees.Read.All`) from high-privilege administrative mutations (`Employees.Write.All`) to prevent unauthorized data tampering.

 
| **Least-Privilege Role Delegation** | Restricts administrative tasks to targeted credential-management permissions (`My custom app role`), eliminating the security risks associated with standing Global Administrator access. 

 
| **Explicit Redirect URI Control** | Prevents token injection and redirection attacks by strictly binding reply URLs (such as `https://localhost`) to authorized application endpoints.

---

## 🧹 6. Rollback & Cleanup

To maintain tenant configuration hygiene and remove lab-specific application objects, execute the following cleanup steps:

### Portal Cleanup

1. **Delete Lab Application Registration:**
* Navigate to **Microsoft Entra admin center** (`https://entra.microsoft.com`) > **App registrations** > **All applications**.


* Locate and select **Demo app**.
* Click **Delete** from the top menu and confirm the deletion to remove the application object and associated service principal.


2. **Delete Custom Directory Role:**
* Navigate to **Entra ID** > **Roles & admins**.


* Locate and select **My custom app role**.
* Click **Delete** to remove the custom administrative definition from the tenant.



---

## 📝 Troubleshooting Logs & Environmental Discrepancies

* **Application Propagation Delay:** Newly registered application identifiers and permission changes can occasionally experience a 1-to-2 minute directory replication delay before downstream API calls recognize the updated scopes.
* **URI Uniqueness Constraints:** Application ID URIs must be globally unique across directories when using default verification prefixes, requiring custom domains or verified tenant URI structures if conflicts occur.

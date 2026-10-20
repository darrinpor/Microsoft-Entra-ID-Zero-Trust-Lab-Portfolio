# Lab 05: Add Guest Users to the Directory

## 📋 Lab Objective
Learn and execute multiple methods for onboarding external vendor and partner accounts into Microsoft Entra ID for B2B collaboration. This lab covers individual administrative portal invitations, bulk CSV operations, and programmatic guest provisioning using the Microsoft Graph PowerShell SDK.

---

## 🛠️ Implementation Steps & Verification

### Exercise 1: Add Guest Users to the Directory

1. Signed in to the **Microsoft Entra admin center** (`https://entra.microsoft.com`) as a Global Administrator.
2. Navigated to **Entra ID** → **Users** → **All users** and selected **+ New user** → **Invite external user**.
3. Entered the target external email address (`sc300externaluser1@sc300email.com`), reviewed user properties, and issued the invitation.

#### `01-guest-user-directory-verification.png`
![01 - Guest User Directory Verification](./screenshots/01-guest-user-directory-verification.png)

* **Visual Annotations & Highlights:**
  * **Green Box:** Highlights the **User type** directory attribute column.
  * **Red Box:** Highlights **Guest**, confirming proper external identity classification for `sc300externaluser1`.
* **Technical Note:** Confirms that individual B2B guest invitations instantly populate directory objects with proper external attribute tagging.

---

### Exercise 2: Invite Guest Users in Bulk & via PowerShell

#### Task 1: Bulk User Invite via CSV
1. Navigated to **Entra ID** → **Users** → **All users** → **Bulk operations** → **Bulk invite**.
2. Downloaded the sample CSV template (`InviteUsersTemplate.csv`), populated the required fields, and uploaded the completed file.

#### `02-bulk-invite-file-uploaded.png`
![02 - Bulk Invite File Uploaded](./screenshots/02-bulk-invite-file-uploaded.png)

* **Visual Annotations & Highlights:**
  * **Lower Red Box:** Highlights the validation confirmation message **"File uploaded successfully"**.
  * **Upper Red Box:** Highlights the portal toast notification confirming submission of the bulk invite operation.
* **Technical Note:** Validates syntactic correctness of CSV headers, required redirection URL parameters, and external email structures before initiating backend batch jobs.

3. Monitored job execution in **Users** → **Bulk operation results**.

#### `03-bulk-operation-results.png`
![03 - Bulk Operation Results](./screenshots/03-bulk-operation-results.png)

* **Visual Annotations & Highlights:**
  * **Left Red Box:** Identifies the executed bulk import file (`InviteUsersTemplate.csv`).
  * **Right Red Box:** Highlights the **Status** column showing **Succeeded** for the **Invite Users** operation type.
* **Technical Note:** Demonstrates scalability for onboarding multiple external partner accounts simultaneously without manual single-user portal entry.

---

#### Task 2: Invite Guest Users with Microsoft Graph PowerShell
1. Opened PowerShell (v7.2+).
2. Authenticated into the tenant with delegated permissions (`Connect-MgGraph -Scopes "User.ReadWrite.All"`).
3. Configured invitation parameters (`$params`) specifying the target invited user email (`admin@fabrikam.com`) and redirect URL (`https://myapp.contoso.com`).
4. Executed the `New-MgInvitation` cmdlet to programmatically generate and dispatch the B2B invitation.

---

## 🔍 Troubleshooting & Environmental Discrepancies
* **Tenant Domain Restriction on B2B Invites:** During bulk CSV upload, entering an email address matching the host tenant's domain (`@DarrinPorIdentityLabs.onmicrosoft.com`) resulted in a failed job status. Entra ID strictly requires guest accounts to use external domains (e.g., `@gmail.com` or external partner tenant domains) to prevent invalid self-referential B2B invitations.
* **Lab Manual Template Discrepancy:** The lab manual provided a screenshot of the `Bulk Create Users` template (`version:v1.0` requiring passwords and principal names). The actual portal template for `Bulk Invite Users` (`InviteUsersTemplate.csv`) correctly prompts for `inviteeEmail`, `inviteRedirectURL`, and `sendEmail` without password parameters.

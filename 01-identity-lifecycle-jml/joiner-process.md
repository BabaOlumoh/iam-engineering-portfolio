# Joiner Process

## Purpose

The Joiner process ensures that new employees receive the correct identity and access before their first working day.

The process starts with an approved HR record and ends with a validated account in Active Directory, Microsoft Entra ID, and required business applications.

---

## Joiner Objectives

The process must ensure that:

- New starter accounts are created only from approved HR records
- User attributes are accurate and consistent
- Access is based on department, job role, location, and contract type
- Access is assigned through groups, not direct permissions
- The account syncs successfully to Microsoft Entra ID
- The user has the correct licences and baseline access
- Evidence is captured before the ticket is closed

---

## Joiner Trigger

A Joiner event is triggered when HR creates or approves a new employee record.

Minimum required conditions:

```text
EmploymentStatus = Active or PreHire
StartDate is present
Department is present
Manager is present
JobTitle is present
EmployeeID is unique
```

---

## Process Flow

```mermaid
flowchart TD
    A[Approved HR new starter record] --> B[Validate mandatory fields]
    B --> C{Validation passed?}
    C -->|No| D[Send to exception queue]
    C -->|Yes| E[Generate username and UPN]
    E --> F[Create AD user account]
    F --> G[Set user attributes]
    G --> H[Place user in correct OU]
    H --> I[Add baseline groups]
    I --> J[Add department and role groups]
    J --> K[Set temporary password]
    K --> L[Enable account according to start date policy]
    L --> M[Sync to Microsoft Entra ID]
    M --> N[Assign cloud groups and licences]
    N --> O[Validate access]
    O --> P[Record evidence]
    P --> Q[Notify Service Desk / Manager]
```

---

## Step-by-Step Procedure

### Step 1: Receive HR Record

The joiner process begins when HR provides a new starter record.

Required fields:

- Employee ID
- First name
- Last name
- Start date
- Department
- Job title
- Manager
- Office/location
- Contract type

---

### Step 2: Validate HR Data

Validation checks:

| Check | Expected Result |
|---|---|
| Employee ID is unique | No existing active user with same Employee ID |
| Name fields are populated | First name and last name exist |
| Start date is valid | Date is not blank or invalid |
| Department exists | Department maps to approved RBAC profile |
| Manager exists | Manager account exists and is active |
| Job title exists | Job title maps to access profile or exception path |
| Contract type is valid | Permanent, fixed-term, contractor, temporary |

If validation fails, the request should not proceed to full provisioning.

---

### Step 3: Generate Username and UPN

Example convention:

```text
First initial + surname
```

Example:

```text
Name: Amina Yusuf
Username: amina.yusuf
UPN: amina.yusuf@ogkareemu.live
Email: amina.yusuf@ogkareemu.live
```

If the username already exists:

```text
amina.yusuf2@ogkareemu.live
```

---

### Step 4: Create AD Account

The account should be created in the correct department OU.

Example:

```text
OU=Finance,OU=USERS,OU=OGKAREEMU,DC=ad,DC=iamhomelab,DC=com
```

Key attributes to set:

- givenName
- sn
- displayName
- sAMAccountName
- userPrincipalName
- mail
- employeeID
- department
- title
- manager
- company
- office
- description

---

### Step 5: Assign Baseline Groups

Every standard employee should receive baseline access.

Example baseline groups:

| Group | Purpose |
|---|---|
| GG_All_Employees | Standard employee group |
| LIC_M365_E3 | Microsoft 365 licence assignment |
| APP_Intranet_Users | Intranet access |
| APP_ServiceDeskPortal_Users | Self-service ticketing portal |
| CA_Require_MFA_AllUsers | Conditional Access MFA targeting |

---

### Step 6: Assign Department and Role Groups

Example for Finance Analyst:

| Access Type | Group |
|---|---|
| Department group | GG_DEPT_FINANCE_All |
| Role group | GG_DEPT_FINANCE_ASSOCIATE |
| File share read/write | DL_FS_Finance_READ |
| Finance app access | APP_FinanceSystem_Users |
| M365 standard licence | LIC_M365_E3 |

Access should be assigned through the RBAC mapping table, not manually guessed.

---

### Step 7: Password and First Sign-In Handling

Recommended approach:

- Generate a secure temporary password
- Require password change at next sign-in where appropriate
- Never store the temporary password in plain text logs
- Share password using an approved secure process
- For cloud access, enforce MFA registration or Temporary Access Pass if applicable

---

### Step 8: Sync to Microsoft Entra ID

After AD creation, the account should sync to Microsoft Entra ID.

Validation points:

- User appears in Entra ID
- UPN is correct
- Account source shows as synced from on-premises AD
- Department and job title are populated
- Group memberships are present
- Licence assignment is complete

---

### Step 9: Validate Access

Validation checks:

| Area | Check |
|---|---|
| AD | Account exists, enabled, correct OU |
| AD groups | Baseline and role groups assigned |
| Entra ID | User synced successfully |
| M365 | Licence assigned |
| MFA | User included in MFA policy scope |
| Applications | Required app groups assigned |
| Manager | Manager attribute populated |

---

## Joiner Checklist

| Task | Completed |
|---|---|
| HR record approved | ✓ |
<img width="1215" height="223" alt="Screenshot 2026-06-21 at 18 33 32" src="https://github.com/user-attachments/assets/7400b51e-46e7-4a9c-a557-0384d5d0cbef" />
| Mandatory fields validated | ✓ |

| Username and UPN generated | ✓ |
<img width="896" height="646" alt="Screenshot 2026-06-21 at 18 00 24" src="https://github.com/user-attachments/assets/311da314-850e-453b-93ce-6024db2b26e0" />
| AD account created | ✓ |
<img width="501" height="682" alt="Screenshot 2026-06-21 at 18 35 27" src="https://github.com/user-attachments/assets/4d0d17e0-ac4f-46f1-bcee-1eaf986daf81" />
| Correct OU assigned | ✓ |
<img width="497" height="674" alt="Screenshot 2026-06-21 at 18 09 51" src="https://github.com/user-attachments/assets/17819b1c-a46d-4d2c-abb3-98ed5e0a3f0c" />
| Attributes populated | ✓ |
<img width="501" height="682" alt="Screenshot 2026-06-21 at 18 35 27" src="https://github.com/user-attachments/assets/562da744-aca1-4ead-b9a2-8c3eddd0afb9" />
| Baseline groups assigned | ✓ |
<img width="498" height="675" alt="Screenshot 2026-06-21 at 18 06 47" src="https://github.com/user-attachments/assets/43127ba3-a193-457b-804d-0f66bda6e458" />
| Role groups assigned | ✓ |
<img width="498" height="675" alt="Screenshot 2026-06-21 at 18 06 47" src="https://github.com/user-attachments/assets/43127ba3-a193-457b-804d-0f66bda6e458" />
| Account synced to Entra ID | ✓ |
<img width="940" height="70" alt="Screenshot 2026-06-21 at 18 15 43" src="https://github.com/user-attachments/assets/8b3cdd88-4bed-41b6-a436-31485f014c3c" />
<img width="658" height="678" alt="Screenshot 2026-06-21 at 18 36 47" src="https://github.com/user-attachments/assets/07cf466e-86fe-4f46-aafe-dd63c945d742" />
| Licence assigned | ✓ |
<img width="980" height="363" alt="Screenshot 2026-06-21 at 18 51 36" src="https://github.com/user-attachments/assets/ac85e45c-81b7-4a9c-b5d7-6e67126efde1" />
| MFA/access policies apply | ✓ |
<img width="971" height="371" alt="Screenshot 2026-06-21 at 18 52 07" src="https://github.com/user-attachments/assets/bf1dfba0-87dd-4036-bec8-559d83403dbb" />
| Evidence captured | ✓ |
| Manager/Service Desk notified | ✓ |
| Lifecycle Workflow | ✓ |
<img width="810" height="510" alt="Screenshot 2026-06-21 at 18 26 39" src="https://github.com/user-attachments/assets/91ff7bda-5c7c-455c-8723-86756a6edfd7" />
<img width="977" height="438" alt="Screenshot 2026-06-21 at 18 18 27" src="https://github.com/user-attachments/assets/ad73a25c-e47f-4544-9572-63b9be75cc98" />

---

## Joiner Example

| Field | Value |
|---|---|
| Employee ID | EMP1001 |
| Name | Amina Yusuf |
| Department | Finance |
| Job Title | Finance Analyst |
| Manager | Emma Watson |
| Office | Milton Keynes |
| Contract Type | Permanent |
| Start Date | 2026-07-01 |
| Username | ayusuf |
| UPN | amina.yusuf@ogkareemu.live |

Expected access:

- GG_All_Employees
- GG_FIN_All
- GG_FIN_Analysts
- DL_FS_Finance_RW
- APP_FinanceSystem_Users
- LIC_M365_E3
- CA_Require_MFA_AllUsers

---

## Common Joiner Issues and Fixes

| Issue | Likely Cause | Fix |
|---|---|---|
| User not created | Missing mandatory HR field | Ask HR to correct record |
| Duplicate username | Existing account has same naming pattern | Generate alternate username |
| User not syncing to Entra | AD Connect sync issue or OU not in scope | Check sync scope and run delta sync |
| Licence not assigned | User missing licence group | Add correct licence assignment group |
| Manager missing | Manager account not found | Confirm manager with HR |
| Incorrect access | Wrong department/job title mapping | Review RBAC mapping table |

---

## Evidence to Capture

- HR source record or ticket reference
- AD account properties
- AD group membership
- Entra ID user profile
- Licence assignment
- MFA/Conditional Access scope, where possible
- Completion note confirming account is ready

---

## Completion Criteria

A Joiner request is complete when:

- The user account exists in AD
- The user has the correct attributes
- The account has synced to Entra ID
- Baseline and role-based access are assigned
- Required licences are assigned
- Evidence is saved
- Service Desk or the hiring manager has been notified

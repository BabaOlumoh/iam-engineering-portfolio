# RBAC Role Matrix

## Purpose

This document defines the role-based access matrix for the hybrid IAM lab.

The purpose of the matrix is to show how access is assigned based on business role, not individual preference. This supports:

- Least privilege
- Consistent onboarding
- Controlled mover process
- Faster leaver access removal
- Easier audit reviews
- Reduced access errors

---

## RBAC Design Approach

Access is assigned using this logic:

```text
Department + Job Role + Location + Employment Status = Approved Access
```

Example:

```text
Finance + Accounts Payable Clerk + Milton Keynes + Active = Finance AP access
```

Access must not be granted directly to user accounts.

Instead, users are added to role-based groups, and those groups inherit access through resource-based groups.

---

## Group Types Used

| Group Type | Purpose | Example |
|---|---|---|
| Global Group | Represents users with the same business role | `GG_FIN_AP_Clerk` |
| Domain Local Group | Represents access to a specific resource | `DL_FS_Finance_AP_RW` |
| Entra Security Group | Grants access to cloud apps or policies | `AZG_APP_ServiceDesk_Users` |
| Microsoft 365 Group | Collaboration access | `M365_FIN_Team` |
| Privileged Group | Admin or elevated access | `GG_IT_Identity_Admins` |

---

## Baseline Access Matrix

Baseline access is assigned to most standard employees depending on employment type.

| Access Item | Group | Standard User | Contractor | Guest | Notes |
|---|---|---:|---:|---:|---|
| Microsoft 365 standard apps | `GG_M365_StandardUser` | Yes | Conditional | No | Grants baseline productivity access |
| Company intranet | `GG_APP_Intranet_Users` | Yes | Yes | No | Internal users only |
| Teams access | `GG_M365_Teams_Users` | Yes | Yes | Conditional | Guest access handled separately |
| VPN access | `GG_NET_VPN_Users` | Conditional | Conditional | No | Requires manager approval |
| Self-service password reset | `GG_ID_SSPR_Enabled` | Yes | Yes | No | Used for identity recovery |
| MFA registration | `GG_ID_MFA_Required` | Yes | Yes | Conditional | Enforced using Conditional Access |

---

## Department Role Matrix

### Finance Department

| Business Role | AD Role Group | Resource Groups Granted | Access Level | Approval Required |
|---|---|---|---|---|
| Finance Baseline User | `GG_FIN_Baseline` | `DL_FS_Finance_General_R` | Read | Manager |
| Accounts Payable Clerk | `GG_FIN_AP_Clerk` | `DL_FS_Finance_AP_RW`, `DL_App_ERP_AP_User` | Read/Write | Finance Manager |
| Accounts Receivable Clerk | `GG_FIN_AR_Clerk` | `DL_FS_Finance_AR_RW`, `DL_App_ERP_AR_User` | Read/Write | Finance Manager |
| Payroll Officer | `GG_FIN_Payroll_Officer` | `DL_FS_Finance_Payroll_RW`, `DL_App_Payroll_User` | Sensitive RW | Finance Head + HR |
| Finance Manager | `GG_FIN_Manager` | `DL_FS_Finance_All_RW`, `DL_App_ERP_Finance_Manager` | Elevated Department Access | Head of Finance |

### Human Resources Department

| Business Role | AD Role Group | Resource Groups Granted | Access Level | Approval Required |
|---|---|---|---|---|
| HR Baseline User | `GG_HR_Baseline` | `DL_FS_HR_General_R` | Read | HR Manager |
| HR Advisor | `GG_HR_Advisor` | `DL_FS_HR_Cases_RW`, `DL_App_HRIS_User` | Read/Write | HR Manager |
| Recruitment Officer | `GG_HR_Recruitment` | `DL_FS_HR_Recruitment_RW`, `DL_App_ATS_User` | Read/Write | HR Manager |
| Payroll Liaison | `GG_HR_Payroll_Liaison` | `DL_FS_HR_Payroll_R` | Read | HR + Finance Approval |
| HR Manager | `GG_HR_Manager` | `DL_FS_HR_All_RW`, `DL_App_HRIS_Manager` | Elevated Department Access | Head of HR |

### IT Department

| Business Role | AD Role Group | Resource Groups Granted | Access Level | Approval Required |
|---|---|---|---|---|
| IT Baseline User | `GG_IT_Baseline` | `DL_FS_IT_General_R` | Read | IT Manager |
| Service Desk Analyst | `GG_IT_ServiceDesk` | `DL_App_Ticketing_Agent`, `DL_FS_IT_KB_RW` | Operational Support | IT Service Manager |
| Infrastructure Engineer | `GG_IT_Infrastructure` | `DL_FS_IT_Infrastructure_RW`, `DL_App_Monitoring_Admin` | Technical Admin | Infrastructure Manager |
| IAM Analyst | `GG_IT_IAM_Analyst` | `DL_App_IdentityTools_User`, `DL_FS_IAM_Runbooks_RW` | Identity Operations | IAM Manager |
| Identity Administrator | `GG_IT_Identity_Admins` | `AZR_Entra_UserAdmin_Eligible` | Privileged | PIM Approval |

### Sales Department

| Business Role | AD Role Group | Resource Groups Granted | Access Level | Approval Required |
|---|---|---|---|---|
| Sales Baseline User | `GG_SALES_Baseline` | `DL_FS_Sales_General_R` | Read | Sales Manager |
| Sales Executive | `GG_SALES_Executive` | `DL_App_CRM_User`, `DL_FS_Sales_Accounts_RW` | Read/Write | Sales Manager |
| Account Manager | `GG_SALES_AccountManager` | `DL_App_CRM_AccountManager`, `DL_FS_Sales_Accounts_RW` | Read/Write | Sales Manager |
| Sales Manager | `GG_SALES_Manager` | `DL_App_CRM_Manager`, `DL_FS_Sales_All_RW` | Department Manager | Head of Sales |

### Operations Department

| Business Role | AD Role Group | Resource Groups Granted | Access Level | Approval Required |
|---|---|---|---|---|
| Operations Baseline User | `GG_OPS_Baseline` | `DL_FS_Operations_General_R` | Read | Operations Manager |
| Operations Analyst | `GG_OPS_Analyst` | `DL_App_Operations_Reporting`, `DL_FS_Operations_Reports_RW` | Read/Write | Operations Manager |
| Team Leader | `GG_OPS_TeamLeader` | `DL_FS_Operations_Team_RW`, `DL_App_Workforce_Manager` | Team Access | Operations Manager |
| Operations Manager | `GG_OPS_Manager` | `DL_FS_Operations_All_RW`, `DL_App_Operations_Admin` | Department Manager | Head of Operations |

---

## Sensitive Access Matrix

Sensitive access requires additional controls.

| Access Type | Group | Risk | Required Approval | Review Frequency |
|---|---|---|---|---|
| Payroll data | `GG_FIN_Payroll_Officer` | High | HR + Finance | Monthly |
| HR case files | `GG_HR_Advisor` | High | HR Manager | Quarterly |
| Entra user administration | `GG_IT_Identity_Admins` | High | IAM Manager + PIM | Monthly |
| VPN access | `GG_NET_VPN_Users` | Medium | Line Manager | Quarterly |
| File share full control | `DL_FS_*_FC` | High | System Owner | Monthly |
| Break-glass account access | `GG_ID_BreakGlass_Monitored` | Critical | Security Lead | Immediate review |

---

## Cloud Access Matrix

| Cloud Resource | Entra Group | Eligible Users | Notes |
|---|---|---|---|
| Microsoft 365 E3/E5 licence | `AZG_LIC_M365_Standard` | Standard employees | Can be licence assignment group |
| Teams calling | `AZG_LIC_TeamsPhone` | Approved users | Requires cost approval |
| Service Desk application | `AZG_APP_ServiceDesk_Users` | IT Service Desk | Used for SaaS SSO assignment |
| HR system | `AZG_APP_HRIS_Users` | HR users | Sensitive app |
| Finance ERP | `AZG_APP_ERP_Finance` | Finance users | Requires Finance approval |
| Identity governance portal | `AZG_APP_IAM_Tools` | IAM team | Restricted |

---

## Joiner Access Example

### User Details

| Attribute | Value |
|---|---|
| Name | Aisha Bello |
| Department | Finance |
| Job Title | Accounts Payable Clerk |
| Location | Milton Keynes |
| Manager | Finance Manager |
| Employment Status | Active |

### Assigned Groups

| Group | Reason |
|---|---|
| `GG_M365_StandardUser` | Baseline cloud access |
| `GG_ID_MFA_Required` | Security baseline |
| `GG_FIN_Baseline` | Department baseline |
| `GG_FIN_AP_Clerk` | Role-based Finance AP access |

### Effective Access

| Resource | Access |
|---|---|
| Microsoft 365 | Standard user access |
| Finance general folder | Read |
| Finance AP folder | Read/Write |
| ERP AP module | User access |

---

## Mover Access Example

### Scenario

A user moves from **Finance AP Clerk** to **HR Advisor**.

### Remove Old Access

```text
GG_FIN_Baseline
GG_FIN_AP_Clerk
```

### Add New Access

```text
GG_HR_Baseline
GG_HR_Advisor
```

### Control

The old access must be removed before new access is added.

This prevents privilege creep where a user keeps Finance access after moving into HR.

---

## Leaver Access Example

### Remove User From Role Groups

```text
GG_FIN_Baseline
GG_FIN_AP_Clerk
GG_M365_StandardUser
GG_NET_VPN_Users
```

### Additional Actions

- Disable AD account
- Revoke Entra sessions
- Remove licences where appropriate
- Remove privileged roles
- Record final group membership export
- Confirm mailbox and data retention process

---

## Review Process

| Review Type | Frequency | Owner |
|---|---|---|
| Department access review | Quarterly | Department Manager |
| Privileged access review | Monthly | IAM/Security Manager |
| Leaver access check | Daily/Weekly | Service Desk/IAM |
| Sensitive data access review | Monthly | Data Owner |
| Group naming and hygiene review | Quarterly | IAM Team |

---

## Success Criteria

The RBAC design is successful when:

- No user has direct permissions on resources
- Every group has a clear owner and purpose
- Every role has documented baseline access
- Movers lose old access before gaining new access
- Leavers are removed from all active access paths
- Sensitive access requires additional approval
- Access can be explained to an auditor using the matrix

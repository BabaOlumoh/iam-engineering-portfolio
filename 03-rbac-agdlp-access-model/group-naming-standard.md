# Group Naming Standard

## Purpose

This document defines a consistent naming standard for Active Directory and Microsoft Entra groups used in the RBAC and AGDLP access model.

A good naming standard helps IAM, Service Desk, Security and audit teams quickly understand:

- What the group is used for
- Who owns the group
- Whether it is role-based or resource-based
- Whether it grants sensitive access
- Whether it is used on-premises or in the cloud

---

## Naming Principles

Group names must be:

- Clear
- Consistent
- Searchable
- Short enough to manage easily
- Linked to a business purpose
- Avoid personal names unless absolutely required
- Avoid vague terms such as `GeneralAccess`, `TempGroup` or `TestGroup`

---

## Group Naming Format

### Standard Format

```text
<PREFIX>_<SCOPE>_<RESOURCE-OR-ROLE>_<ACCESS>
```

Example:

```text
GG_FIN_AP_Clerk
DL_FS_Finance_AP_RW
AZG_APP_HRIS_Users
M365_HR_Team
```

---

## Prefix Standard

| Prefix | Meaning | Platform | Example |
|---|---|---|---|
| `GG` | Global Group | Active Directory | `GG_FIN_AP_Clerk` |
| `DL` | Domain Local Group | Active Directory | `DL_FS_Finance_AP_RW` |
| `UG` | Universal Group | Active Directory | `UG_AllStaff` |
| `AZG` | Azure/Entra Security Group | Entra ID | `AZG_APP_HRIS_Users` |
| `M365` | Microsoft 365 Group | Entra ID/M365 | `M365_HR_Team` |
| `PIM` | Privileged Identity Management group | Entra ID | `PIM_Entra_UserAdmin_Eligible` |
| `DYN` | Dynamic Group | Entra ID | `DYN_All_Finance_Users` |

---

## Scope Standard

| Scope | Meaning |
|---|---|
| `FIN` | Finance |
| `HR` | Human Resources |
| `IT` | Information Technology |
| `OPS` | Operations |
| `SALES` | Sales |
| `MKT` | Marketing |
| `ALL` | All users |
| `EXT` | External or guest users |
| `PRIV` | Privileged access |
| `SEC` | Security controls |

---

## Resource Type Standard

| Code | Meaning | Example |
|---|---|---|
| `FS` | File Share | `DL_FS_Finance_AP_RW` |
| `APP` | Application | `AZG_APP_HRIS_Users` |
| `LIC` | Licence | `AZG_LIC_M365_Standard` |
| `VPN` | VPN Access | `GG_NET_VPN_Users` |
| `MAIL` | Mailbox Access | `DL_MAIL_SharedFinance_RW` |
| `SP` | SharePoint | `AZG_SP_HR_Policies_RW` |
| `CA` | Conditional Access targeting | `AZG_CA_MFA_Required` |
| `IAM` | Identity tools | `GG_IT_IAM_Analyst` |

---

## Access Level Standard

| Suffix | Meaning |
|---|---|
| `R` | Read |
| `RW` | Read/Write |
| `FC` | Full Control |
| `User` | Standard application user |
| `Admin` | Application or system administrator |
| `Approver` | Can approve requests |
| `Eligible` | Eligible privileged role through PIM |
| `Active` | Standing active privileged assignment |
| `Owner` | Group or resource owner |

---

## Examples

### Role-Based Global Groups

```text
GG_FIN_Baseline
GG_FIN_AP_Clerk
GG_FIN_Manager
GG_HR_Advisor
GG_IT_ServiceDesk
GG_IT_IAM_Analyst
GG_OPS_TeamLeader
```

These groups represent people or roles.

Users are added to these groups.

---

### Resource-Based Domain Local Groups

```text
DL_FS_Finance_General_R
DL_FS_Finance_AP_RW
DL_FS_HR_Cases_RW
DL_FS_IT_KB_RW
DL_App_Ticketing_Agent
DL_App_ERP_AP_User
```

These groups represent access to resources.

Permissions are assigned to these groups.

---

### Entra ID Groups

```text
AZG_LIC_M365_Standard
AZG_APP_HRIS_Users
AZG_APP_ServiceDesk_Users
AZG_CA_MFA_Required
AZG_CA_BlockLegacyAuth_Target
AZG_SP_HR_Policies_RW
```

These groups are used for cloud application access, licence assignment or Conditional Access targeting.

---

### Privileged Access Groups

```text
PIM_Entra_UserAdmin_Eligible
PIM_Entra_GroupsAdmin_Eligible
PIM_Entra_ConditionalAccessAdmin_Eligible
PIM_Azure_Subscription_Contributor_Eligible
```

These groups are used for just-in-time privileged access and should be reviewed frequently.

---

## AGDLP Naming Example

### Business Requirement

Finance AP Clerks need read/write access to the Accounts Payable file share.

### Correct Design

```text
User Account
   ↓
GG_FIN_AP_Clerk
   ↓
DL_FS_Finance_AP_RW
   ↓
\\fileserver\Finance\AccountsPayable - Modify Permission
```

### Why This Is Correct

- User is placed into a role group
- Role group is nested into a resource group
- Permission is assigned only to the resource group
- Access can be removed by removing the user from the role group
- Auditors can clearly see why the user has access

---

## Incorrect Group Naming Examples

Avoid names like:

```text
FinanceAccess
JohnFinance
TempAccess1
NewGroup
Managers
ReadWrite
AppUsers
```

These are poor because they do not show:

- Platform
- Owner
- Resource
- Access level
- Business purpose

---

## Group Description Standard

Every group should have a useful description.

### Description Format

```text
Purpose: <what this group grants>. Owner: <business/system owner>. Approval: <approval required>. Review: <review frequency>.
```

### Example

```text
Purpose: Grants read/write access to the Finance Accounts Payable file share. Owner: Finance Manager. Approval: Finance Manager required. Review: Quarterly.
```

---

## Group Ownership Standard

Every group must have an owner.

| Group Type | Owner |
|---|---|
| Department role group | Department Manager |
| File share permission group | Data Owner |
| Application access group | Application Owner |
| Licence group | IT/IAM Team |
| Privileged access group | Security/IAM Manager |
| Conditional Access target group | Security/IAM Team |

---

## Group Review Frequency

| Group Type | Review Frequency |
|---|---|
| Standard department access | Quarterly |
| Sensitive business data | Monthly |
| Privileged admin access | Monthly |
| Contractor access | Monthly |
| Guest access | Monthly or expiry-based |
| Licence groups | Quarterly |

---

## Group Lifecycle Standard

Each group should have a lifecycle:

1. Request group creation
2. Confirm business owner
3. Confirm resource or role purpose
4. Create group using naming standard
5. Add description and owner
6. Assign permissions or nesting
7. Test with pilot user
8. Document in access matrix
9. Include in access review schedule
10. Retire group when no longer required

---

## PowerShell Examples

### Create a Role-Based Global Group

```powershell
New-ADGroup `
  -Name "GG_FIN_AP_Clerk" `
  -SamAccountName "GG_FIN_AP_Clerk" `
  -GroupScope Global `
  -GroupCategory Security `
  -Path "OU=Groups,DC=ad,DC=iamhomelab,DC=com" `
  -Description "Purpose: Finance AP Clerk role group. Owner: Finance Manager. Approval: Finance Manager required. Review: Quarterly."
```

### Create a Resource-Based Domain Local Group

```powershell
New-ADGroup `
  -Name "DL_FS_Finance_AP_RW" `
  -SamAccountName "DL_FS_Finance_AP_RW" `
  -GroupScope DomainLocal `
  -GroupCategory Security `
  -Path "OU=Groups,DC=ad,DC=iamhomelab,DC=com" `
  -Description "Purpose: Grants read/write access to Finance AP file share. Owner: Finance Manager. Approval: Finance Manager required. Review: Quarterly."
```

### Nest Role Group Into Permission Group

```powershell
Add-ADGroupMember `
  -Identity "DL_FS_Finance_AP_RW" `
  -Members "GG_FIN_AP_Clerk"
```

### Add User to Role Group

```powershell
Add-ADGroupMember `
  -Identity "GG_FIN_AP_Clerk" `
  -Members "aisha.bello"
```

---

## Naming Standard Checklist

Before creating a group, confirm:

- [ ] Does the group name follow the standard?
- [ ] Is the group purpose clear?
- [ ] Is the group owner known?
- [ ] Is the approval route known?
- [ ] Is the access level clear?
- [ ] Is this role-based or resource-based?
- [ ] Is this an on-prem AD group or Entra group?
- [ ] Is the group documented in the RBAC matrix?
- [ ] Is it included in an access review cycle?

# Test Evidence

## Purpose

This document records the test evidence required to prove that the Joiner, Mover, and Leaver processes work correctly.

The evidence is designed for a GitHub portfolio project and can also be used as an operational audit checklist.

---

## Test Environment

| Item | Value |
|---|---|
| Lab Name | IAM Home Lab |
| AD Domain | ad.iamhomelab.com |
| Domain Controller | dc01.ad.iamhomelab.com |
| Cloud Directory | Microsoft Entra ID lab tenant |
| Sync Method | Microsoft Entra Connect Sync / Cloud Sync |
| HR Source | HR CSV / test HR dataset |
| Test User | Amina Yusuf |
| Test Username | ayusuf |
| Test UPN | ayusuf@iamhomelab.com |

---

## Evidence Naming Standard

Save screenshots in the `screenshots/` folder using clear names.

Recommended naming format:

```text
jml-[event]-[step-number]-[description].png
```

Examples:

```text
jml-joiner-01-hr-record.png
jml-joiner-02-ad-user-created.png
jml-joiner-03-ad-groups-assigned.png
jml-joiner-04-entra-user-synced.png
jml-mover-01-before-access.png
jml-mover-02-old-groups-removed.png
jml-mover-03-new-groups-added.png
jml-leaver-01-account-disabled.png
jml-leaver-02-sessions-revoked.png
jml-leaver-03-licence-removed.png
```

---

## Test Case 1: Joiner

### Objective

Confirm that a new starter can be created from HR data and receive the correct baseline, department, role, and cloud access.

### Test Data

| Field | Value |
|---|---|
| Employee ID | EMP1001 |
| First Name | Amina |
| Last Name | Yusuf |
| Department | Finance |
| Job Title | Finance Analyst |
| Manager | Sarah Johnson |
| Office | Milton Keynes |
| Contract Type | Permanent |
| Start Date | 2026-07-01 |

### Expected Access

| Access | Group |
|---|---|
| Baseline employee access | GG_All_Employees |
| Finance department access | GG_FIN_All |
| Finance analyst role access | GG_FIN_Analysts |
| Finance file share | DL_FS_Finance_RW |
| Finance application | APP_FinanceSystem_Users |
| M365 licence | LIC_M365_E3 |
| MFA policy scope | CA_Require_MFA_AllUsers |

### Evidence Checklist

| Evidence | Screenshot Name | Status |
|---|---|---|
| HR source record exists | jml-joiner-01-hr-record.png | ☐ |
| AD account created | jml-joiner-02-ad-user-created.png | ☐ |
| AD attributes populated | jml-joiner-03-ad-attributes.png | ☐ |
| AD group membership assigned | jml-joiner-04-ad-groups-assigned.png | ☐ |
| User synced to Entra ID | jml-joiner-05-entra-user-synced.png | ☐ |
| Licence assigned | jml-joiner-06-licence-assigned.png | ☐ |
| Final validation complete | jml-joiner-07-validation-complete.png | ☐ |

### Pass Criteria

The Joiner test passes when:

- User exists in AD
- User has correct attributes
- User has correct baseline and role groups
- User syncs to Entra ID
- User has correct licence
- Evidence is captured

### Result

```text
Status: Pending evidence upload
Tester:
Date:
Notes:
```

---

## Test Case 2: Mover

### Objective

Confirm that a user moving from Finance to IT loses old Finance access and receives only the approved IT/IAM access.

### Test Data

| Field | Before | After |
|---|---|---|
| Department | Finance | IT |
| Job Title | Finance Analyst | IAM Analyst |
| Manager | Finance Manager | IT Security Manager |
| Office | Milton Keynes | Milton Keynes |

### Access to Remove

| Group | Reason |
|---|---|
| GG_FIN_All | No longer in Finance |
| GG_FIN_Analysts | No longer Finance Analyst |
| DL_FS_Finance_RW | Finance file access no longer required |
| APP_FinanceSystem_Users | Finance application no longer required |

### Access to Add

| Group | Reason |
|---|---|
| GG_IT_All | IT department access |
| GG_IT_IAM_Analysts | IAM analyst role access |
| APP_ServiceNow_ITIL | ITSM access |
| APP_Entra_AdminPortal_ReadOnly | Identity administration visibility |
| PIM_Entra_UserAdmin_Eligible | Eligible privileged role access with approval |

### Evidence Checklist

| Evidence | Screenshot Name | Status |
|---|---|---|
| Before group membership captured | jml-mover-01-before-access.png | ☐ |
| HR role change record captured | jml-mover-02-hr-change.png | ☐ |
| Old Finance groups removed | jml-mover-03-old-groups-removed.png | ☐ |
| New IT/IAM groups added | jml-mover-04-new-groups-added.png | ☐ |
| AD attributes updated | jml-mover-05-ad-attributes-updated.png | ☐ |
| Entra ID reflects update | jml-mover-06-entra-updated.png | ☐ |
| After group membership captured | jml-mover-07-after-access.png | ☐ |
| Approval evidence for privileged access | jml-mover-08-approval-evidence.png | ☐ |

### Pass Criteria

The Mover test passes when:

- Finance access is removed
- IT/IAM access is assigned
- Privileged access is approved and eligible rather than permanently assigned where possible
- User attributes are updated
- Entra ID reflects the change
- Evidence shows before and after access state

### Result

```text
Status: Pending evidence upload
Tester:
Date:
Notes:
```

---

## Test Case 3: Standard Leaver

### Objective

Confirm that a standard leaver is disabled, removed from access groups, cloud sessions are revoked, and licences are reclaimed.

### Test Data

| Field | Value |
|---|---|
| Employee ID | EMP1001 |
| User | Amina Yusuf |
| Username | ayusuf |
| UPN | ayusuf@iamhomelab.com |
| Department | IT |
| Job Title | IAM Analyst |
| Final Working Date | 2026-07-31 |
| Leaver Type | Standard |

### Evidence Checklist

| Evidence | Screenshot Name | Status |
|---|---|---|
| HR leaver record captured | jml-leaver-01-hr-leaver-record.png | ☐ |
| AD account disabled | jml-leaver-02-ad-account-disabled.png | ☐ |
| Account moved to Disabled Users OU | jml-leaver-03-disabled-users-ou.png | ☐ |
| AD groups removed | jml-leaver-04-ad-groups-removed.png | ☐ |
| Entra account disabled after sync | jml-leaver-05-entra-disabled.png | ☐ |
| Sessions revoked | jml-leaver-06-sessions-revoked.png | ☐ |
| Licence removed | jml-leaver-07-licence-removed.png | ☐ |
| Mailbox/data action recorded | jml-leaver-08-mailbox-data-handling.png | ☐ |
| Closure evidence captured | jml-leaver-09-closure-note.png | ☐ |

### Pass Criteria

The Standard Leaver test passes when:

- AD account is disabled
- Account is moved to Disabled Users OU
- Cloud sessions are revoked
- Access groups are removed
- Licence is removed or retained with approved reason
- Evidence is captured

### Result

```text
Status: Pending evidence upload
Tester:
Date:
Notes:
```

---

## Test Case 4: Emergency Leaver

### Objective

Confirm that an emergency leaver can be disabled immediately and contained before full cleanup is completed.

### Emergency Process Order

| Step | Action | Evidence |
|---|---|---|
| 1 | Disable AD account | Screenshot of disabled account |
| 2 | Revoke Entra sessions | Screenshot or audit log |
| 3 | Remove privileged access | Group/role evidence |
| 4 | Notify Security/IAM lead | Ticket note |
| 5 | Complete standard cleanup | Group/licence/mailbox evidence |
| 6 | Record timeline | Closure note |

### Evidence Checklist

| Evidence | Screenshot Name | Status |
|---|---|---|
| Emergency request received | jml-emergency-leaver-01-request.png | ☐ |
| Account disabled immediately | jml-emergency-leaver-02-disabled.png | ☐ |
| Sessions revoked | jml-emergency-leaver-03-sessions-revoked.png | ☐ |
| Privileged access removed | jml-emergency-leaver-04-privileged-access-removed.png | ☐ |
| Security notified | jml-emergency-leaver-05-security-notified.png | ☐ |
| Cleanup completed | jml-emergency-leaver-06-cleanup-complete.png | ☐ |

### Pass Criteria

The Emergency Leaver test passes when:

- The account is disabled first
- Sessions are revoked immediately after disablement
- Privileged access is removed quickly
- Cleanup is completed after containment
- Timeline evidence is captured

### Result

```text
Status: Pending evidence upload
Tester:
Date:
Notes:
```

---

## Test Summary

| Test Case | Status | Notes |
|---|---|---|
| Joiner | Pending | Add screenshots after lab execution |
| Mover | Pending | Add before/after access screenshots |
| Standard Leaver | Pending | Add disablement and licence evidence |
| Emergency Leaver | Pending | Add containment timeline evidence |

---

## Final Evidence Statement

When screenshots are added, this project will provide evidence that the JML process can:

- Create users from HR-approved data
- Assign access through RBAC groups
- Prevent access creep during role changes
- Disable leavers from the authoritative source
- Revoke cloud sessions
- Remove access and reclaim licences
- Produce audit-ready evidence for IAM operations

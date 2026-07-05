# PIM Approval Workflow

## Purpose

This document explains the approval workflow for activating privileged Microsoft Entra roles using Privileged Identity Management.

The workflow is designed to make sure privileged access is:

- Requested only when needed
- Protected by MFA
- Supported by a valid business reason
- Approved for high-risk roles
- Automatically time-limited
- Recorded for audit review

---

## Workflow Overview

```text
Admin needs privileged access
        |
        v
Admin opens Microsoft Entra PIM
        |
        v
Admin selects eligible role
        |
        v
Admin completes activation request
        |
        +--> MFA challenge
        +--> Justification entered
        +--> Ticket/reference entered where required
        +--> Duration selected
        |
        v
Is approval required?
        |
        +--> No  -> Role activates after successful checks
        |
        +--> Yes -> Request sent to approver
                    |
                    v
              Approver reviews request
                    |
                    +--> Deny -> Access not granted
                    |
                    +--> Approve -> Role activates
        |
        v
Admin completes task
        |
        v
Role expires automatically
        |
        v
Audit evidence reviewed
```

---

## Roles Requiring Approval

The following roles should require approval before activation in this lab:

| Role | Reason Approval Is Required |
|---|---|
| Privileged Role Administrator | Can assign and manage privileged access |
| Conditional Access Administrator | Can create policies that may block access tenant-wide |
| Security Administrator | Can modify security settings and view sensitive security data |
| Exchange Administrator | Can affect mail flow and mailbox access |
| SharePoint Administrator | Can affect data access across SharePoint and OneDrive |
| Intune Administrator | Can affect device compliance and management controls |

---

## Roles Where Approval May Be Optional

Some roles may not require approval if the organisation wants faster operational support. However, they should still require MFA and justification.

| Role | Suggested Control |
|---|---|
| Authentication Administrator | MFA + justification; approval optional |
| User Administrator | MFA + justification; approval optional or required for production |
| Groups Administrator | MFA + justification; approval optional, but required for privileged groups |
| Security Reader | MFA; justification optional |
| Global Reader | MFA; justification optional |

---

## Activation Request Requirements

When a user activates a privileged role, they should provide:

| Requirement | Example |
|---|---|
| Business justification | `Resetting MFA for user after verified Service Desk request` |
| Ticket number | `INC10423` |
| Expected duration | `1 hour` |
| Role requested | `Authentication Administrator` |
| Scope of work | `Reset authentication methods only` |

---

## Example Activation Requests

### Valid Request 1: MFA Reset

| Field | Value |
|---|---|
| Requester | `adm.servicedesk@0gkareemu.live` |
| Role | Authentication Administrator |
| Duration | 2 hours |
| Justification | `INC10231 - Verified user identity and resetting MFA methods after phone replacement.` |
| Approval | Not required or approved by Service Desk Lead |

### Valid Request 2: Conditional Access Change

| Field | Value |
|---|---|
| Requester | `adm.security@0gkareemu.live` |
| Role | Conditional Access Administrator |
| Duration | 1 hour |
| Justification | `CHG2044 - Update report-only MFA policy to exclude approved break-glass accounts.` |
| Approval | Required |

### Invalid Request 1: Vague Justification

| Field | Value |
|---|---|
| Role | Privileged Role Administrator |
| Justification | `Need admin access` |
| Decision | Deny |
| Reason | No ticket number, unclear scope, no business justification |

### Invalid Request 2: Excessive Duration

| Field | Value |
|---|---|
| Role | Conditional Access Administrator |
| Duration | 8 hours |
| Decision | Deny or reduce duration |
| Reason | High-risk role should use shorter activation window |

---

## Approver Responsibilities

Approvers should check:

1. Is the requested role appropriate for the task?
2. Is the justification clear?
3. Is a valid ticket or change reference provided?
4. Is the requested duration reasonable?
5. Is the requester authorised to perform the task?
6. Is there a safer lower-privilege role that could be used instead?
7. Is this request linked to a planned change, incident, or approved support request?

---

## Approval Decision Guide

| Condition | Decision |
|---|---|
| Clear business reason and correct role | Approve |
| Valid ticket/change reference provided | Approve if role and duration are appropriate |
| Vague justification | Deny |
| No ticket for high-risk role | Deny |
| Excessive duration | Deny or request shorter activation |
| Wrong role requested | Deny and recommend correct role |
| Requester is trying to approve own high-risk access | Deny or route to independent approver |

---

## Suggested PIM Configuration Steps

### 1. Open PIM

Go to:

```text
Microsoft Entra admin center
> ID Governance
> Privileged Identity Management
> Microsoft Entra roles
```

### 2. Select Role Settings

Choose the role to configure, for example:

```text
Conditional Access Administrator
```

### 3. Configure Activation Settings

Recommended settings for high-risk roles:

| Setting | Value |
|---|---|
| Maximum activation duration | 1 hour |
| Require MFA on activation | Enabled |
| Require justification | Enabled |
| Require ticket information | Enabled |
| Require approval to activate | Enabled |
| Approvers | IAM Lead / Security Manager |

### 4. Configure Notifications

Enable notifications for:

- User activation
- Approver request
- Administrator notification
- Role assignment changes

### 5. Save and Test

Test with a lab admin account:

- Eligible assignment
- Activation request
- Approval request
- Successful activation
- Audit log entry

---

## Emergency Access Exception

Break-glass accounts should not rely on normal PIM approval workflows because they are used when normal access controls fail.

However, they must be heavily monitored.

Recommended controls:

- Separate break-glass accounts from daily admin accounts
- Exclude only from policies required to prevent lockout
- Monitor every sign-in
- Store credentials securely
- Review monthly
- Test periodically
- Keep usage documented

---

## Runbook: PIM Activation for MFA Reset

1. Verify the user request in the ticketing system.
2. Confirm the user's identity using the approved identity verification process.
3. Sign in with the admin account.
4. Open Microsoft Entra PIM.
5. Activate `Authentication Administrator`.
6. Complete MFA challenge.
7. Enter the ticket number and justification.
8. Reset the user's authentication methods.
9. Add ticket notes showing action taken.
10. Allow role to expire or deactivate manually.
11. Confirm audit log entry exists.

---

## Runbook: PIM Activation for Conditional Access Change

1. Confirm approved change request exists.
2. Confirm backout plan exists.
3. Confirm break-glass accounts are available.
4. Sign in using admin account.
5. Activate `Conditional Access Administrator` through PIM.
6. Complete MFA challenge.
7. Enter change reference and justification.
8. Wait for approval.
9. Apply policy change in report-only mode where possible.
10. Validate sign-in impact.
11. Move to enforcement only after successful testing.
12. Capture audit evidence.
13. Allow role to expire or deactivate manually.

---

## Controls Tested

| Control | Expected Result |
|---|---|
| MFA required during activation | User must complete MFA before role activation |
| Justification required | Activation cannot proceed without reason |
| Approval required | High-risk role waits for approver decision |
| Time-bound activation | Role expires automatically |
| Audit logging | Activation appears in PIM audit history |
| Least privilege | User activates only the role needed for task |


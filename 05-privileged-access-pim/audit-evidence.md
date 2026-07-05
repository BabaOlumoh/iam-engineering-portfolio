# PIM Audit Evidence

## Purpose

This document defines the evidence to capture for the Microsoft Entra PIM lab.

The goal is to prove that privileged access is:

- Assigned correctly
- Activated securely
- Approved where required
- Time-bound
- Logged for audit review

---

## Evidence Collection Checklist

| Evidence ID | Evidence Item | Expected Screenshot / Output | Status |
|---|---|---|---|
| PIM-EV-001 | PIM Microsoft Entra roles page | Shows available roles in PIM | Pending |
| PIM-EV-002 | Eligible role assignment | Shows lab admin account as eligible for role | Pending |
| PIM-EV-003 | Role settings | Shows MFA, justification, duration, and approval settings | Pending |
| PIM-EV-004 | Activation request | Shows user requesting role activation | Pending |
| PIM-EV-005 | MFA challenge | Shows MFA required before activation | Pending |
| PIM-EV-006 | Approval request | Shows approver receiving pending request | Pending |
| PIM-EV-007 | Approved activation | Shows role successfully activated | Pending |
| PIM-EV-008 | Active role view | Shows user currently active in role | Pending |
| PIM-EV-009 | Expired/deactivated role | Shows role no longer active after duration | Pending |
| PIM-EV-010 | PIM audit history | Shows activation, approval, and deactivation events | Pending |

---

## Screenshot Naming Standard

Use a consistent naming format:

```text
screenshots/01-pim-entra-roles-page.png
screenshots/02-eligible-role-assignment.png
screenshots/03-role-settings-mfa-approval.png
screenshots/04-activation-request.png
screenshots/05-approval-request.png
screenshots/06-approved-activation.png
screenshots/07-active-role-confirmation.png
screenshots/08-role-expired.png
screenshots/09-pim-audit-history.png
screenshots/10-entra-audit-log.png
```

---

## Test Case 1: Eligible Role Assignment

### Objective

Confirm that a lab admin account is assigned a privileged role as eligible rather than permanently active.

### Test Data

| Field | Value |
|---|---|
| User | `adm.servicedesk@0gkareemu.live` |
| Role | Authentication Administrator |
| Assignment Type | Eligible |
| Duration | Permanent eligible or time-bound eligible depending on lab design |

### Steps

1. Open Microsoft Entra admin center.
2. Go to ID Governance.
3. Open Privileged Identity Management.
4. Select Microsoft Entra roles.
5. Choose `Authentication Administrator`.
6. Add assignment for the lab admin account.
7. Set assignment type to eligible.
8. Save assignment.

### Expected Result

The user appears as eligible for the role but does not have active privileges until activation.

### Evidence

Capture:

```text
screenshots/02-eligible-role-assignment.png
```

---

## Test Case 2: MFA and Justification Required During Activation

### Objective

Confirm that role activation requires MFA and a business justification.

### Test Data

| Field | Value |
|---|---|
| User | `adm.servicedesk@0gkareemu.live` |
| Role | Authentication Administrator |
| Justification | `INC10231 - Verified identity and resetting MFA methods after phone replacement.` |
| Duration | 2 hours |

### Steps

1. Sign in as the lab admin account.
2. Open PIM.
3. Select `My roles`.
4. Select the eligible role.
5. Click activate.
6. Complete MFA prompt.
7. Enter justification and ticket number.
8. Submit activation request.

### Expected Result

The role activates only after MFA and justification are completed.

### Evidence

Capture:

```text
screenshots/04-activation-request.png
screenshots/06-approved-activation.png
```

---

## Test Case 3: Approval Required for High-Risk Role

### Objective

Confirm that high-risk role activation requires approval.

### Test Data

| Field | Value |
|---|---|
| User | `adm.security@0gkareemu.live` |
| Role | Conditional Access Administrator |
| Approver | `approver.iamlead@0gkareemu.live` |
| Justification | `CHG2044 - Update report-only Conditional Access MFA policy.` |
| Duration | 1 hour |

### Steps

1. Sign in as the requesting admin.
2. Open PIM.
3. Request activation for `Conditional Access Administrator`.
4. Complete MFA.
5. Enter change reference and justification.
6. Submit request.
7. Sign in as approver.
8. Review pending approval.
9. Approve the request.
10. Confirm requester receives active role.

### Expected Result

The role does not activate until the approver approves the request.

### Evidence

Capture:

```text
screenshots/05-approval-request.png
screenshots/06-approved-activation.png
```

---

## Test Case 4: Role Expires Automatically

### Objective

Confirm that activated privileged access expires automatically after the configured duration.

### Test Data

| Field | Value |
|---|---|
| User | `adm.security@0gkareemu.live` |
| Role | Conditional Access Administrator |
| Activation Duration | 1 hour |

### Steps

1. Activate the role through PIM.
2. Confirm the role is active.
3. Wait for the activation window to end or manually deactivate.
4. Confirm the role is no longer active.

### Expected Result

The user loses active privileged access after the activation window ends.

### Evidence

Capture:

```text
screenshots/07-active-role-confirmation.png
screenshots/08-role-expired.png
```

---

## Test Case 5: Audit History Captures Activation

### Objective

Confirm that PIM audit history records privileged role activity.

### Steps

1. Open Microsoft Entra admin center.
2. Go to ID Governance.
3. Open Privileged Identity Management.
4. Select Microsoft Entra roles.
5. Open Resource audit.
6. Filter by date, role, or user.
7. Confirm activation and approval activity appears.

### Expected Result

PIM audit history shows privileged role actions such as:

- Role assignment added
- Activation requested
- Activation approved
- Role activated
- Role deactivated or expired

### Evidence

Capture:

```text
screenshots/09-pim-audit-history.png
screenshots/10-entra-audit-log.png
```

---

## Example Audit Review Table

| Date | User | Role | Action | Justification | Approver | Result |
|---|---|---|---|---|---|---|
| 2026-06-22 | `adm.servicedesk@0gkareemu.live` | Authentication Administrator | Activated | `INC10231 - MFA reset` | N/A | Successful |
| 2026-06-22 | `adm.security@0gkareemu.live` | Conditional Access Administrator | Requested activation | `CHG2044 - CA policy update` | `approver.iamlead@0gkareemu.live` | Approved |
| 2026-06-22 | `adm.security@0gkareemu.live` | Conditional Access Administrator | Deactivated | Activation expired | System | Successful |

---

## Audit Questions This Evidence Answers

| Audit Question | Evidence |
|---|---|
| Who had privileged access? | PIM role assignment and activation history |
| Was access permanent or temporary? | Eligible assignment and activation duration |
| Why was access activated? | Justification and ticket number |
| Who approved access? | PIM approval record |
| Was MFA required? | Role settings and activation evidence |
| Did access expire? | Deactivation/expiration event |
| Was a high-risk role controlled? | Approval workflow and audit log |

---

## Evidence Redaction Guidance

Before publishing screenshots to GitHub, redact:

- Real user names
- Real email addresses outside the lab domain
- Tenant IDs
- Object IDs
- IP addresses
- Device IDs
- Correlation IDs
- Internal ticket numbers from production systems
- Any production organisation name

For a portfolio project, use only lab users and lab tenant data.

---

## Final Validation Summary

| Control | Pass Criteria | Result |
|---|---|---|
| Eligible assignment configured | User is eligible, not permanently active | Pending screenshot |
| MFA required | Activation requires MFA | Pending screenshot |
| Justification required | Activation requires business reason | Pending screenshot |
| Approval required for high-risk role | Request waits for approver | Pending screenshot |
| Activation is time-bound | Role expires after configured duration | Pending screenshot |
| Audit history available | Activation appears in audit report | Pending screenshot |

---

## Portfolio Summary

This evidence demonstrates that privileged access was not simply assigned permanently. Access was governed using PIM, protected with MFA, controlled through approval where appropriate, and recorded for audit review.

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
| PIM-EV-001 | PIM Microsoft Entra roles page | Shows available roles in PIM | Success |
| PIM-EV-002 | Eligible role assignment | Shows lab admin account as eligible for role | Success |
| PIM-EV-003 | Role settings | Shows MFA, justification, duration, and approval settings | Success |
| PIM-EV-004 | Activation request | Shows user requesting role activation | Success |
| PIM-EV-005 | MFA challenge | Shows MFA required before activation | Success |
| PIM-EV-006 | Approval request | Shows approver receiving pending request | Success |
| PIM-EV-007 | Approved activation | Shows role successfully activated | Success |
| PIM-EV-008 | Active role view | Shows user currently active in role | Success |
| PIM-EV-009 | Expired/deactivated role | Shows role no longer active after duration | Success |
| PIM-EV-010 | PIM audit history | Shows activation, approval, and deactivation events | Success |

---

## Test Case 1: Eligible Role Assignment

### Objective

Confirm that a lab admin account is assigned a privileged role as eligible rather than permanently active.

### Test Data

| Field | Value |
|---|---|
| User | `admin@ogkareemu.live` |
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

<img width="997" height="302" alt="Screenshot 2026-07-05 at 07 37 56" src="https://github.com/user-attachments/assets/010a15fc-c910-4b6c-b67a-70315b7c873d" />

---

## Test Case 2: MFA and Justification Required During Activation

### Objective

Confirm that role activation requires MFA and a business justification.

### Test Data

| Field | Value |
|---|---|
| User | `admin@ogkareemu.live` |
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
**Activation Request**

**EV-1**
<img width="1017" height="564" alt="Screenshot 2026-07-05 at 07 39 55" src="https://github.com/user-attachments/assets/86fa478b-bd89-444c-b5cd-8cf1bae5867e" />

**EV-2**
<img width="1010" height="396" alt="Screenshot 2026-07-05 at 07 41 03" src="https://github.com/user-attachments/assets/5b78f719-f8e0-4857-8d10-0ca78c315489" />


**Approved Activation**

**EV-1**
<img width="899" height="497" alt="Screenshot 2026-07-05 at 07 42 14" src="https://github.com/user-attachments/assets/917f680a-e577-43e2-a446-184caa5e4b21" />

**EV-2**


<img width="305" height="600" alt="Screenshot 2026-07-05 at 07 43 06" src="https://github.com/user-attachments/assets/ba8e796d-3c98-4aa3-bff5-8336a1dfbe6e" />

**EV-3**
<img width="1034" height="498" alt="Screenshot 2026-07-05 at 07 42 48" src="https://github.com/user-attachments/assets/c1bf2d50-48c4-42e0-88f0-bd677471c683" />

**EV-4**
<img width="1024" height="322" alt="Screenshot 2026-07-05 at 07 43 58" src="https://github.com/user-attachments/assets/342d4b7c-f926-48cd-b14e-8561803074c8" />

---

## Test Case 3: Approval Required for High-Risk Role

### Objective

Confirm that the activation of high-risk roles requires approval.

### Test Data

| Field | Value |
|---|---|
| User | `admin@ogkareemu.live` |
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
9. Submit request.
10. Sign in as approver.
11. Review pending approval.
12. Approve the request.
13. Confirm requester receives active role.

### Expected Result

The role does not activate until the approver approves the request.

### Evidence
**Activation Request**
**EV-1**
<img width="1017" height="564" alt="Screenshot 2026-07-05 at 07 39 55" src="https://github.com/user-attachments/assets/86fa478b-bd89-444c-b5cd-8cf1bae5867e" />

**EV-2**
<img width="1010" height="396" alt="Screenshot 2026-07-05 at 07 41 03" src="https://github.com/user-attachments/assets/5b78f719-f8e0-4857-8d10-0ca78c315489" />


**Approved Activation**
**EV-1**
<img width="899" height="497" alt="Screenshot 2026-07-05 at 07 42 14" src="https://github.com/user-attachments/assets/917f680a-e577-43e2-a446-184caa5e4b21" />

**EV-2**


<img width="305" height="600" alt="Screenshot 2026-07-05 at 07 43 06" src="https://github.com/user-attachments/assets/ba8e796d-3c98-4aa3-bff5-8336a1dfbe6e" />

**EV-3**
<img width="1034" height="498" alt="Screenshot 2026-07-05 at 07 42 48" src="https://github.com/user-attachments/assets/c1bf2d50-48c4-42e0-88f0-bd677471c683" />

**EV-4**
<img width="1024" height="322" alt="Screenshot 2026-07-05 at 07 43 58" src="https://github.com/user-attachments/assets/342d4b7c-f926-48cd-b14e-8561803074c8" />

---

## Test Case 4: Role Expires Automatically

### Objective

Confirm that activated privileged access expires automatically after the configured duration.

### Test Data

| Field | Value |
|---|---|
| User | `admin@ogkareemu.live` |
| Role | Authentication Administrator |
| Activation Duration | 2 hour |

### Steps

1. Activate the role through PIM.
2. Confirm the role is active.
3. Wait for the activation window to end or manually deactivate.
4. Confirm the role is no longer active.

### Expected Result

The user loses active privileged access after the activation window ends.

### Evidence
**EV-1**
<img width="1024" height="322" alt="Screenshot 2026-07-05 at 07 43 58" src="https://github.com/user-attachments/assets/3786ab9f-fcc6-4d37-b666-cd9031f5e826" />

**EV-2**


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
<img width="963" height="433" alt="Screenshot 2026-07-05 at 07 45 31" src="https://github.com/user-attachments/assets/43561c8a-7113-4594-a3db-be2918eecc33" />



---

## Example Audit Review Table

| Date | User | Role | Action | Justification | Approver | Result |
|---|---|---|---|---|---|---|
| 2026-07-05 | `admin@ogkareemu.live` | Authentication Administrator | Activated | `INC10231 - MFA reset` | N/A | Successful |
| 2026-07-05 | `admin@ogkareemu.live` | Authentication Administrator | Requested activation | `INC10231 - MFA reset` | `l*****************@ogkareemu.live` | Approved |
| 2026-07-05 | `admin@ogkareemu.live` | Authentication Administrator | Deactivated | Activation expired | System | Successful |

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

## Final Validation Summary

| Control | Pass Criteria | Result |
|---|---|---|
| Eligible assignment configured | User is eligible, not permanently active | screenshot attached |
| MFA required | Activation requires MFA | screenshot attached |
| Justification required | Activation requires business reason | screenshot attached |
| Approval required for high-risk role | Request waits for approver | screenshot attached |
| Activation is time-bound | Role expires after configured duration | screenshot attached |
| Audit history available | Activation appears in audit report | screenshot attached |

---

## Portfolio Summary

This evidence demonstrates that privileged access was not simply assigned permanently. Access was governed using PIM, protected with MFA, controlled through approval where appropriate, and recorded for audit review.

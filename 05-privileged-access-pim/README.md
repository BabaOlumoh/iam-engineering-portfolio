# 05 - Privileged Access Management with Microsoft Entra PIM

## Project Summary

This project demonstrates how to reduce standing administrative privileges in Microsoft Entra ID by implementing **Microsoft Entra Privileged Identity Management (PIM)**.

The lab shows how privileged roles can be assigned as **eligible**, activated only when required, protected with **MFA**, controlled through **approval workflows**, and reviewed through **audit evidence**.

This project shows practical understanding of:

- Privileged access management
- Just-in-time access
- Least privilege access design
- Microsoft Entra role governance
- Approval-based elevation
- Audit and compliance evidence
- Separation between daily user accounts, admin accounts, and emergency access accounts

---

## Business Problem

In many organisations, administrators are given permanent privileged access because it is convenient. This creates risk because:

- Compromised admin accounts can cause tenant-wide damage.
- Users may retain privileged access longer than needed.
- There may be weak evidence of who elevated, when, why, and who approved it.
- Service Desk or IAM staff may have broader permissions than their job requires.
- Auditors may struggle to prove that privileged access is controlled.

This project solves that by using **Microsoft Entra PIM** to make admin access temporary, justified, approved, and auditable.

---

## Lab Scenario

The environment represents a small hybrid IAM lab where users are managed through Active Directory and Microsoft Entra ID.

Privileged access is required for tasks such as:

- Resetting authentication methods
- Managing users and groups
- Reviewing sign-in and audit logs
- Managing Conditional Access policies
- Managing privileged role assignments

Instead of assigning permanent admin roles to users, this lab uses PIM to enforce:

- Eligible role assignments
- Just-in-time activation
- MFA on activation
- Business justification
- Approval for high-impact roles
- Time-bound access
- Audit review after activation

---

## Project Objectives

By the end of this project, the lab should demonstrate that:

1. Admin access is not permanently assigned by default.
2. Users activate privileged roles only when needed.
3. High-risk roles require approval before activation.
4. Activation requires MFA and justification.
5. Access expires automatically after a defined period.
6. Emergency access accounts are separated from normal PIM workflows.
7. PIM activity can be reviewed using audit evidence.

---

## Folder Structure

```text
05-privileged-access-pim/
├── README.md
├── role-design.md
├── approval-workflow.md
├── audit-evidence.md
```

---

## Technologies Used

| Area | Technology |
|---|---|
| Identity platform | Microsoft Entra ID |
| Privileged access | Microsoft Entra Privileged Identity Management |
| Authentication control | MFA / Conditional Access |
| Governance | Eligible assignments, approvals, justification, audit history |
| Evidence | Entra admin center screenshots and audit logs |

---

## Roles Included in the Design

| Role | Purpose | Assignment Model |
|---|---|---|
| Global Administrator | Emergency-only tenant recovery | Break-glass only |
| Privileged Role Administrator | Manage role assignments and PIM settings | Eligible with approval |
| Security Administrator | Manage security configuration and policies | Eligible with approval |
| Conditional Access Administrator | Manage Conditional Access policies | Eligible with approval |
| User Administrator | Manage user lifecycle operations | Eligible |
| Authentication Administrator | Reset authentication methods | Eligible |
| Groups Administrator | Manage Microsoft 365 and security groups | Eligible |
| Global Reader | Read-only tenant visibility | Eligible or active depending on need |
| Security Reader | Review security logs and reports | Eligible or active depending on need |

---

## High-Level Design

```text
User / Admin Account
        |
        v
Eligible PIM Role Assignment
        |
        v
Activation Request
        |
        +--> MFA required
        +--> Business justification required
        +--> Ticket number required for high-risk roles
        +--> Approval required for sensitive roles
        |
        v
Temporary Role Activation
        |
        v
Admin Task Completed
        |
        v
Role Expires Automatically
        |
        v
Audit Evidence Reviewed
```

---

## Security Principles Applied

### 1. Least Privilege

Users are assigned only the role needed to perform their job. For example, a Service Desk Analyst should not receive Global Administrator when Authentication Administrator is enough for MFA reset tasks.

### 2. Just-in-Time Access

Privileged access is activated only when required and expires automatically.

### 3. Separation of Duties

The person requesting elevated access should not approve their own activation for high-risk roles.

### 4. Strong Authentication

MFA is required before role activation.

### 5. Auditability

Every activation should have a reason, requester, time, duration, role, and approval record.

### 6. Emergency Access Resilience

Break-glass accounts are kept separate from normal user and admin accounts to prevent tenant lockout during incidents.

---

## Example Admin Personas

| Persona | Example Role Need | PIM Design |
|---|---|---|
| Service Desk Analyst | Reset MFA methods, support user account issues | Authentication Administrator, User Administrator |
| IAM Analyst | Manage users, groups, access reviews, app assignments | User Administrator, Groups Administrator, Security Reader |
| Security Analyst | Review sign-ins, risky users, audit logs | Security Reader, Security Administrator |
| IAM Lead | Approve privileged access and manage role assignments | Privileged Role Administrator |
| Emergency Admin | Recover tenant during outage or lockout | Separate break-glass account |

---

## What This Project Proves to Employers

This project shows that I understand privileged access beyond simply assigning admin roles. It demonstrates that I can design an IAM control that balances security, operations, and audit requirements.

It proves practical understanding of:

- Why permanent admin access is risky
- How to design role-based privilege boundaries
- How PIM supports Zero Trust
- How approval workflows reduce privilege misuse
- How audit evidence supports compliance
- How Service Desk and IAM teams can operate safely without excessive access

---

## Screenshots

1. PIM landing page
2. Microsoft Entra roles page
3. Eligible role assignment
4. Role settings showing MFA/justification/approval
5. Activation request screen
6. Approval request screen
7. Successful role activation
8. Role expiration or deactivation
9. PIM audit history
10. Entra audit log showing role activation

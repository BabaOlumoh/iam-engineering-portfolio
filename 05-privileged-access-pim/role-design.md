# PIM Role Design

## Purpose

This document defines the privileged role design for the Microsoft Entra PIM lab.

The goal is to remove unnecessary standing admin access and replace it with:

- Eligible role assignments
- Time-bound activation
- MFA enforcement
- Justification
- Approval for high-risk roles
- Audit review

---

## Design Principles

### 1. No Daily Account Should Have Permanent Admin Access

Normal user accounts should not be used for privileged administration. Admin activities should be performed using separate admin accounts where possible.

Example:

| Account Type | Example | Purpose |
|---|---|---|
| Standard account | `ade.user@0gkareemu.live` | Email, Teams, normal work |
| Admin account | `adm.ade@0gkareemu.live` | Privileged admin tasks |
| Break-glass account | `bg.tenant.recovery@0gkareemu.live` | Emergency access only |

---

### 2. Use Eligible Assignments by Default

A role should be assigned as **eligible** unless there is a strong operational reason for permanent active assignment.

Eligible assignment means the user has permission to request the role, but they do not hold the role until they activate it.

---

### 3. Match the Role to the Task

Avoid assigning broad roles when a narrower role is enough.

Example:

| Task | Avoid | Better Role |
|---|---|---|
| Reset a user's MFA methods | Global Administrator | Authentication Administrator |
| Update user profile details | Global Administrator | User Administrator |
| Manage groups | Global Administrator | Groups Administrator |
| Review security logs | Security Administrator | Security Reader |
| Manage Conditional Access | Global Administrator | Conditional Access Administrator |

---

### 4. Require Strong Controls for Sensitive Roles

Sensitive roles should require approval before activation.

Sensitive roles include:

- Global Administrator
- Privileged Role Administrator
- Security Administrator
- Conditional Access Administrator
- Exchange Administrator
- SharePoint Administrator
- Intune Administrator

---

## Role Assignment Matrix

| Role | Target Persona | Assignment Type | Approval Required | MFA Required | Justification Required | Max Activation Duration | Notes |
|---|---|---:|---:|---:|---:|---:|---|
| Global Administrator | Emergency admin only | Active break-glass only | N/A | Strong auth where supported | N/A | N/A | Not assigned through normal daily use |
| Privileged Role Administrator | IAM Lead | Eligible | Yes | Yes | Yes | 1 hour | Can manage role assignments and PIM settings |
| Security Administrator | Security Analyst / IAM Lead | Eligible | Yes | Yes | Yes | 2 hours | High-impact security role |
| Conditional Access Administrator | IAM Lead / Security Admin | Eligible | Yes | Yes | Yes | 1 hour | Can change access policies; high lockout risk |
| User Administrator | IAM Analyst / Service Desk Lead | Eligible | No or Yes | Yes | Yes | 2 hours | Used for user lifecycle admin tasks |
| Authentication Administrator | Service Desk / IAM Analyst | Eligible | No or Yes | Yes | Yes | 2 hours | Used for MFA method reset and auth support |
| Groups Administrator | IAM Analyst | Eligible | No | Yes | Yes | 2 hours | Used for Microsoft 365/security group management |
| Global Reader | Auditor / IAM Analyst | Eligible or Active | No | Yes | Optional | 4 hours | Read-only visibility |
| Security Reader | Security Analyst / IAM Analyst | Eligible or Active | No | Yes | Optional | 4 hours | Read security reports and logs |

---

## Role Design by Operational Scenario

### Scenario 1: Service Desk MFA Reset

**Requirement:** Service Desk needs to reset a user's authentication methods.

**Role:** Authentication Administrator

**Design:**

- Assignment type: Eligible
- MFA required: Yes
- Justification required: Yes
- Approval: Optional depending on organisation risk appetite
- Activation duration: 2 hours
- Example justification: `INC10245 - User changed phone and cannot complete MFA registration.`

---

### Scenario 2: Joiner/Mover/Leaver Support

**Requirement:** IAM Analyst needs to update user attributes, group membership, and account settings.

**Roles:**

- User Administrator
- Groups Administrator

**Design:**

- Assignment type: Eligible
- MFA required: Yes
- Justification required: Yes
- Approval: Optional for User Administrator; optional or required for Groups Administrator depending on group sensitivity
- Activation duration: 2 hours

---

### Scenario 3: Conditional Access Policy Change

**Requirement:** Security or IAM lead needs to modify a Conditional Access policy.

**Role:** Conditional Access Administrator

**Design:**

- Assignment type: Eligible
- MFA required: Yes
- Justification required: Yes
- Approval: Required
- Ticket number: Required
- Activation duration: 1 hour
- Approver: IAM Lead or Security Manager

**Reason:** Incorrect Conditional Access changes can lock users or administrators out of the tenant.

---

### Scenario 4: PIM Configuration Change

**Requirement:** IAM Lead needs to update PIM settings or assign privileged roles.

**Role:** Privileged Role Administrator

**Design:**

- Assignment type: Eligible
- MFA required: Yes
- Justification required: Yes
- Approval: Required
- Activation duration: 1 hour
- Approver: Secondary IAM Lead or Security Manager

---

## Break-Glass Role Design

Break-glass accounts should not be treated like normal admin accounts.

Recommended controls:

- Minimum two emergency access accounts
- Cloud-only accounts
- Long, complex passwords stored securely
- Excluded from Conditional Access policies that could cause lockout
- Not used for daily administration
- Monitored with alerts
- Tested periodically
- Reviewed monthly

Break-glass accounts exist to recover the tenant when normal authentication or Conditional Access controls fail.

---

## PIM Role Settings Standard

| Setting | Standard Configuration |
|---|---|
| Activation maximum duration | 1-4 hours depending on role risk |
| Require MFA on activation | Enabled |
| Require justification | Enabled |
| Require ticket information | Enabled for high-risk roles |
| Require approval | Enabled for high-risk roles |
| Notifications | Enabled for administrators and approvers |
| Permanent active assignment | Avoid except emergency access |
| Assignment review | Review monthly or quarterly |

---

## Risk-Based Role Classification

| Risk Level | Example Roles | Controls |
|---|---|---|
| Critical | Global Administrator, Privileged Role Administrator | Break-glass or eligible only, approval required, short duration, monitored |
| High | Security Administrator, Conditional Access Administrator, Exchange Administrator | Eligible only, approval required, MFA, justification, ticket required |
| Medium | User Administrator, Groups Administrator, Authentication Administrator | Eligible, MFA, justification, approval optional |
| Low | Global Reader, Security Reader | Eligible or active depending on operational need |

---

## Design Decision Summary

| Decision | Reason |
|---|---|
| Use eligible assignments for admin roles | Reduces standing privilege |
| Require MFA on activation | Protects privilege elevation |
| Require approval for high-risk roles | Adds separation of duties |
| Require justification | Creates audit trail |
| Use short activation windows | Limits exposure if account is compromised |
| Keep break-glass accounts separate | Prevents tenant lockout |
| Capture audit evidence | Supports compliance and operational review |

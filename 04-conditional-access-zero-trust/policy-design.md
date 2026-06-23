# Conditional Access Policy Design

## Purpose

This document defines the Conditional Access policy design for enforcing MFA across all users in Microsoft Entra ID using a controlled Zero Trust approach.

The design assumes a real-world organisation where users access Microsoft 365 cloud apps, administrators manage privileged resources, and emergency access accounts are required to prevent tenant lockout.

## Design Principles

| Principle | How It Is Applied |
|---|---|
| Verify explicitly | Require MFA before granting access to cloud resources |
| Use least privilege | Apply stronger controls to privileged users and admin portals |
| Assume breach | Block legacy authentication and monitor failed MFA attempts |
| Avoid lockout | Exclude emergency access accounts from normal CA restrictions |
| Roll out safely | Use report-only mode before enabling enforcement |
| Document evidence | Capture policy settings, sign-in logs, and test results |

## Policy Naming Convention

Policies use the following naming format:

```text
CA-[Number] - [Control] - [Scope]
```

Examples:

```text
CA-001 - Require MFA - All Users
CA-002 - Require Strong MFA - Admin Roles
CA-003 - Block Legacy Authentication - All Users
CA-004 - Require MFA - Security Info Registration
```

## CA-001 - Require MFA - All Users

### Objective

Require MFA for all standard workforce users accessing Microsoft cloud resources.

### Policy Configuration

| Area | Configuration |
|---|---|
| Users | Include: All users |
| Exclude | Break-glass accounts, approved service accounts, directory sync accounts where applicable |
| Target resources | All cloud apps or Office 365 during pilot |
| Conditions | All locations initially; named locations can be added later |
| Client apps | Browser, mobile apps, desktop clients |
| Grant control | Require multifactor authentication |
| Session controls | None during initial rollout; review after baseline success |
| State | Report-only first, then On |

### Recommended Exclusions

| Account Type | Reason |
|---|---|
| Break-glass account 1 | Prevent full tenant lockout |
| Break-glass account 2 | Backup emergency access path |
| Azure AD Connect sync account | Avoid breaking hybrid identity synchronisation |
| Approved non-interactive service accounts | Prevent automation failure; should be replaced with managed identities where possible |

### Implementation Steps

1. Sign in to the Microsoft Entra admin center.
2. Browse to **Protection > Conditional Access > Policies**.
3. Create a new policy named `CA-001 - Require MFA - All Users`.
4. Under **Users**, include **All users**.
5. Exclude emergency access accounts and approved service accounts.
6. Under **Target resources**, select **All cloud apps** or **Office 365** for pilot.
7. Under **Grant**, select **Require multifactor authentication**.
8. Set policy to **Report-only**.
9. Review sign-in logs and Conditional Access report-only results.
10. Move to **On** after testing confirms expected behaviour.

### Expected Behaviour

| Scenario | Expected Result |
|---|---|
| Standard user signs in to Microsoft 365 | MFA is required |
| Standard user completes MFA | Access is granted |
| Standard user fails MFA | Access is blocked or interrupted |
| Break-glass account signs in | CA-001 does not apply due to exclusion |
| Service account attempts non-interactive sign-in | Should not be blocked unexpectedly during rollout |

## CA-002 - Require Strong MFA - Admin Roles

### Objective

Require stronger authentication for privileged users because administrator accounts have a tenant-wide impact.

### Policy Configuration

| Area | Configuration |
|---|---|
| Users | Directory roles: Global Administrator, Privileged Role Administrator, Security Administrator, Conditional Access Administrator, User Administrator |
| Exclude | Break-glass accounts managed through separate emergency access process |
| Target resources | Microsoft Admin Portals and Azure management apps |
| Grant control | Require phishing-resistant MFA or stronger authentication strength where available |
| State | Report-only, then On |

### Rationale

Admin accounts should not rely only on basic MFA methods where stronger methods are available. Stronger options include FIDO2/passkeys, certificate-based authentication, or other approved phishing-resistant methods.

### Expected Behaviour

| Scenario | Expected Result |
|---|---|
| Global Administrator opens admin portal | Strong MFA required |
| Admin uses weak or unapproved method | Access is interrupted or denied |
| Admin uses approved strong method | Access is granted |

## CA-003 - Block Legacy Authentication - All Users

### Objective

Block legacy authentication protocols that do not support modern authentication and can bypass MFA.

### Policy Configuration

| Area | Configuration |
|---|---|
| Users | Include: All users |
| Exclude | Emergency access accounts during testing only if required |
| Target resources | All cloud apps |
| Conditions | Client apps: legacy authentication clients |
| Grant control | Block access |
| State | Report-only, then On |

### Examples of Legacy Authentication Risk

- Older Office clients.
- Basic authentication protocols.
- Scripts or apps using username and password only.
- Mail protocols that do not support modern authentication.

### Expected Behaviour

| Scenario | Expected Result |
|---|---|
| Modern Outlook client signs in | Policy does not block solely due to client type |
| Legacy auth attempt occurs | Access is blocked |
| Sign-in logs reviewed | Legacy auth attempts are visible for remediation |

## CA-004 - Require MFA - Security Info Registration

### Objective

Protect the process where users register or modify authentication methods.

### Policy Configuration

| Area | Configuration |
|---|---|
| Users | Include: All users |
| Exclude | Break-glass accounts if required by emergency access process |
| Target resources | User action: Register security information |
| Grant control | Require multifactor authentication |
| State | Report-only, then On |

### Expected Behaviour

| Scenario | Expected Result |
|---|---|
| User registers MFA methods | MFA is required where applicable |
| Attacker with only password attempts registration | Access is interrupted |

## Policy Deployment Plan

### Phase 1 - Preparation

- Confirm all users have at least one valid MFA method.
- Confirm break-glass accounts exist.
- Confirm emergency access credentials are securely stored.
- Confirm service accounts and sync accounts are known.
- Confirm helpdesk has MFA reset process.

### Phase 2 - Report-Only Testing

- Enable policies in report-only mode.
- Review sign-in logs for at least several business days.
- Identify impacted users, apps, service accounts, and locations.
- Resolve false positives before enforcement.

### Phase 3 - Pilot Enforcement

- Enable CA-001 for a pilot group.
- Monitor failed sign-ins and service desk tickets.
- Confirm users can complete MFA successfully.
- Confirm break-glass accounts are not locked out.

### Phase 4 - Full Enforcement

- Enable CA-001 for all users.
- Enable CA-002 for admins.
- Enable CA-003 after legacy auth dependencies are resolved.
- Continue monitoring logs and support tickets.

## Risk Register

| Risk | Impact | Mitigation |
|---|---|---|
| Admin lockout | Critical | Maintain two emergency access accounts excluded from normal CA policies |
| Users not registered for MFA | High | Run registration campaign and provide helpdesk guidance |
| Service account failure | High | Identify and exclude approved service accounts temporarily |
| Legacy app outage | Medium/High | Use report-only logs to identify legacy authentication before blocking |
| MFA fatigue attacks | Medium | Prefer number matching, stronger methods, and user awareness |
| Poor documentation | Medium | Record policy settings, exclusions, test results, and approval notes |

## Operational Checklist

- [ ] Break-glass accounts created.
- [ ] Break-glass accounts tested.
- [ ] Break-glass accounts excluded from normal CA policies.
- [ ] MFA methods enabled for users.
- [ ] Test users selected.
- [ ] CA-001 created in report-only mode.
- [ ] Sign-in logs reviewed.
- [ ] Service accounts reviewed.
- [ ] Pilot enforcement completed.
- [ ] Full enforcement approved.
- [ ] Evidence captured in screenshots folder.

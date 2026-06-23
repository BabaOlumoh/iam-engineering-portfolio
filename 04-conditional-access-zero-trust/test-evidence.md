# Test Evidence

## Purpose

This document records the evidence required to prove that the Conditional Access MFA enforcement project works as designed.

The evidence should show that MFA is enforced for standard users, stronger controls apply to administrators, legacy authentication is blocked, and break-glass accounts are protected from tenant lockout scenarios.

## Test Users

| Test User | Role | Purpose |
|---|---|---|
| standard.user@tenant.com | Standard user | Confirm MFA is required for normal access |
| admin.user@tenant.com | Privileged user | Confirm stronger admin policy applies |
| bg-admin-01@tenant.onmicrosoft.com | Break-glass account | Confirm exclusion and emergency access path |
| service.sync@tenant.com | Service/sync account example | Confirm service accounts are reviewed before enforcement |

## Test Scope

| Policy | Test Status | Evidence Required |
|---|---|---|
| CA-001 - Require MFA - All Users | Pending / Passed / Failed | Policy config and sign-in logs |
| CA-002 - Require Strong MFA - Admin Roles | Pending / Passed / Failed | Admin sign-in and policy result |
| CA-003 - Block Legacy Authentication | Pending / Passed / Failed | Legacy auth sign-in blocked or report-only result |
| CA-004 - Require MFA - Security Info Registration | Pending / Passed / Failed | Security info registration prompt evidence |

## Evidence Naming Standard

Use clear screenshot names so the evidence is easy to review.

```text
screenshots/ca-001-policy-overview.png
screenshots/ca-001-users-included-exclusions.png
screenshots/ca-001-grant-controls-mfa.png
screenshots/ca-001-report-only-result-standard-user.png
screenshots/ca-001-enabled-on.png
screenshots/ca-002-admin-strong-mfa-result.png
screenshots/ca-003-legacy-auth-blocked.png
screenshots/break-glass-exclusion.png
screenshots/break-glass-signin-alert.png
```

## Test Case 1 - Standard User Requires MFA

### Objective

Confirm that a standard user must complete MFA when accessing Microsoft 365 resources.

### Preconditions

- `CA-001 - Require MFA - All Users` exists.
- Policy is in report-only mode or On.
- Test user is included in policy.
- Test user is not excluded.
- Test user has an MFA method registered.

### Steps

1. Sign in as `standard.user@tenant.com`.
2. Access Microsoft 365 or another target cloud app.
3. Observe MFA prompt or report-only result.
4. Complete MFA.
5. Open Entra sign-in logs.
6. Confirm Conditional Access policy result.

### Expected Result

The user is required to complete MFA before access is granted.

### Evidence

| Evidence | Screenshot |
|---|---|
| Policy assignment includes user | `screenshots/ca-001-users-included-exclusions.png` |
| Grant control requires MFA | `screenshots/ca-001-grant-controls-mfa.png` |
| Sign-in log shows policy applied | `screenshots/ca-001-report-only-result-standard-user.png` |

### Actual Result

```text
Result: Pending
Notes:
```

## Test Case 2 - User Fails or Cancels MFA

### Objective

Confirm that access is not granted when MFA is not completed.

### Steps

1. Sign in as `standard.user@tenant.com`.
2. Trigger access to Microsoft 365.
3. Cancel or fail the MFA challenge.
4. Review sign-in logs.

### Expected Result

Access is interrupted or blocked because MFA was not satisfied.

### Evidence

```text
screenshots/ca-001-mfa-failed-signin.png
```

### Actual Result

```text
Result: Pending
Notes:
```

## Test Case 3 - Break-Glass Account Is Excluded

### Objective

Confirm that emergency access accounts are excluded from normal MFA enforcement policy to prevent tenant lockout.

### Preconditions

- Break-glass account exists.
- Account is cloud-only.
- Account is excluded from CA-001.
- Alerting is configured for break-glass sign-in.

### Steps

1. Open `CA-001 - Require MFA - All Users`.
2. Confirm break-glass account is excluded.
3. Sign in with the break-glass account during an approved test window.
4. Confirm sign-in succeeds.
5. Confirm alert is generated.

### Expected Result

The break-glass account is not blocked by CA-001, and its sign-in activity is logged and alerted.

### Evidence

| Evidence | Screenshot |
|---|---|
| Break-glass account excluded | `screenshots/break-glass-exclusion.png` |
| Sign-in successful | `screenshots/break-glass-signin-test.png` |
| Alert generated | `screenshots/break-glass-signin-alert.png` |

### Actual Result

```text
Result: Pending
Notes:
```

## Test Case 4 - Admin Requires Stronger MFA

### Objective

Confirm that privileged users must satisfy a stronger authentication requirement than standard users.

### Preconditions

- Admin test user is assigned an Entra admin role.
- `CA-002 - Require Strong MFA - Admin Roles` exists.
- Admin has an approved strong authentication method.

### Steps

1. Sign in as `admin.user@tenant.com`.
2. Access Microsoft Entra admin center or Microsoft Admin Portal.
3. Observe authentication requirement.
4. Complete approved strong MFA method.
5. Review sign-in logs.

### Expected Result

The admin user must complete strong MFA before accessing admin resources.

### Evidence

```text
screenshots/ca-002-admin-strong-mfa-result.png
```

### Actual Result

```text
Result: Pending
Notes:
```

## Test Case 5 - Legacy Authentication Blocked

### Objective

Confirm that legacy authentication is blocked or flagged before enforcement.

### Preconditions

- `CA-003 - Block Legacy Authentication - All Users` exists.
- Policy is in report-only mode or On.
- Legacy client app condition is configured.

### Steps

1. Review sign-in logs for legacy authentication attempts.
2. Confirm CA-003 result in report-only mode.
3. After dependencies are remediated, enable policy.
4. Confirm legacy authentication attempts are blocked.

### Expected Result

Legacy authentication attempts are blocked after enforcement.

### Evidence

```text
screenshots/ca-003-legacy-auth-report-only.png
screenshots/ca-003-legacy-auth-blocked.png
```

### Actual Result

```text
Result: Pending
Notes:
```

## Test Case 6 - Security Info Registration Requires MFA

### Objective

Confirm that users must complete MFA when registering or changing security information.

### Preconditions

- `CA-004 - Require MFA - Security Info Registration` exists.
- User is included in policy.

### Steps

1. Sign in as test user.
2. Browse to security information registration page.
3. Attempt to add or update MFA method.
4. Confirm MFA requirement.
5. Review sign-in logs.

### Expected Result

The user must complete MFA before registering or changing security information.

### Evidence

```text
screenshots/ca-004-security-info-registration-mfa.png
```

### Actual Result

```text
Result: Pending
Notes:
```

## Sign-In Log Review Checklist

For each test, review the following fields in Microsoft Entra sign-in logs:

- User principal name.
- Application accessed.
- Authentication requirement.
- Conditional Access policy result.
- MFA requirement satisfied or not satisfied.
- Client app used.
- Location.
- Device details where applicable.
- Failure reason where applicable.

## Final Validation Summary

| Control | Pass Criteria | Result |
|---|---|---|
| MFA enforced for all standard users | Standard user is prompted for MFA | Pending |
| Admins require stronger authentication | Admin user must satisfy stronger control | Pending |
| Break-glass account excluded | Emergency account can sign in during approved test | Pending |
| Break-glass usage monitored | Alert generated on sign-in | Pending |
| Legacy auth blocked | Legacy auth attempt denied after enforcement | Pending |
| Evidence documented | Screenshots saved with clear names | Pending |

## Lessons Learned

Use this section after completing the lab.

```text
What worked well:

What failed or needed adjustment:

What I would improve in a production rollout:

How this improves the organisation's identity security posture:
```

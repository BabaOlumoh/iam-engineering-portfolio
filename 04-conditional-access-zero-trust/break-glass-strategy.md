# Break-Glass Strategy

## Purpose

Break-glass accounts, also known as emergency access accounts, are used only when normal administrator access is unavailable. They protect the organisation from being locked out of Microsoft Entra ID during Conditional Access misconfiguration, MFA outage, federation outage, or privileged account problems.

This document defines how emergency access accounts should be designed, protected, excluded, monitored, and tested.

## Why Break-Glass Accounts Are Required

Conditional Access policies can affect every user in the tenant. A misconfigured policy could block administrators from signing in, especially if all users are included without exclusions.

Break-glass accounts provide a controlled emergency path to recover access.

## Account Design

| Design Area | Recommendation |
|---|---|
| Number of accounts | Minimum of two emergency access accounts |
| Account type | Cloud-only accounts, not synced from on-prem AD |
| Role assignment | Global Administrator only where justified |
| Password | Long, unique, securely stored, not reused |
| MFA method | Phishing-resistant method such as FIDO2/passkey or certificate-based authentication where required |
| Conditional Access | Excluded from normal blocking/restrictive CA policies |
| Usage | Only during emergency or scheduled test |
| Monitoring | Alert on every sign-in attempt |

## Example Accounts

| Account | Purpose |
|---|---|
| bg-admin-01@tenant.onmicrosoft.com | Primary emergency access account |
| bg-admin-02@tenant.onmicrosoft.com | Secondary emergency access account |


## Key Design Decisions

### 1. Use cloud-only accounts

Emergency access accounts should not depend on on-premises Active Directory, AD FS, Entra Connect, or third-party identity providers. If the on-premises environment is unavailable, the emergency account must still work.

### 2. Exclude from normal Conditional Access policies

Break-glass accounts should be excluded from normal Conditional Access policies that could block or restrict sign-in. This avoids a full tenant lockout if a Conditional Access policy is misconfigured.

### 3. Still protect emergency access accounts

Excluding break-glass accounts from normal Conditional Access does not mean they should be weak. They must be protected using strong credentials, secure storage, phishing-resistant MFA where required, and high-priority monitoring.

### 4. Monitor every sign-in

Every break-glass sign-in should trigger an alert. A break-glass sign-in is either a planned test or a security incident.

## Conditional Access Exclusion Standard

Every Conditional Access policy should be reviewed to confirm whether emergency access accounts are excluded.

| Policy Type | Break-Glass Exclusion Required? | Reason |
|---|---|---|
| Require MFA for all users | Yes | Avoid lockout due to MFA or CA misconfiguration |
| Block legacy authentication | Usually yes during initial rollout; review carefully | Avoid blocking emergency recovery path |
| Require compliant device | Yes | Emergency access should not depend on device compliance service |
| Require trusted location | Yes | Emergency may occur outside trusted network |
| Block selected countries | Yes or separate controlled design | Avoid blocking emergency access during travel/outage |
| Admin strong MFA policy | Usually excluded from normal policy, protected separately | Avoid dependency on same control plane |

## Monitoring Requirements

Break-glass accounts should have alerting configured for:

- Successful sign-in.
- Failed sign-in.
- Password change.
- MFA method change.
- Role assignment change.
- Conditional Access exclusion change.
- Account disabled or deleted.

## Example Alert Logic

```text
IF userPrincipalName equals breakglass@ogkareemu.live
OR userPrincipalName equals breakglass2@ogkareemu.live
THEN send high-priority alert to IAM/security administrators
```

## Testing Schedule

| Test | Frequency | Owner | Evidence Required |
|---|---|---|---|
| Confirm credentials available | Quarterly | IAM/Security lead | Test record only; do not expose password |
| Confirm sign-in works | Quarterly | IAM/Security lead | Sign-in log screenshot |
| Confirm alert fires | Quarterly | Security operations | Alert screenshot |
| Confirm CA exclusion remains | Monthly or after CA change | IAM admin | Policy exclusion screenshot |
| Confirm account role assignment | Monthly | IAM admin | Role assignment screenshot |

## Emergency Access Procedure

1. Confirm normal administrator access is unavailable.
2. Obtain approval from the authorised incident lead where possible.
3. Retrieve emergency access credential using approved secure process.
4. Sign in using the emergency access account.
5. Resolve the issue causing lockout or outage.
6. Document actions taken.
7. Rotate credentials if exposure is suspected.
8. Review sign-in logs and audit logs.
9. Produce post-incident review.

## Example Use Cases

| Scenario | Use Break-Glass? | Reason |
|---|---|---|
| Global Admin forgot password | No | Use normal recovery process |
| Conditional Access blocks all admins | Yes | Emergency access required |
| MFA service disruption blocks admin access | Yes | Emergency recovery may be required |
| User cannot register MFA | No | Service Desk process should handle it |
| Entra Connect server offline | Usually no | Cloud admin accounts should still work |
| Admin account compromised | Possibly | Use only if no other secure admin path exists |

## Controls to Prevent Misuse

- Restrict knowledge of emergency credential location.
- Require two-person approval to access stored credentials where possible.
- Alert on every sign-in.
- Review sign-in logs after every test or incident.
- Do not use break-glass accounts for daily administration.
- Keep accounts separate from normal privileged identity workflows.
- Test regularly but minimally.

## Evidence to be Captured

- Break-glass account list with sensitive details redacted.
- Conditional Access exclusion showing break-glass accounts excluded.
- Successful emergency account test sign-in.
- Alert triggered by emergency account sign-in.
- Role assignment evidence.
- Review checklist evidence.

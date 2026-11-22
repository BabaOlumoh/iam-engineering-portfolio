# Approval and Expiry Policy

## Purpose

This document defines the approval, expiry, extension, and removal rules for the entitlement management access package.

The goal is to ensure access is granted only when there is a valid business reason and removed automatically when it is no longer required.

## Policy Summary

| Policy Area | Configuration |
|---|---|
| Package name | AP-FINANCE-REPORTING-TEMP |
| Requestor scope | Internal users |
| Approval required | Yes |
| Approver | Finance Manager |
| Requestor justification | Required |
| Access duration | 30 days |
| Extension allowed | Yes |
| Extension approval | Required |
| Access review | Quarterly |
| Emergency assignment | IAM Team only |

## Request Policy

Users must request access through the My Access portal. Direct manual assignment should be avoided unless there is an emergency or approved exception.

### Requestor Requirements

Each request must include:

- User identity
- Access package selected
- Business justification
- Expected duration
- Manager or business context if required

### Example Request Form Questions

| Question | Purpose |
|---|---|
| Why do you need access? | Confirms business justification |
| Which project or task requires this access? | Links access to business activity |
| How long do you need access? | Supports expiry decision |
| Has your manager approved this request? | Adds accountability |

## Approval Workflow

```text
User submits request
        |
        v
Finance Manager receives approval request
        |
        v
Approver reviews justification
        |
        +--> Approve -> Access is assigned
        |
        +--> Deny -> Request is closed with reason
```

## Approval Criteria

The approver should approve only when:

- The user has a valid business reason
- The requested access matches the user's role or task
- The requested duration is reasonable
- The access does not conflict with separation-of-duties rules
- The user is still active in the organisation

The approver should deny when:

- The justification is vague
- The user does not need the access for their role
- The request is for convenience rather than business need
- The access should be requested through a different package
- The user is a guest or contractor, and this package is internal-only

## Approval SLA

| Request Type | Target Response Time |
|---|---|
| Standard access request | 2 business days |
| Urgent business access | Same business day where possible |
| Sensitive access | Requires full review before approval |

## Expiry Design

Access should expire automatically after a defined period.

| Access Type | Expiry |
|---|---|
| Standard temporary access | 30 days |
| Short-term project access | 7-14 days |
| Contractor access | End date or maximum 30 days |
| Sensitive access | 7 days or less |
| Permanent access | Avoid unless role-based and reviewed |

For this lab, the access package assignment expires after 30 days.

## Extension Policy

Users may request an extension before access expires.

Extension rules:

- Extension must require approval.
- Extension must include updated business justification.
- Extension should not exceed the original maximum duration unless justified.
- Repeated extensions should trigger IAM review.

Example extension justification:

```text
The audit reporting activity has been extended by two weeks. I require continued access to complete evidence preparation and reconciliation tasks.
```

## Access Removal

Access can be removed in the following ways:

| Removal Method | Trigger |
|---|---|
| Automatic expiry | Assignment reaches expiry date |
| Manual removal | IAM or package manager removes assignment |
| Access review removal | Reviewer marks access as no longer required |
| Leaver process | User leaves organisation |
| Mover process | User changes department or role |

## Mover Control

If a user moves out of the Finance-related role or project, access should be reviewed and removed even if the access package has not expired.

Example:

```text
User moves from Finance Analyst to HR Analyst.
Finance Reporting access is no longer required.
IAM removes access package assignment or waits for access review outcome where appropriate.
```

## Leaver Control

If a user leaves the organisation, the leaver process takes priority over entitlement expiry.

Leaver actions:

1. Disable account in source system.
2. Block sign-in in Entra ID.
3. Revoke sessions.
4. Remove access package assignments.
5. Remove group memberships.
6. Retain audit evidence.

## Emergency Access Assignment

In exceptional cases, IAM may directly assign access to a user.

This should require:

- Ticket reference
- Manager approval
- Business owner approval
- Short expiry
- Post-assignment review
- Evidence capture

Emergency access should not become the standard process.

## Separation of Duties

The same person should not ideally:

- Request access
- Approve their own access
- Own the resource
- Configure the approval policy
- Audit the assignment

Recommended design:

| Role | Person/Team |
|---|---|
| Requestor | End user |
| Approver | Business owner |
| Policy administrator | IAM team |
| Reviewer | Business owner or Security team |
| Auditor | Security or compliance team |

## Risk Controls

| Risk | Control |
|---|---|
| Users keep access too long | Expiry and reviews |
| Wrong users request access | Requestor scope |
| Poor justification | Required request questions |
| Unapproved access | Approval workflow |
| Manual access bypass | Service Desk runbook |
| Access remains after department move | Mover review |
| Guest access risk | Separate guest package |

## Service Desk Guidance

Service Desk should not manually add users to the underlying group unless an approved exception exists.

Standard response:

```text
This access is controlled through Microsoft Entra Entitlement Management. Please request the access package through the My Access portal and provide a business justification. The request will be routed to the correct business approver.
```

## IAM Administration Checklist

Before publishing the access package, the IAM team should confirm:

- Catalogue exists
- Resource owner is assigned
- Correct group/resource is added
- Requestor scope is correct
- Approval is enabled
- Approver is correct
- Justification is required
- Expiry is configured
- Extension behaviour is configured
- Test request has been completed
- Evidence has been captured

## Review Cadence

| Review Type | Frequency |
|---|---|
| Access package assignment review | Quarterly |
| Approver review | Quarterly |
| Resource owner review | Every 6 months |
| Policy configuration review | Every 6 months |
| Emergency assignments review | Monthly |

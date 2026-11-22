# Access Package Design

## Purpose

This document explains the design of the access package used in this lab. The goal is to demonstrate how access to business resources can be requested, approved, assigned, reviewed, and removed using Microsoft Entra Entitlement Management.

## Design Principle

The access package is designed around the following IAM principles:

- Least privilege
- Business owner approval
- Time-bound access
- Clear ownership
- Repeatable access request process
- Audit-ready evidence
- Reduced manual Service Desk access changes

## Example Business Requirement

The Finance department needs to provide temporary access to a reporting workspace for internal users working on month-end or audit-related tasks.

Access should not be granted permanently by default. Users must provide a business reason, and the Finance Manager must approve the request.

## Access Package Name

```text
AP-FINANCE-REPORTING-TEMP
```

## Naming Standard

| Prefix | Meaning |
|---|---|
| AP | Access Package |
| FINANCE | Business function or department |
| REPORTING | Access purpose |
| TEMP | Temporary access |

## Catalogue Design

| Catalogue | Purpose |
|---|---|
| Business Access Packages | Standard business access packages for internal users |

A dedicated catalogue makes it easier to delegate access package management to business or IAM owners without granting excessive tenant-wide permissions.

## Access Package Scope

| Setting | Design |
|---|---|
| Target users | Internal employees |
| Guest users | Not included in this package |
| Request method | My Access portal |
| Assignment type | User requests access |
| Approval required | Yes |
| Assignment duration | 30 days |
| Extension allowed | Yes, with approval |
| Access review | Quarterly |

## Included Resources

The access package contains resources that represent a realistic business access bundle.

| Resource Type | Example Resource | Access Provided |
|---|---|---|
| Microsoft 365 Group | M365-FIN-Reporting-Readers | Group membership |
| SharePoint Online Site | Finance Reports | Read access |
| Teams Team | Finance Reporting Workspace | Member access |
| Enterprise Application | Finance Reporting App | User assignment |

## Resource Ownership

| Resource | Owner | Responsibility |
|---|---|---|
| Finance Reporting Group | Finance Manager | Confirms business need |
| Finance SharePoint Site | Finance Manager / Site Owner | Confirms document access requirement |
| Finance Reporting App | Application Owner | Confirms application entitlement |
| Access Package | IAM Team | Maintains package and policy configuration |

## Requestor Experience

The user completes the following steps:

1. Opens the My Access portal.
2. Searches for the Finance Reporting access package.
3. Selects the access package.
4. Provides business justification.
5. Submits the request.
6. Waits for approval.
7. Receives access automatically after approval.

## Example Business Justification

```text
I require temporary access to the Finance Reporting workspace to support the June month-end reconciliation activity. Access is required for 30 days and approved by my line manager.
```

## Access Assignment Flow

```text
Requestor
   |
   v
My Access portal
   |
   v
Access package policy
   |
   v
Approver decision
   |
   v
Resource assignment
   |
   v
User receives access
```

## Resource Assignment Model

The access package should assign access through groups where possible rather than direct permissions.

Preferred model:

```text
Access Package
   |
   v
Security Group / M365 Group
   |
   v
Application, SharePoint site, Team, or resource permission
```

This makes permissions easier to audit, review, and remove.

## Access Package Policy Design

| Policy Setting | Design Decision | Reason |
|---|---|---|
| Who can request | Users in organisation | Keeps package internal-only |
| Approval required | Yes | Prevents unapproved access |
| Approver | Finance Manager | Business owner approves business need |
| Justification | Required | Creates audit trail |
| Expiry | 30 days | Reduces standing access |
| Extension | Allowed with approval | Supports legitimate longer projects |
| Review | Quarterly | Detects access no longer required |

## Internal vs External Access

This lab package is designed for internal users only. For guest users, a separate package should be created with stricter controls, such as:

- Connected organisation configuration
- Sponsor approval
- Shorter expiry
- Guest access review
- Terms of use
- More restrictive Conditional Access policy

## Security Controls

| Control | Implementation |
|---|---|
| Least privilege | Package includes only required resources |
| Approval | Finance Manager approval required |
| Justification | Requestor must provide business reason |
| Time-bound access | Assignment expires after 30 days |
| Audit trail | Request, approval, and assignment events captured |
| Review | Quarterly review of package assignments |
| Separation of duties | IAM manages policy; business owner approves access |

## Operational Ownership

| Team | Responsibility |
|---|---|
| Service Desk | Direct users to My Access portal and handle basic request questions |
| IAM Team | Configure and maintain packages, policies, catalogues, and evidence |
| Business Owner | Approve or deny requests based on business need |
| Security Team | Review logs, excessive access, and governance posture |

## Out of Scope

The following items are not included in this basic lab but can be added as future improvements:

- External user access package
- Connected organisations
- Terms of use requirement
- Automated access reviews with manager attestation
- Integration with ticketing system
- Graph API automation
- Privileged access groups with PIM

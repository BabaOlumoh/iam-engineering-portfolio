# 06 - Entitlement Management with Microsoft Entra ID Governance

## Project Summary

This project demonstrates how Microsoft Entra Entitlement Management can be used to provide controlled, approved, time-bound access to business resources using access packages.

The lab simulates a real enterprise access governance process where users request access through the My Access portal, business owners approve or deny requests, access is automatically assigned after approval, and access expires after a defined period.

This project is designed to show practical Identity and Access Management skills beyond basic user administration. It focuses on access governance, least privilege, approval workflows, expiry, evidence capture, and operational handover to Service Desk and IAM teams.

## Scenario

An organisation has several Microsoft 365 resources that should not be assigned manually by Service Desk without approval. Examples include:

- SharePoint site access
- Document library access
- Teams membership
- Temporary access to Microsoft 365 groups
- Contractor or guest collaboration access
- Application access requiring manager or resource-owner approval

Instead of granting access manually, the IAM team creates an access package that contains the required resources and policies.

Users request access using the My Access portal. The request is routed for approval, access is granted only after approval, and the assignment expires automatically after the approved duration.

## Lab Objectives

The objective of this project is to demonstrate the following IAM capabilities:

- Design an access package for controlled access to business resources
- Define who can request access
- Configure approval requirements
- Require business justification from requesters
- Apply time-bound access expiry
- Demonstrate access assignment after approval
- Demonstrate access removal after expiry or manual removal
- Capture audit and test evidence
- Document the operational process for Service Desk and IAM teams

## Technologies Used

| Technology | Purpose |
|---|---|
| Microsoft Entra ID | Cloud identity platform |
| Microsoft Entra ID Governance | Entitlement management and access governance |
| Entitlement Management | Access packages, assignment policies, approval, expiry |
| My Access portal | User-facing access request portal |
| Microsoft 365 Groups / Teams | Example resource included in access package |
| SharePoint Online | Example resource included in access package |
| Entra audit logs | Evidence of requests, approvals, assignment, and removal |

## Example Access Package

| Field | Example Value |
|---|---|
| Access package name | AP-FINANCE-REPORTING-TEMP |
| Catalogue | Business Access Packages |
| Resource owner | Finance Manager |
| Requestor scope | Internal users only |
| Approval required | Yes |
| Approver | Finance Manager or Access Package Manager |
| Justification required | Yes |
| Assignment duration | 30 days |
| Extension allowed | Yes, with approval |
| Review frequency | Quarterly |
| Resources included | Finance Reporting Group, SharePoint Finance Reports site |

## Target Design

```text
User requires access
        |
        v
User requests access in My Access portal
        |
        v
Request requires justification
        |
        v
Approver reviews business need
        |
        v
Access package assignment approved
        |
        v
User receives group/site/application access
        |
        v
Access expires automatically after defined duration
        |
        v
IAM reviews evidence and access history
```

## Why This Project Matters

Many organisations still grant access through tickets and manual group membership changes. This can lead to excessive permissions, unclear ownership, lack of audit evidence, and poor leaver/mover controls.

Entitlement Management improves the process by making access:

- Request-based
- Approved by the right owner
- Time-bound
- Auditable
- Repeatable
- Easier to review
- Safer for temporary or sensitive access

## Repository Structure

```text
06-entitlement-management/
├── README.md
├── access-package-design.md
├── approval-expiry-policy.md
├── test-evidence.md
└── screenshots/
    ├── README.md
    └── .gitkeep
```

## Skills Demonstrated

- Microsoft Entra ID Governance
- Access package design
- Entitlement Management configuration
- Access request and approval workflow design
- Temporary access governance
- Least privilege access design
- Audit evidence capture
- Operational IAM documentation
- Service Desk handover process
- Access lifecycle management



## Example Outcome

Before this design, access was assigned manually through group membership changes. After implementing Entitlement Management, users request access through a controlled workflow, approvals are captured, access has a defined expiry, and IAM can review assignment evidence.

## Documentation Files

| File | Purpose |
|---|---|
| `access-package-design.md` | Explains the access package design, resources, requestor scope, and ownership model |
| `approval-expiry-policy.md` | Documents approval stages, justification, expiry, extensions, and removal rules |
| `test-evidence.md` | Provides evidence checklist and test scenarios for validating the lab |



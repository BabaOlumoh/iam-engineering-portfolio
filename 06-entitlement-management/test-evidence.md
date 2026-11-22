# Test Evidence

## Purpose

This document provides the test cases and evidence checklist for validating the entitlement management access package.

The aim is to prove that access requests, approvals, assignments, expiry, and removal behave as expected.

## Test Environment

| Item | Example Value |
|---|---|
| Tenant | ogkareemu.live lab tenant |
| Identity platform | Microsoft Entra ID |
| Governance feature | Microsoft Entra Entitlement Management |
| Access package | AP-FINANCE-REPORTING-TEMP |
| Requestor | Babatunde@ogkareemu.live |
| Approver | *@ogkareemu.live |
| Resource group | SG-Finance-Reporting-SharePoint |
| Assignment duration | 30 days |


## Test Case 1 - Access Package Exists

| Field | Detail |
|---|---|
| Objective | Confirm the access package exists with the correct name and catalog |
| Precondition | Access package has been created |
| Test steps | Open Entra admin center > Identity Governance > Entitlement Management > Access packages |
| Expected result | AP-FINANCE-REPORTING-TEMP is visible |
| Evidence | [06-entitlement-management/screenshots/access-package-overview.png](https://github.com/BabaOlumoh/iam-engineering-portfolio/blob/d48d22afc181f35ec15f4cae40e1652429d53035/06-entitlement-management/screenshots/access-package-overview.png) |
| Status | Pass |

## Test Case 2 - Resources Added to Access Package

| Field | Detail |
|---|---|
| Objective | Confirm correct resources are included |
| Precondition | Group or SharePoint resource exists |
| Test steps | Open access package > Resource roles |
| Expected result | Finance Reporting resource role is listed |
| Evidence | Screenshot of resource roles |
| Status | Pass / Fail |

## Test Case 3 - Requestor Scope is Correct

| Field | Detail |
|---|---|
| Objective | Confirm only intended users can request the package |
| Precondition | Policy has been created |
| Test steps | Open access package policy > Requests |
| Expected result | Requestor scope is limited to internal users or selected group |
| Evidence | Screenshot of request policy |
| Status | Pass / Fail |

## Test Case 4 - Approval is Required

| Field | Detail |
|---|---|
| Objective | Confirm approval is required before assignment |
| Precondition | Approver has been configured |
| Test steps | Open access package policy > Approval settings |
| Expected result | Approval is enabled and approver is configured |
| Evidence | Screenshot of approval settings |
| Status | Pass / Fail |

## Test Case 5 - Business Justification is Required

| Field | Detail |
|---|---|
| Objective | Confirm requestor must provide justification |
| Precondition | Request form questions configured |
| Test steps | Request package from My Access portal |
| Expected result | Request cannot be submitted without required information |
| Evidence | Screenshot of request form |
| Status | Pass / Fail |

## Test Case 6 - User Submits Access Request

| Field | Detail |
|---|---|
| Objective | Confirm user can submit a valid request |
| Precondition | User is in requestor scope |
| Test steps | My Access portal > select package > enter justification > submit |
| Expected result | Request is submitted and pending approval |
| Evidence | Screenshot of request history |
| Status | Pass / Fail |

## Test Case 7 - Approver Approves Request

| Field | Detail |
|---|---|
| Objective | Confirm approver can approve the request |
| Precondition | Request is pending approval |
| Test steps | Approver opens My Access portal > Approvals > approves request |
| Expected result | Request status changes to approved |
| Evidence | Screenshot of approval decision |
| Status | Pass / Fail |

## Test Case 8 - User Receives Access After Approval

| Field | Detail |
|---|---|
| Objective | Confirm access is assigned after approval |
| Precondition | Request has been approved |
| Test steps | Check user membership in target group/resource |
| Expected result | User is added to the resource through the access package assignment |
| Evidence | Screenshot of group membership or assignment state |
| Status | Pass / Fail |

## Test Case 9 - Assignment Has Expiry

| Field | Detail |
|---|---|
| Objective | Confirm assignment is time-bound |
| Precondition | Assignment is active |
| Test steps | Open access package assignments |
| Expected result | Assignment shows expiry date |
| Evidence | Screenshot of assignment details |
| Status | Pass / Fail |

## Test Case 10 - Access Can Be Removed

| Field | Detail |
|---|---|
| Objective | Confirm access can be removed manually or by expiry |
| Precondition | Assignment is active |
| Test steps | Remove assignment or allow expiry in test policy |
| Expected result | User no longer has resource access |
| Evidence | Screenshot of removed assignment and group membership |
| Status | Pass / Fail |

## Test Case 11 - Audit Logs Capture Activity

| Field | Detail |
|---|---|
| Objective | Confirm audit trail is available |
| Precondition | Request and approval activity has occurred |
| Test steps | Open Entra audit logs or entitlement management request history |
| Expected result | Request, approval, assignment, and removal events are visible |
| Evidence | Screenshot/export of audit logs |
| Status | Pass / Fail |

## Test Case 12 - Service Desk Process Validation

| Field | Detail |
|---|---|
| Objective | Confirm Service Desk does not bypass entitlement process |
| Precondition | User raises access request ticket |
| Test steps | Service Desk directs user to My Access portal instead of manually adding group |
| Expected result | Access follows approved governance workflow |
| Evidence | Example ticket note or runbook screenshot |
| Status | Pass / Fail |

## Evidence Checklist

| Evidence Item | Captured? |
|---|---|
| Access package overview | Yes / No |
| Catalogue configuration | Yes / No |
| Resource roles | Yes / No |
| Requestor scope | Yes / No |
| Approval settings | Yes / No |
| Lifecycle expiry settings | Yes / No |
| My Access request form | Yes / No |
| Request pending approval | Yes / No |
| Approval decision | Yes / No |
| Assignment active | Yes / No |
| User added to group/resource | Yes / No |
| Assignment expiry date | Yes / No |
| Manual removal or expiry evidence | Yes / No |
| Audit log evidence | Yes / No |

## Example Test Result Summary

| Test Area | Result | Notes |
|---|---|---|
| Access package creation | Pass | Package created in Business Access Packages catalogue |
| Requestor scope | Pass | Internal users can request |
| Approval workflow | Pass | Finance Manager approval required |
| Justification | Pass | Requestor must provide business reason |
| Assignment | Pass | User assigned after approval |
| Expiry | Pass | Assignment expires after configured duration |
| Audit evidence | Pass | Request and approval events visible |

## Lessons Learned

- Access packages reduce manual access assignment by Service Desk.
- Approval workflows make business owners accountable for access decisions.
- Expiry reduces the risk of users keeping access indefinitely.
- Evidence capture is important for audits and interviews.
- Access packages should be designed with clear ownership and naming standards.

## Interview Explanation

I built an entitlement management lab using Microsoft Entra ID Governance. The project demonstrates how access packages can be used to govern access to Microsoft 365 resources. Users request access through My Access, provide justification, and wait for business owner approval. Once approved, access is assigned automatically and expires after a defined period. I also captured test evidence showing request submission, approval, assignment, expiry, and audit logs.

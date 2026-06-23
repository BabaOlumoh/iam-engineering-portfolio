# Hybrid Identity Troubleshooting Guide

## Purpose

This document captures common issues, checks, and fixes for the hybrid identity lab using Active Directory, Microsoft Entra ID, Entra Connect Sync, custom domain `0gkareemu.live`, and the Microsoft Entra provisioning agent.

---

## Troubleshooting Method

When troubleshooting hybrid identity, use this order:

1. Confirm the source object in Active Directory.
2. Confirm the object is in sync scope.
3. Confirm the required attributes are valid and unique.
4. Run or wait for Entra Connect Sync.
5. Check Synchronization Service Manager.
6. Check Microsoft Entra ID user state.
7. Check provisioning agent health for Lifecycle Workflow on-prem actions.
8. Review Entra audit logs and provisioning logs.

---

## Issue 1: User Does Not Appear in Microsoft Entra ID

### Symptoms

- User exists in AD but not in Microsoft Entra ID.
- Delta sync completes but user is missing.
- No obvious error in Entra portal.

### Checks

#### Check user OU

```powershell
Get-ADUser john.smith -Properties DistinguishedName | Select-Object Name,DistinguishedName
```

Confirm the user is in an OU included in Entra Connect Sync scope.

#### Check sync scheduler

```powershell
Get-ADSyncScheduler
```

#### Run delta sync

```powershell
Start-ADSyncSyncCycle -PolicyType Delta
```

#### Review Synchronization Service Manager

Check:

- AD connector import.
- Connector space object.
- Metaverse object.
- Microsoft Entra connector export.

### Likely Causes

| Cause | Fix |
|---|---|
| User is outside synced OU | Move user into scoped OU or update OU filtering |
| Sync scheduler disabled | Re-enable sync scheduler |
| Attribute validation issue | Fix invalid/duplicate attributes |
| Export error | Review export error details |

---

## Issue 2: User Has Wrong Sign-In Domain

### Symptoms

- User appears as `user@tenant.onmicrosoft.com`.
- User does not appear as `user@0gkareemu.live`.
- User cannot sign in with expected UPN.

### Checks

#### Check AD user UPN

```powershell
Get-ADUser john.smith -Properties UserPrincipalName | Select-Object Name,UserPrincipalName
```

Expected:

```text
john.smith@0gkareemu.live
```

#### Check AD UPN suffixes

```powershell
Get-ADForest | Select-Object -ExpandProperty UPNSuffixes
```

Expected list includes:

```text
0gkareemu.live
```

#### Check Entra custom domain

Confirm the domain is verified in Microsoft Entra ID.

### Likely Causes

| Cause | Fix |
|---|---|
| Custom domain not verified | Verify `0gkareemu.live` in Entra ID |
| UPN suffix not added in AD | Add suffix in Active Directory Domains and Trusts |
| User still has internal UPN | Update AD user UPN |
| Sync has not run | Run delta sync |

---

## Issue 3: Duplicate Attribute Sync Error

### Symptoms

- User fails to export to Microsoft Entra ID.
- Sync error mentions duplicate UPN, mail, or proxy address.
- User exists in AD but not correctly in cloud.

### Checks

#### Search for duplicate UPN in AD

```powershell
Get-ADUser -Filter * -Properties UserPrincipalName |
Where-Object {$_.UserPrincipalName -eq "john.smith@0gkareemu.live"} |
Select-Object Name,SamAccountName,UserPrincipalName
```

#### Search for duplicate mail

```powershell
Get-ADUser -Filter * -Properties Mail |
Where-Object {$_.Mail -eq "john.smith@0gkareemu.live"} |
Select-Object Name,SamAccountName,Mail
```

### Fix

- Ensure UPN is unique.
- Ensure mail/proxy addresses are unique.
- Remove stale test users or update conflicting attributes.
- Run delta sync after correction.

---

## Issue 4: Attribute Does Not Sync to Entra ID

### Symptoms

- User appears in Entra ID, but department, job title, manager, or employee attributes are missing.
- Lifecycle Workflow does not trigger as expected.

### Checks

#### Check source AD attribute

```powershell
Get-ADUser john.smith -Properties Department,Title,Manager,EmployeeID |
Select-Object Name,Department,Title,Manager,EmployeeID
```

#### Run delta sync

```powershell
Start-ADSyncSyncCycle -PolicyType Delta
```

#### Check Synchronization Service Manager

Review attribute flow for the user object.

### Likely Causes

| Cause | Fix |
|---|---|
| Attribute missing in AD | Populate the attribute in AD |
| Attribute not in sync scope/rule | Review Entra Connect sync rules |
| Manager object not synced | Ensure manager account is also in sync scope |
| Workflow attribute missing | Confirm required lifecycle attribute is synced |

---

## Issue 5: Password or Sign-In Does Not Work

### Symptoms

- User exists in Entra ID but cannot sign in.
- Password hash sync appears delayed.
- User receives invalid username or password error.

### Checks

#### Confirm sign-in method

Check Entra Connect configuration for selected sign-in method.

#### Confirm account state in AD

```powershell
Get-ADUser john.smith -Properties Enabled,LockedOut,UserPrincipalName |
Select-Object Name,Enabled,LockedOut,UserPrincipalName
```

#### Force password change in lab if required

```powershell
Set-ADAccountPassword john.smith -Reset -NewPassword (ConvertTo-SecureString "TempPass123!" -AsPlainText -Force)
Enable-ADAccount john.smith
```

### Likely Causes

| Cause | Fix |
|---|---|
| User disabled in AD | Enable user if appropriate |
| Wrong UPN | Correct UPN to `@0gkareemu.live` |
| Password sync not completed | Wait or force sync |
| Account locked | Unlock account in AD |
| Conditional Access blocking | Review sign-in logs |

---

## Issue 6: Lifecycle Workflow Does Not Trigger

### Symptoms

- Workflow exists but does not run for the user.
- User is skipped.
- No leaver or mover action occurs.

### Checks

- Confirm user exists in Microsoft Entra ID.
- Confirm user is synchronized from AD.
- Confirm workflow scope includes the user.
- Confirm trigger attribute is populated.
- Confirm execution condition is correct.
- Check workflow run history.
- Check Entra audit logs.

### Example attributes to check

```powershell
Get-ADUser john.smith -Properties Department,EmployeeID,EmployeeType,Manager |
Select-Object Name,Department,EmployeeID,EmployeeType,Manager
```

### Likely Causes

| Cause | Fix |
|---|---|
| User outside workflow scope | Adjust workflow scope |
| Required attribute missing | Populate and sync required attribute |
| Date/time attribute not synced | Configure attribute synchronization |
| Workflow disabled | Enable workflow |
| User is not synced | Resolve Entra Connect Sync issue first |

---

## Issue 7: On-Premises Lifecycle Action Fails

### Symptoms

- Lifecycle Workflow runs but AD account is not disabled.
- Workflow task fails.
- Provisioning log shows agent or permissions issue.

### Checks

#### Check provisioning agent health

In Microsoft Entra admin center, verify that the provisioning agent is online and healthy.

#### Check server service state

On the agent server, check relevant Microsoft Entra provisioning services.

```powershell
Get-Service | Where-Object {$_.DisplayName -like "*provision*" -or $_.DisplayName -like "*Microsoft Entra*"} |
Select-Object DisplayName,Status,StartType
```

#### Check network connectivity to domain controller

```powershell
Test-NetConnection dc01.ad.iamhomelab.local -Port 389
Test-NetConnection dc01.ad.iamhomelab.local -Port 636
```

#### Check permissions

Confirm the service account or agent configuration has permission to perform the required AD action.

### Likely Causes

| Cause | Fix |
|---|---|
| Agent offline | Restart service or reinstall agent |
| Agent cannot reach domain controller | Fix DNS, firewall, or routing |
| Permission issue | Delegate required AD permissions |
| User not found in AD | Confirm source anchor and synced identity |
| Unsupported task | Use supported on-premises lifecycle tasks |

---

## Issue 8: Entra Connect and Cloud Sync Confusion

### Explanation

Microsoft Entra Connect Sync and Microsoft Entra Cloud Sync are different synchronization approaches. A lab may contain both the classic Entra Connect Sync server and the Microsoft Entra provisioning agent, but they should not be configured to synchronize the same objects in conflicting ways.

### Safe design principle

Use one primary sync engine for each object scope.

For this lab:

```text
Primary directory sync: Microsoft Entra Connect Sync
Provisioning agent: Lifecycle Workflow / on-premises provisioning support
```

Avoid configuring Entra Cloud Sync and Entra Connect Sync to manage the same user population unless the design is intentional and supported.

---

## Issue 9: User Disabled in Cloud But Re-Enabled After Sync

### Symptoms

- User is disabled in Microsoft Entra ID.
- After sync, the user becomes enabled again.

### Cause

In a hybrid model where the user is synchronized from Active Directory, the AD account state is authoritative. If the account is enabled in AD, the cloud state may be overwritten during synchronization.

### Fix

Disable the account in Active Directory first.

```powershell
Disable-ADAccount john.smith
Start-ADSyncSyncCycle -PolicyType Delta
```

Expected result:

```text
The disabled state flows from AD to Microsoft Entra ID.
```

---

## Issue 10: Domain Verification Fails

### Symptoms

- `0gkareemu.live` remains unverified in Microsoft Entra ID.
- TXT record verification fails.

### Checks

- Confirm DNS TXT record was created at the domain registrar/DNS provider.
- Confirm there are no spelling mistakes: `0gkareemu.live` uses the number zero at the beginning.
- Wait for DNS propagation.
- Confirm the TXT value matches exactly.

### Example DNS check

```powershell
Resolve-DnsName -Name 0gkareemu.live -Type TXT
```

---

## Issue 11: Manager Attribute Not Showing in Entra ID

### Symptoms

- User syncs to Entra ID.
- Manager is missing.
- Approval workflows cannot identify manager.

### Checks

```powershell
Get-ADUser john.smith -Properties Manager | Select-Object Name,Manager
```

Confirm:

- The manager attribute is populated.
- The manager account also exists in a synced OU.
- Delta sync has completed.

### Fix

- Populate the manager in AD.
- Ensure manager object is also synchronized.
- Run delta sync.

---

## Issue 12: Provisioning Agent Installation Error

### Symptoms

- Agent install fails.
- Agent registration fails.
- Agent does not appear in Entra portal.

### Checks

- Confirm admin permissions.
- Confirm outbound internet access.
- Confirm TLS/proxy inspection is not breaking Microsoft service communication.
- Confirm server time is correct.
- Review installation logs.

### Fix

- Run installer as administrator.
- Allow required outbound access.
- Re-register the agent.
- Restart the server if required.

---

## Operational Checklist

Use this checklist during every sync or lifecycle test.

| Check | Pass/Fail | Notes |
|---|---|---|
| AD user exists in correct OU |  |  |
| UPN uses `@0gkareemu.live` |  |  |
| Required attributes populated |  |  |
| User has expected AD groups |  |  |
| Delta sync completed |  |  |
| User visible in Entra ID |  |  |
| User shows as synced from AD |  |  |
| Provisioning agent healthy |  |  |
| Lifecycle Workflow scope matches user |  |  |
| Workflow run completed |  |  |
| On-prem action verified in AD |  |  |
| Cloud state updated after sync |  |  |

---

## Useful PowerShell Commands

### AD user check

```powershell
Get-ADUser john.smith -Properties * | 
Select-Object Name,SamAccountName,UserPrincipalName,Mail,Enabled,Department,Title,Manager
```

### Group membership check

```powershell
Get-ADPrincipalGroupMembership john.smith | Select-Object Name
```

### Sync scheduler check

```powershell
Get-ADSyncScheduler
```

### Start delta sync

```powershell
Start-ADSyncSyncCycle -PolicyType Delta
```

### Start initial sync

```powershell
Start-ADSyncSyncCycle -PolicyType Initial
```

### Check UPN suffixes

```powershell
Get-ADForest | Select-Object -ExpandProperty UPNSuffixes
```

### Test domain controller LDAP ports

```powershell
Test-NetConnection dc01.ad.iamhomelab.local -Port 389
Test-NetConnection dc01.ad.iamhomelab.local -Port 636
```

---

## Lessons Learned

- The UPN suffix should be planned before users are synchronized.
- A verified custom domain gives a cleaner sign-in experience.
- OU filtering reduces sync risk in labs and production.
- Hybrid users should normally be changed from AD, not directly in Entra ID.
- Sync errors are usually caused by scope, duplicates, invalid attributes, or permissions.
- Lifecycle Workflow on-premises actions require the provisioning agent and correct connectivity.
- Documentation and screenshots are essential because hybrid identity is operationally sensitive.

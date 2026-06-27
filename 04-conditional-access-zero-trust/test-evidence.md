# Test Evidence

## Purpose

This document records the evidence required to prove that the Conditional Access MFA enforcement project works as designed.

The evidence should show that MFA is enforced for standard users, stronger controls apply to administrators, legacy authentication is blocked, and break-glass accounts are protected from tenant lockout scenarios.

## Test Users

| Test User | Role | Purpose |
|---|---|---|
| adam.cole@ogkareemu.live | Standard user | Confirm MFA is required for normal access |
| admin@ogkareemu.live | Privileged user | Confirm stronger admin policy applies |
| breakglass@ogkareemu.live | Break-glass account | Confirm exclusion and emergency access path |


## Test Scope

| Policy | Test Status | Evidence Required |
|---|---|---|
| CA-001 - Require MFA - All Users | Passed | Policy config and sign-in logs |
| CA-002 - Require Strong MFA - Admin Roles | Passed | Admin sign-in and policy result |
| CA-003 - Block Legacy Authentication | Passed | Legacy auth sign-in blocked or report-only result |
| CA-004 - Require MFA - Security Info Registration | Passed | Security info registration prompt evidence |


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

1. Sign in as `adam.cole@ogkareemu.live`.
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
| Policy assignment includes user |
<img width="872" height="694" alt="Screenshot 2026-06-27 at 23 30 05" src="https://github.com/user-attachments/assets/053e66b8-3d55-4e37-aa6e-53745308b1e7" />
<img width="949" height="215" alt="Screenshot 2026-06-27 at 23 06 00" src="https://github.com/user-attachments/assets/cd285eb6-3bf3-4d6b-9e57-65e6a87dbccd" />

| Grant control requires MFA |
<img width="970" height="348" alt="Screenshot 2026-06-27 at 23 30 43" src="https://github.com/user-attachments/assets/298cc093-1fb3-4858-acb6-a5e43a2af444" />


## Test Case 2 - User Fails or Cancels MFA

### Objective

Confirm that access is not granted when MFA is not completed.

### Steps

1. Sign in as `adam.cole@ogkareemu.live`.
2. Trigger access to Microsoft 365.
3. Cancel or fail the MFA challenge.
4. Review sign-in logs.

### Expected Result

Access is interrupted or blocked because MFA was not satisfied.

### Evidence

<img width="548" height="627" alt="Screenshot 2026-06-27 at 23 19 16" src="https://github.com/user-attachments/assets/014c9174-375b-4002-9dd9-04f6bbfcc7d0" />


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
| Break-glass account excluded 
<img width="830" height="693" alt="Screenshot 2026-06-27 at 23 36 12" src="https://github.com/user-attachments/assets/2adcf77e-402c-42ff-bbe1-f120edd37452" />

| Sign-in successful 
<img width="744" height="491" alt="Screenshot 2026-06-27 at 23 35 00" src="https://github.com/user-attachments/assets/705b22dd-dac6-4418-bca7-717edf146e1d" />

| Alert generated


## Test Case 4 - Admin Requires Stronger MFA

### Objective

Confirm that privileged users must satisfy a stronger authentication requirement than standard users.

### Preconditions

- Admin test user is assigned an Entra admin role.
- `CA-002 - Require Strong MFA - Admin Roles` exists.
- Admin has an approved strong authentication method.

### Steps

1. Sign in as `admin@ogkareemu.live`.
2. Access Microsoft Entra admin center or Microsoft Admin Portal.
3. Observe the authentication requirement.
4. Complete the approved strong MFA method.
5. Review sign-in logs.

### Expected Result

The admin user must complete strong MFA before accessing admin resources.

### Evidence

<img width="996" height="613" alt="Screenshot 2026-06-27 at 23 55 46" src="https://github.com/user-attachments/assets/b12f718c-5cfa-44a6-b435-827c07661b5c" />
<img width="463" height="413" alt="Screenshot 2026-06-28 at 00 03 07" src="https://github.com/user-attachments/assets/daa6e09f-055e-4276-95a4-42fa03e3f299" />



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

<img width="977" height="708" alt="Screenshot 2026-06-27 at 23 56 51" src="https://github.com/user-attachments/assets/8e61420c-1b04-4dac-b082-4718f6669474" />


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
<img width="981" height="674" alt="Screenshot 2026-06-27 at 23 58 34" src="https://github.com/user-attachments/assets/9029119c-2b6c-4c49-abc2-99fb295d90df" />
<img width="1200" height="391" alt="Screenshot 2026-06-28 at 00 00 49" src="https://github.com/user-attachments/assets/1c06543c-6345-4b02-abbe-876cb7b9feb8" />
<img width="698" height="680" alt="Screenshot 2026-06-28 at 00 01 33" src="https://github.com/user-attachments/assets/2915dd61-cfa5-4a5d-b50d-6576626e91a0" />





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
| MFA enforced for all standard users | Standard user is prompted for MFA | Passed |
| Admins require stronger authentication | Admin user must satisfy stronger control | Passed |
| Break-glass account excluded | Emergency account can sign in during approved test | Passed |
| Break-glass usage monitored | Alert generated on sign-in | Passed |
| Legacy auth blocked | Legacy auth attempt denied after enforcement | Passed |

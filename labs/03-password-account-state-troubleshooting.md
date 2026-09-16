# Lab 03: Password and Account-State Troubleshooting

## Objective

Practice common Tier 1 Windows account-support tasks by resetting a local user's password, disabling and re-enabling the account, and verifying the account's final state.

## Environment

- Windows 11 Pro ARM
- UTM virtual machine
- `Donovan` — Administrator
- `HelpDeskTest` — Standard local user

## Scenario

A local Windows user needed account maintenance performed from an administrator account.

The goal was to practice password reset and account-state troubleshooting, then verify that the user could successfully sign in after the account was restored.

## Administrator and User Roles

For this lab, I used:

- `Donovan` as the IT / administrator account
- `HelpDeskTest` as the end-user account being supported

Administrative changes were performed from `Donovan`, while login behavior was tested from `HelpDeskTest`.

## Password Reset

From the administrator account, I reset the password for `HelpDeskTest`.

I then tested the account to confirm the previous credentials no longer worked and the new password could be used successfully.

## Account Disable Test

I disabled the `HelpDeskTest` account from the administrator account.

I then attempted to sign into `HelpDeskTest` and verified that Windows prevented the login while the account was disabled.

This reproduced a common help-desk scenario where a user cannot authenticate because the account itself is inactive.

## Account Re-Enable Test

After confirming the login failure, I re-enabled `HelpDeskTest`.

I signed into the account again and verified that normal access was restored.

## Inspecting the Account State

I inspected the account from the command line using:

```powershell
net user HelpDeskTest
```

The account information showed:

- Account active: `Yes`
- User may change password: `Yes`
- Local Group Memberships: `Users`
- Global Group memberships: `None`

The output also showed a recent Last logon timestamp, confirming that the account had successfully authenticated after the password and account-state changes.

### Account State Verification

<img width="614" height="520" alt="image" src="https://github.com/user-attachments/assets/ebecebc7-abdf-4641-9370-87ddda1304ce" />

## Final Result

The account troubleshooting sequence was completed successfully.

`HelpDeskTest` was:

- managed from the administrator account
- given a new password
- tested with the updated credentials
- disabled
- confirmed unable to sign in while disabled
- re-enabled
- confirmed able to sign in again
- inspected with `net user` to verify its final state

## What I Learned

A login problem is not always caused by an incorrect password.

The account itself may be disabled or otherwise inactive, so troubleshooting should include checking both credentials and account state.

I also learned how `net user` can be used to inspect a local account and verify information such as account status, password properties, group membership, and recent logon activity.

## Skills Demonstrated

- Windows Account Administration
- Password Reset
- Account Disable / Enable
- Login Troubleshooting
- Local User Management
- `net user`
- Standard User Accounts
- Authentication Troubleshooting
- Verification and Testing

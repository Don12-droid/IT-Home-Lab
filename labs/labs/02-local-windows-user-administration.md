# Lab 02: Local Windows User Administration

## Objective

Practice creating and managing a local Windows account, changing its privilege level, and verifying that administrator access was successfully removed.

## Environment

- Windows 11 Pro ARM
- UTM virtual machine
- `Donovan` — Administrator
- `HelpDeskTest` — Local test account

## Scenario

A local Windows account needed to be created, temporarily given administrator privileges, and then returned to Standard-user access.

The goal was to verify the account's final privilege level using both Windows Settings and command-line tools.

## Creating the Test Account

I created a local Windows account named `HelpDeskTest`.

### Local Account

<img width="441" height="313" alt="image" src="https://github.com/user-attachments/assets/34f840d1-ed30-4ea0-b618-66f8bff73f39" />

## Testing the User Account

I signed into `HelpDeskTest` and verified the active account from PowerShell.

```powershell
whoami
```

The command returned the `HelpDeskTest` account.

### Logged-In User Verification

<img width="826" height="239" alt="image" src="https://github.com/user-attachments/assets/58ec1c2e-1937-4901-8b08-ae1f75d644b8" />

## Changing Account Privileges

During the lab, I temporarily changed `HelpDeskTest` from a Standard user to an Administrator account.

After testing the account with elevated privileges, I changed it back to Standard-user access.

The important final step was verifying that the downgrade actually took effect.

## Verifying Administrator Group Membership

I ran:

```powershell
net localgroup administrators
```

The local Administrators group contained:

- `Administrator`
- `Donovan`

`HelpDeskTest` was not listed.

This confirmed that the account was no longer a member of the local Administrators group.

### Administrators Group Verification

<img width="1112" height="414" alt="image" src="https://github.com/user-attachments/assets/62ad3519-0a58-4490-b883-59b85da65740" />

## Verifying Restricted Administrative Access

During the original lab, I also tested an administrator-only command:

```powershell
net session
```

The command returned:

```text
System error 5 has occurred.

Access is denied.
```

This provided an additional check that `HelpDeskTest` was operating without administrative privileges.

<img width="1106" height="301" alt="image" src="https://github.com/user-attachments/assets/e5525db4-304c-4673-99af-283709b999cf" />


## Final Result

`HelpDeskTest` was successfully:

- created as a local Windows account
- configured and tested as a Standard user
- temporarily promoted to Administrator
- signed into successfully
- changed back to Standard-user access
- verified as absent from the local Administrators group
- confirmed unable to perform an administrator-only operation

## What I Learned

Changing an account type in the Windows interface should be followed by verification.

I learned how to confirm the account's actual privilege state using:

```powershell
whoami
```

to verify the active user,

```powershell
net localgroup administrators
```

to inspect local administrator membership, and

```powershell
net session
```

to test whether the current session had administrative privileges.

This reinforced the importance of verifying the effective account state rather than relying only on what the Windows Settings interface displays.

## Skills Demonstrated

- Windows Local User Administration
- Standard vs. Administrator Accounts
- Local Group Membership
- PowerShell
- Privilege Verification
- Least Privilege
- Windows Account Management
- Troubleshooting

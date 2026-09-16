# Lab 05: Software Installation, UAC, and Permission Troubleshooting

## Objective

Diagnose why a Standard Windows user could not install software, restore proper UAC elevation behavior, and complete a successful install and uninstall.

## Environment

- Windows 11 Pro ARM
- UTM VM
- `Donovan` — Administrator
- `HelpDeskTest` — Standard user
- 7-Zip 26.03 ARM64

## Problem

While logged into `HelpDeskTest`, the 7-Zip installer attempted to write to:

```text
C:\Program Files\7-Zip\
```

and failed with:

```text
Access is denied.
```

<img width="306" height="224" alt="image" src="https://github.com/user-attachments/assets/06daeffb-d218-41e3-848d-093efa7d9357" />

## Troubleshooting

I verified the account was not an administrator using:

```powershell
whoami
net localgroup administrators
net session
```

`net session` returned `Access is denied`, confirming the shell was not elevated.

I then tested **Run as administrator**, but no credential prompt appeared.

Attempting to open `secpol.msc` from the Standard account also failed with a permissions error.

<img width="1114" height="615" alt="image" src="https://github.com/user-attachments/assets/484393d2-3e0f-40a9-bc11-2a822bdb1bc5" />


## Root Cause

From the administrator account, I reviewed:

**Local Security Policy → Local Policies → Security Options**

I found:

- **Behavior of the elevation prompt for standard users:** Prompt for credentials
- **Run all administrators in Admin Approval Mode:** Disabled

Admin Approval Mode being disabled prevented normal UAC elevation behavior.

<img width="1200" height="590" alt="image" src="https://github.com/user-attachments/assets/2fa7fa66-2179-4b54-a03f-ca8e3d12149b" />

## Fix

I enabled:

**User Account Control: Run all administrators in Admin Approval Mode**

Windows required a restart.

After rebooting, `HelpDeskTest` correctly received a UAC credential prompt when using **Run as administrator**.

<img width="450" height="553" alt="image" src="https://github.com/user-attachments/assets/b8e92be1-36fb-4b41-aa80-1abdaa31a912" />


## Verification

After entering administrator credentials, I ran:

```powershell
net session
```

It returned:

```text
There are no entries in the list.
```

Earlier, the same command returned `Access is denied`, so this showed the PowerShell session was now elevated.

I then:

- installed 7-Zip successfully
- launched 7-Zip File Manager
- verified it under Installed Apps
- uninstalled it using UAC elevation

<img width="988" height="526" alt="image" src="https://github.com/user-attachments/assets/8a705a81-8074-4301-96ee-836a785b368c" />

<img width="1053" height="242" alt="image" src="https://github.com/user-attachments/assets/341bf2ab-9216-4b2b-890e-4091180c908d" />

<img width="312" height="221" alt="image" src="https://github.com/user-attachments/assets/fb824e82-18d9-4538-b1de-668b41f4ae07" />

<img width="298" height="214" alt="image" src="https://github.com/user-attachments/assets/9dcb9705-a218-4280-87ff-d23033922621" />

## Result

The issue was resolved by correcting the UAC policy configuration and verifying proper administrative elevation.

## Skills Demonstrated

- Windows 11 Administration
- User Account Control
- Local Security Policy
- Standard vs. Administrator Accounts
- Software Installation and Removal
- PowerShell
- Troubleshooting
- Root-Cause Analysis

## What I Learned

A software installation failure may not be caused by the installer itself. In this case, the underlying issue was a Windows security-policy configuration that prevented normal elevation behavior.

# Lab 08: Windows Update and System Repair Troubleshooting

## Objective

Practice diagnosing Windows system-health issues using Windows Update, System File Checker, and DISM.

## Environment

- Windows 11 Pro ARM
- UTM virtual machine
- `Donovan` — Administrator account
- Windows Update
- Command Prompt / PowerShell
- System File Checker
- DISM

## Scenario

This home lab simulates a help-desk investigation into possible Windows system corruption or update-related issues.

The goal is to review Windows Update status, check system-file integrity, evaluate Windows image health with DISM, and verify the system is in a healthy state.
## Troubleshooting Timeline

## Checking Windows Update Status

I first reviewed Windows Update to check whether the system was missing any current updates.

Windows reported:

```text
You're up to date
```

The system had checked for updates successfully and no pending updates were shown.

This established that the simulated issue was not being caused by an obviously outdated Windows installation.

### Windows Update Status

<img width="1007" height="792" alt="image" src="https://github.com/user-attachments/assets/7c4106ff-9943-405d-ab70-3e89870b9b91" />

## Running System File Checker

I opened PowerShell as an administrator and ran:

```powershell
sfc /scannow
```

System File Checker completed the verification process and reported:

```text
Windows Resource Protection did not find any integrity violations.
```

This indicated that Windows protected system files were intact and no repair was required at this stage.

### System File Checker Result

<img width="973" height="510" alt="image" src="https://github.com/user-attachments/assets/3f108b87-e8de-460e-905a-c1cc9c2638a3" />

## Checking the Windows Component Store

I continued the system-health check by running DISM from an elevated PowerShell session:

```powershell
DISM /Online /Cleanup-Image /ScanHealth
```

DISM completed successfully and reported:

```text
No component store corruption detected.
```

This indicated that the Windows component store was healthy and did not require repair.

### DISM ScanHealth Result

<img width="818" height="234" alt="image" src="https://github.com/user-attachments/assets/05a93fba-977f-4db3-ac52-1eb834b73249" />

## Verification

To perform a final health check on the Windows component store, I ran:

```powershell
DISM /Online /Cleanup-Image /CheckHealth
```

DISM reported:

```text
No component store corruption detected.
The operation completed successfully.
```

This confirmed that the Windows image remained healthy and that no component-store repair was required.

### Final DISM Health Check

<img width="588" height="155" alt="image" src="https://github.com/user-attachments/assets/184da251-8ed1-4a83-b10d-d5c06183731d" />

## Final Result

The simulated Windows system-repair investigation was completed successfully.

Windows Update showed that the system was current with no pending updates.

System File Checker reported:

```text
Windows Resource Protection did not find any integrity violations.
```

DISM ScanHealth reported:

```text
No component store corruption detected.
```

A final DISM CheckHealth verification returned the same healthy result.

Because no corruption was detected, no repair action was necessary.

## What I Learned

Windows Update, SFC, and DISM can be used together to investigate Windows system-health issues.

I learned that `sfc /scannow` checks protected Windows system files for corruption, while DISM can check the health of the Windows component store.

I also learned that repair commands should not be run unnecessarily. In this lab, both SFC and DISM reported healthy results, so there was no reason to force a repair.

Most importantly, I verified the system state using multiple tools instead of relying on a single result.

## Skills Demonstrated

- Windows Update Troubleshooting
- Windows System File Checker
- `sfc /scannow`
- DISM
- `DISM /ScanHealth`
- `DISM /CheckHealth`
- Windows Component Store Health
- Elevated PowerShell
- System Integrity Verification
- Windows Troubleshooting Methodology





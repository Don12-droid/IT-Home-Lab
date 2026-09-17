# Lab 08: Windows Update and System Repair Troubleshooting

## Objective

Practice diagnosing and repairing a Windows system using Windows Update, System File Checker, and DISM.

## Environment

- Windows 11 Pro ARM
- UTM virtual machine
- `Donovan` — Administrator account
- Windows Update
- Command Prompt / PowerShell
- System File Checker
- DISM

## Scenario

This home lab simulates a help-desk ticket where a user reports that Windows has been behaving inconsistently and may have missing or corrupted system files.

The goal is to review Windows Update status, check system-file integrity, run Windows image repair tools, and verify the system returns a healthy result.

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

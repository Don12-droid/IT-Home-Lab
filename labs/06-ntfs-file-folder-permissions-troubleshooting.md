# Lab 06: NTFS File and Folder Permissions Troubleshooting

## Objective

Diagnose and resolve a Windows file-permission issue affecting a Standard user.

## Environment

- Windows 11 Pro ARM
- UTM VM
- `Donovan` — Administrator
- `HelpDeskTest` — Standard user

## Scenario

A Standard user can access a folder and read files but cannot modify existing files or create new ones.

## Troubleshooting Timeline

Configured `HelpDeskTest` with read-only NTFS permissions on:

`C:\HelpDeskLab\SharedDocs`

The account was allowed:

- Read & execute
- List folder contents
- Read

The account was not allowed:

- Modify
- Write
- Full control

<img width="362" height="449" alt="image" src="https://github.com/user-attachments/assets/a278fd29-7825-447c-b3cf-895b175fe9d9" />


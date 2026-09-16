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

## Unexpected Write Access

After assigning `HelpDeskTest` read-only permissions, I tested the folder from the Standard user account.

I expected the account to be unable to modify the folder, but I was still able to:

- edit and save `test-document.txt`
- create a second text file
- save content to the new file

This meant another permission was still granting write access.

I checked the folder's ACL with:

```powershell
icacls C:\HelpDeskLab\SharedDocs
```

The output showed that `HelpDeskTest` had `RX` (Read & Execute), but `Authenticated Users` had inherited `M` (Modify) permission.

Because `HelpDeskTest` is an authenticated user, that inherited Modify permission explained why the account could still create and edit files.

### ACL Before Fix
<img width="740" height="403" alt="image" src="https://github.com/user-attachments/assets/a8756345-803a-496b-8b86-fd32bf6b581d" />


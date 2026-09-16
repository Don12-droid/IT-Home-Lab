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

## Inspecting Inherited Permissions

I opened **Advanced Security Settings** for `C:\HelpDeskLab\SharedDocs` to determine why the Standard user still had write access.

The permissions list showed that `Authenticated Users` had **Modify** access inherited from `C:\`.

Because `HelpDeskTest` is an authenticated user, this inherited permission overrode the expected read-only behavior.

### Inherited Modify Permission
<img width="762" height="516" alt="image" src="https://github.com/user-attachments/assets/e22a156f-b30a-40eb-809b-49b07618bae2" />

### Inheritance Disabled

I disabled inheritance on `SharedDocs` and converted the inherited permissions into explicit permissions.

This allowed me to isolate the unwanted `Authenticated Users` entry without changing permissions on the parent folder.

At this point, `Authenticated Users` still had **Modify** access, but it was no longer inherited.
<img width="754" height="508" alt="image" src="https://github.com/user-attachments/assets/babe63a3-ac96-47bb-be21-6b284693c8d0" />

### Permission Fix Applied

I removed the explicit `Authenticated Users` **Modify** permission from `SharedDocs`.

The final ACL now showed:

- `HelpDeskTest` — Read & execute
- `Users` — Read & execute
- `Donovan` — Full control
- `Administrators` — Full control
- `SYSTEM` — Full control

`Authenticated Users` no longer had Modify access.

### Final ACL After Fix

<img width="758" height="514" alt="image" src="https://github.com/user-attachments/assets/3c83fcb6-4d18-4bbb-aabb-17418391c533" />




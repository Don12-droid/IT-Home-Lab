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

## Verifying the Fix

I signed back into `HelpDeskTest` and retested access to `SharedDocs`.

The account could still access the folder, but an operation requiring Modify permission triggered a **Destination Folder Access Denied** prompt and required administrator confirmation.

This showed that `HelpDeskTest` no longer had unrestricted Modify access through `Authenticated Users`.

### Standard User Permission Restriction

<img width="506" height="181" alt="image" src="https://github.com/user-attachments/assets/44566291-7185-45d3-8be3-74a72a2b77f9" />

### Delete Permission Test

While signed into `HelpDeskTest`, I attempted to delete `test document 2`.

Windows blocked the operation and displayed **File Access Denied**, requiring administrator permission to continue.

This confirmed that the Standard user no longer had Delete permission on the folder contents.

### Delete Access Denied

<img width="452" height="225" alt="image" src="https://github.com/user-attachments/assets/f3c84b3c-fff5-41c1-b525-961aea06cc44" />

### Write Permission Test

To verify that `HelpDeskTest` could no longer modify the file, I tested write access directly from PowerShell:

```powershell
Add-Content "C:\HelpDeskLab\SharedDocs\test-document.txt" "PowerShell write test"
```

PowerShell returned an **Access denied** error and `UnauthorizedAccessException`.

This confirmed that the Standard user no longer had write permission to the file.

### PowerShell Write Access Denied

<img width="1108" height="605" alt="image" src="https://github.com/user-attachments/assets/3c650571-c5aa-4c9a-b8ec-b57ccec3240e" />

## Final ACL Verification

After removing the unwanted Modify permission, I checked the folder ACL again:

```powershell
icacls C:\HelpDeskLab\SharedDocs
```

The final ACL showed:

- `HelpDeskTest` — `RX` (Read & Execute)
- `Users` — `RX` (Read & Execute)
- `Donovan` — Full Control
- `Administrators` — Full Control
- `SYSTEM` — Full Control

The previous `Authenticated Users` Modify permission was no longer present.

This matched the retest results: `HelpDeskTest` could still read files, but write and delete operations were blocked.

### Final ACL

<img width="651" height="195" alt="image" src="https://github.com/user-attachments/assets/50938bcb-be52-4aec-b1f3-1d8fc87b45eb" />

## Final Result

The original read-only configuration did not work as expected because `HelpDeskTest` was still receiving inherited Modify access through the `Authenticated Users` group.

After disabling inheritance, converting inherited permissions to explicit permissions, and removing the unwanted `Authenticated Users` Modify entry:

- `HelpDeskTest` retained read access
- file modification was blocked
- file deletion required administrator approval
- PowerShell write attempts returned Access Denied
- the final ACL showed only Read & Execute access for the Standard user

## What I Learned

Direct permissions are not always the same as effective permissions. A user can receive additional access through group membership and inherited ACL entries.

When the expected permission restriction did not work, checking the folder with `icacls` revealed that `Authenticated Users` was still granting Modify access.

This lab showed why NTFS troubleshooting requires checking both direct and inherited permissions rather than only looking at the user's individual permission entry.

## Skills Demonstrated

- NTFS Permissions
- Access Control Lists (ACLs)
- Permission Inheritance
- Effective Permissions
- Windows File Security
- `icacls`
- Standard vs. Administrator Accounts
- PowerShell
- Access-Denied Troubleshooting
- Root-Cause Analysis


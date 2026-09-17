# Lab 07: Device Manager and Driver Troubleshooting

## Objective

Practice troubleshooting a Windows device issue using Device Manager, driver information, device status, and safe recovery steps.

## Environment

- Windows 11 Pro ARM
- UTM virtual machine
- `HelpDeskTest` — Standard user
- Device Manager

## Scenario

This home lab simulates a help-desk ticket involving a Windows device that is not functioning correctly.

The goal is to inspect the device in Device Manager, review its status and driver information, perform a safe troubleshooting action, and verify normal operation afterward.

## Troubleshooting Timeline

## Standard User Limitation

I first opened Device Manager while signed into the `HelpDeskTest` standard user account.

Windows allowed me to view device information but displayed a warning that administrative privileges were required to make device changes.

This confirmed that device configuration and driver-management tasks require elevated permissions.

### Device Manager Standard User Warning

<img width="410" height="153" alt="image" src="https://github.com/user-attachments/assets/c7c4f402-6c01-4fd8-8f9d-54465138f0a9" />

## Switching to the Administrator Account

Because the standard user account could not make device changes, I switched to the `Donovan` administrator account.

I reopened Device Manager and reviewed the hardware devices available inside the Windows virtual machine.

The VM included several virtual devices, including:

- Red Hat VirtIO GPU DOD controller
- Red Hat VirtIO Ethernet Adapter
- High Definition Audio Device
- WAN Miniport devices

I chose the High Definition Audio Device for the troubleshooting exercise because modifying the virtual network or display adapter could disrupt the VM.

## Inspecting the Audio Device

I opened the properties for the `High Definition Audio Device` under:

`Sound, video and game controllers`

Before making any changes, I reviewed the device status and driver information to establish a baseline.

On the General tab, Windows reported:

```text
This device is working properly.
```

This confirmed that the device was functioning normally before the troubleshooting simulation began.

### Device Status Before Troubleshooting

<img width="395" height="449" alt="image" src="https://github.com/user-attachments/assets/23d5782d-cbea-4d45-82f7-330e8f71ba75" />

## Reviewing Driver Information

I then opened the Driver tab to inspect the currently installed driver.

The device was using a Microsoft driver.

The driver information showed:

- Driver Provider: Microsoft
- Driver Date: 8/25/2026
- Driver Version: 10.0.26100.9278
- Digital Signer: Microsoft Windows

Device Manager also provided options to:

- update the driver
- disable the device
- uninstall the device

The Roll Back Driver option was unavailable, indicating that a previous driver version was not currently available for rollback.

### Installed Driver Information

<img width="394" height="445" alt="image" src="https://github.com/user-attachments/assets/bffbe079-0b20-4b9d-808c-433e38514dd7" />

## Simulating the Device Failure

To create a controlled troubleshooting scenario, I disabled the High Definition Audio Device from Device Manager.

Windows displayed a warning that disabling the device would cause it to stop functioning. I reviewed the warning before confirming the change.

### Device Disable Warning

After disabling the device, I reopened its properties and reviewed the General tab.

Windows reported:

```text
This device is disabled. (Code 22)
```

This confirmed that the device was disabled and that Windows correctly identified the problem state.

### Device Disabled - Code 22

<img width="400" height="451" alt="image" src="https://github.com/user-attachments/assets/2af7edc4-2822-4398-93d0-2b066b7ee623" />

## Restoring the Device

Device Manager provided an **Enable Device** option after the audio device was disabled.

I used this option to re-enable the High Definition Audio Device.

## Verification

After re-enabling the device, I reopened its properties and checked the General tab.

Windows reported:

```text
This device is working properly.
```

This confirmed that the device had been successfully restored and that the Code 22 condition was resolved.

### Final Device Status

<img width="392" height="445" alt="image" src="https://github.com/user-attachments/assets/a68326f0-8aeb-4265-b619-2ce9d0b6cb18" />

## Final Result

The simulated device issue was successfully diagnosed and resolved.

The High Definition Audio Device was initially functioning normally. After the device was intentionally disabled, Windows reported:

```text
This device is disabled. (Code 22)
```

Using Device Manager, I re-enabled the device and verified that Windows returned the status to:

```text
This device is working properly.
```

The lab demonstrated how Device Manager can be used to inspect device status, review driver information, identify a disabled-device error, and restore normal operation.

## What I Learned

Device Manager provides both device-status information and driver-management options that can help diagnose hardware-related issues.

I learned that Code 22 indicates that a device has been disabled.

I also learned that device-management changes require administrative privileges, while a standard user can still view device information.

Most importantly, I verified the device state before and after making changes instead of assuming that the recovery action worked.

## Skills Demonstrated

- Windows Device Manager
- Device Status Troubleshooting
- Driver Inspection
- Device Enable / Disable
- Code 22 Troubleshooting
- Administrator vs. Standard User Permissions
- Windows Hardware Troubleshooting
- Troubleshooting Verification
- Help-Desk Troubleshooting Methodology






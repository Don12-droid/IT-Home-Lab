# Lab 01: Windows Performance and High-CPU Process Troubleshooting

## Objective

Practice diagnosing a simulated Windows performance issue involving elevated CPU usage and unnecessary startup applications.

## Environment

- Windows 11 Pro ARM
- UTM virtual machine
- `HelpDeskTest` — Standard user
- Task Manager
- Startup Apps
- Windows process monitoring

## Scenario

This home lab simulates a help-desk ticket where a user reports that Windows feels slow after login.

The goal is to inspect system resource usage, identify the process responsible for elevated CPU usage, terminate it safely, review startup applications, and verify that system performance returns to normal.

## Troubleshooting Timeline

## Baseline System State

Before simulating the issue, I opened Task Manager to establish a baseline for normal system resource usage.

At the time of the baseline:

- CPU usage was approximately 7%
- Memory usage was approximately 57%
- Disk usage was approximately 1%
- Network usage was 0%
- Microsoft Edge, Notepad, Task Manager, and Windows Explorer were running normally

### Baseline Task Manager View

<img width="764" height="585" alt="image" src="https://github.com/user-attachments/assets/5b52fc7f-ec16-46d0-86f6-492180bea030" />

## Identifying the High-CPU Process

After the system showed elevated CPU usage, I sorted Task Manager by the CPU column to identify which process was consuming the most resources.

Windows PowerShell was using approximately 22% CPU, making it the largest contributor to the elevated system load.

### High-CPU Process Identified

<img width="755" height="568" alt="image" src="https://github.com/user-attachments/assets/44778025-e28c-4ed0-92f9-6b072e073aa7" />

## Resolving the High-CPU Process

After identifying Windows PowerShell as the primary source of elevated CPU usage, I ended the high-CPU process through Task Manager.

I then waited for system resource usage to stabilize and checked Task Manager again.

CPU usage dropped from approximately 37% during the problem state to approximately 2%.

This confirmed that the abnormal PowerShell process was responsible for the elevated CPU usage and that terminating it resolved the simulated performance issue.

### Post-Resolution Verification

<img width="756" height="577" alt="image" src="https://github.com/user-attachments/assets/1dbed951-28c6-4e5a-abd1-aa1fbb499c62" />

## Reviewing Startup Applications

I reviewed the Startup apps section in Task Manager to identify applications that could contribute to slower startup or increased background resource usage.

Two enabled applications were marked with a High startup impact:

- Microsoft Edge
- OneDrive

Other startup applications were either disabled, low impact, or not yet measured.

### Startup Apps Review

<img width="746" height="572" alt="image" src="https://github.com/user-attachments/assets/78164d5a-f9d9-4913-bfe2-f60ef040abb6" />

## Reducing Startup Impact

Microsoft Edge was enabled as a startup application and was marked with a High startup impact.

Because Edge is not required for Windows to function, I disabled it from Startup apps to reduce unnecessary startup load.

I left other startup items unchanged because they were either required, already disabled, or not necessary to modify for this lab.

### Microsoft Edge Disabled at Startup

<img width="760" height="566" alt="image" src="https://github.com/user-attachments/assets/14291dfd-a4ef-4c61-b46e-e9f70243118a" />

## Final Result

The simulated Windows performance issue was successfully identified and resolved.

Task Manager showed that Windows PowerShell was consuming significantly more CPU than the other running processes. After ending the high-CPU PowerShell process, overall CPU usage dropped from approximately 37% to approximately 2%.

I also reviewed Startup apps and identified Microsoft Edge as an enabled application with High startup impact. I disabled Edge from startup to reduce unnecessary background load during login.

The system returned to a normal resource-usage state after the troubleshooting steps were completed.

## What I Learned

Task Manager can be used to quickly identify which processes are responsible for abnormal CPU, memory, disk, or network usage.

Sorting processes by CPU usage made it easier to identify the process contributing most heavily to the slowdown.

I also learned that startup applications can contribute to slower login and unnecessary background resource usage. Reviewing startup impact helps determine which nonessential applications may be appropriate to disable.

Most importantly, I learned to verify the result after making a change rather than assuming the issue was resolved. In this lab, CPU usage dropped from approximately 37% to approximately 2% after the high-CPU process was terminated.

## Skills Demonstrated

- Windows Performance Troubleshooting
- Task Manager
- CPU Usage Analysis
- Process Identification
- Process Termination
- Startup Application Management
- Startup Impact Analysis
- Windows Resource Monitoring
- Troubleshooting Verification
- Help-Desk Troubleshooting Methodology


# Lab 01: Windows Performance and Frozen Application Troubleshooting

## Objective

Practice diagnosing a simulated Windows performance issue involving a slow user session and an unresponsive application.

## Environment

- Windows 11 Pro ARM
- UTM virtual machine
- `HelpDeskTest` — Standard user
- Task Manager
- Startup Apps
- Windows process monitoring

## Scenario

This home lab simulates a help-desk ticket where a user reports that Windows feels slow after login and an application has become unresponsive.

The goal is to inspect system resource usage, identify the affected process, safely terminate it, review startup applications, and verify that the system returns to a normal state.

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


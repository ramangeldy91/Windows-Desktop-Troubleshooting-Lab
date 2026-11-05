# Windows & Desktop Troubleshooting Lab

## Project Overview

This project is a practical Help Desk L1 troubleshooting lab focused on diagnosing, resolving, validating, and documenting common Windows desktop issues.

The lab uses a personal Windows 11 laptop as a controlled learning environment to practice realistic IT support workflows using Windows GUI tools and PowerShell.

All incidents are clearly identified as either simulated scenarios or actual observations from the personal lab environment.

## Project Objectives

- Develop systematic Windows troubleshooting skills.
- Identify common desktop and operating system issues.
- Perform safe diagnostic procedures.
- Apply appropriate troubleshooting solutions.
- Validate results after resolution.
- Document incidents, decisions, and technical findings.
- Maintain clear evidence of practical work.

## Technologies & Tools

- Windows 11 Pro Education
- PowerShell
- Command Prompt (CMD)
- Task Manager
- Resource Monitor
- Device Manager
- Event Viewer
- Reliability Monitor
- Windows Settings
- Windows Time Service (W32Time)

## Troubleshooting Scenarios

### Planned Scenarios

1. Windows Update
2. Disk Space & Storage
3. Application Troubleshooting
4. User Profile Issues
5. Basic Windows Diagnostics

### Completed Cases

#### INC-001 - Computer Performance Troubleshooting

**Scenario type:** Simulated Help Desk L1 troubleshooting exercise.

**Issue:** A user reported slow application performance and intermittent system freezes affecting multiple applications.

**Diagnostics covered:**
- Task Manager resource utilization analysis.
- Resource Monitor process-level disk activity investigation.
- Event Viewer investigation.
- Reliability Monitor review.
- Evaluation of CPU, memory, and disk-related symptoms.
- Assessment of Microsoft Defender Antivirus activity.

**Preliminary assessment:** The synthetic diagnostic results suggested a possible disk I/O bottleneck. Microsoft Defender Antivirus was identified as a process requiring further investigation, but its activity was not established as the root cause.

**Documentation:**
- [Incident Scenario](scenarios/INC-001-Computer-Performance/Incident-Scenario.md)
- [Troubleshooting Workflow](scenarios/INC-001-Computer-Performance/Troubleshooting-Workflow.md)
- [Diagnostic Results](scenarios/INC-001-Computer-Performance/Diagnostic-Results.md)
- [L1 Escalation Report](scenarios/INC-001-Computer-Performance/L1-Escalation-Report.md)

**Important limitation:** This is a simulated scenario. All diagnostic values and findings are synthetic and do not represent measurements from an actual corporate workstation or the personal laptop.

#### INC-002 - Windows Time Synchronization

**Scenario type:** Practical troubleshooting exercise on a personal Windows 11 laptop.

**Issue:** Event Viewer recorded a Time-Service warning (Event ID 134) indicating a DNS name resolution failure for `time.windows.com`.

**Diagnostics performed:**
- Checked DNS resolution.
- Verified Windows Time Service status.
- Inspected NTP configuration and synchronization status.
- Tested NTP server responses using `w32tm /stripchart`.
- Reviewed the Windows network profile and connectivity.
- Compared TCP port testing with NTP server response testing over UDP.
- Reviewed the Windows time zone configuration.

**Resolution:** Executed `w32tm /resync /rediscover` in an elevated PowerShell session.

**Validation:** Confirmed successful synchronization using `w32tm /query /status`. Final status showed Leap Indicator 0, Stratum 5, and a successful synchronization timestamp.

**Evidence and report:** [INC-002 Evidence](evidence/INC-002-Time-Service/)

**Troubleshooting Demo Video:** [Watch INC-002 Demo](evidence/INC-002-Time-Service/INC-002-Windows-Time-Synchronization-Troubleshooting-Demo.mp4)

**Important limitation:** The initial DNS failure was not conclusively attributed to a specific underlying cause. Successful synchronization was confirmed, but permanent resolution of the original transient issue was not established.

## Project Structure

- `scenarios/` - Practical troubleshooting scenarios.
- `documentation/` - Procedures, troubleshooting guides, and incident workflows.
- `evidence/` - Diagnostic screenshots and test reports.
- `scripts/` - PowerShell diagnostic scripts.

## Scope & Safety

- All exercises are performed in a controlled personal-device learning environment.
- No intentional damage to the operating system.
- No unnecessary changes to system configuration.
- Diagnostic actions are prioritized before remediation.
- Administrative privileges are used only when required.
- No real corporate accounts, systems, or employee data are used.

## Project Status

In Progress

Additional troubleshooting scenarios, practical validations, documentation, and evidence will be added as the lab develops.
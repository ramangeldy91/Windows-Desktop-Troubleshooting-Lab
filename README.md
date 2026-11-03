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
- Device Manager
- Event Viewer
- Windows Settings
- Windows Time Service (W32Time)

## Troubleshooting Scenarios

1. Computer Performance
2. Windows Update
3. Disk Space & Storage
4. Application Troubleshooting
5. User Profile Issues
6. Basic Windows Diagnostics
7. Windows Time Synchronization

## Completed Troubleshooting Cases

### INC-002 — Windows Time Synchronization

**Issue:** Event Viewer recorded a Time-Service warning (Event ID 134) indicating a DNS name resolution failure for `time.windows.com`.

**Diagnostics performed:**
- Checked DNS resolution.
- Verified Windows Time Service status.
- Inspected NTP configuration and synchronization status.
- Tested NTP server responses.
- Reviewed the Windows time zone configuration.

**Resolution:** Executed `w32tm /resync /rediscover` in an elevated PowerShell session.

**Validation:** Confirmed successful synchronization using `w32tm /query /status`. Final status showed Leap Indicator 0, Stratum 5, and a successful synchronization timestamp.

**Evidence and report:** [INC-002 Evidence](evidence/INC-002-Time-Service/)

**Troubleshooting Demo Video:** [Watch INC-002 Demo](evidence/INC-002-Time-Service/INC-002-Windows-Time-Synchronization-Troubleshooting-Demo.mp4)

This was a practical troubleshooting exercise on a personal laptop. The initial DNS failure was not conclusively attributed to a specific underlying cause.

## Project Structure

- `scenarios/` - Practical troubleshooting scenarios.
- `documentation/` - Procedures and troubleshooting guides.
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
# INC-002 - Windows Time Synchronization Troubleshooting

## 1. Incident Overview

**Incident ID:** INC-002  
**Category:** Windows Time Service / NTP Synchronization  
**Environment:** Personal Windows 11 Pro Education laptop  
**Date:** November 3, 2026  
**Status:** Resolved  
**Tools:** PowerShell, Event Viewer, Windows Time Service (`W32Time`)

This case documents a hands-on troubleshooting exercise performed on a personal Windows 11 laptop in a learning lab environment.

## 2. Initial Symptoms

An Event Viewer warning was identified under:

**Windows Logs > System**

- **Source:** Time-Service
- **Event ID:** 134
- **Description:** The NTP client failed to resolve the DNS name `time.windows.com`.
- **Error:** Host not found (`0x80072AF9`).

The event indicated a possible DNS resolution issue affecting the Windows Time Service.

## 3. Diagnostic Process

The following checks were performed:

| Check | Command / Tool | Result |
|---|---|---|
| DNS resolution | `nslookup time.windows.com` | DNS resolution successful during testing |
| Service status | `Get-Service -Name W32Time` | Running |
| NTP status | `w32tm /query /status` | Initially reported unsynchronized |
| NTP peers | `w32tm /query /peers` | Configured peer found |
| NTP connectivity | `w32tm /stripchart /computer:time.windows.com /samples:5 /dataonly` | Five NTP responses received |
| NTP configuration | `w32tm /query /configuration` | NTP client enabled; configured source: `time.windows.com,0x9` |
| Network profile | `Get-NetConnectionProfile` | Personal mobile hotspot; IPv4 connectivity available |
| Time zone | `Get-TimeZone` | Georgian Standard Time (UTC+04:00) |

## 4. Root Cause Assessment

Event ID 134 reported a DNS name resolution failure for `time.windows.com`.

Subsequent testing confirmed that DNS resolution and NTP server responses were working.

A transient DNS resolution failure is the most likely explanation for the original warning. However, the available evidence does not establish the exact underlying cause with certainty.

## 5. Resolution

After confirming the configured NTP source and checking the local time settings, the following command was executed in an elevated PowerShell session:

`w32tm /resync /rediscover`

The command completed successfully.

## 6. Post-Resolution Validation

The Windows Time Service status was checked again using:

`w32tm /query /status`

The final output confirmed:

| Parameter | Final Result |
|---|---|
| Leap Indicator | 0 - No warning |
| Stratum | 5 |
| Last Successful Sync | November 3, 2026, 16:11:38 |
| Time Source | `time.windows.com,0x9` |
| Source IP | `51.145.123.29` |

The final status confirmed successful time synchronization.

## 7. Evidence References

- `01-Before-Synchronization.png` - Initial unsynchronized Windows Time Service status.
- `02-After-Synchronization.png` - Successful synchronization status after remediation.
- `INC-002-Windows-Time-Synchronization-Troubleshooting-Demo.mp4` - Video demonstration of the troubleshooting workflow, diagnostic checks, and synchronization recovery.

## 8. Limitations

- This was a personal-device learning exercise, not a corporate production incident.
- No domain controller or enterprise time infrastructure was involved.
- No permanent DNS or NTP configuration changes were made.
- The exact underlying cause of the initial DNS failure was not conclusively established.

## 9. Lessons Learned

- Event Viewer helps identify Windows service warnings and errors.
- DNS resolution and NTP connectivity should be checked independently.
- A running Windows Time Service does not necessarily mean that the system is synchronized.
- `w32tm /query /status` provides evidence of the actual synchronization state.
- Post-remediation validation is essential before marking a troubleshooting case as resolved.
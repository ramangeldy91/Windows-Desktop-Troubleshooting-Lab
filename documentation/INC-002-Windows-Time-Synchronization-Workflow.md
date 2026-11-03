# INC-002 - Windows Time Synchronization Troubleshooting Workflow



## 1. Incident Overview



**Incident ID:** INC-002

**Category:** Windows System Services / Time Synchronization

**Environment:** Personal Windows 11 Pro Education laptop

**Operating System Architecture:** 64-bit

**Build:** 26300

**Diagnostic Date:** November 3, 2026

**Status:** Synchronization successfully restored



### Observed Issue



During a review of Windows System logs, a Time-Service warning (Event ID 134) was identified.



The event reported that the Windows Time client could not resolve the manually configured time server:



`time.windows.com,0x9`



The reported error was:



`The host was unknown. (0x80072AF9)`



The Windows Time client indicated that it would retry the operation.



**User impact:** The Windows clock displayed the correct local time. No noticeable user-facing time discrepancy was identified during the investigation.



**Scope:** This was a controlled troubleshooting exercise on a personal laptop, not a corporate endpoint or production incident.



---



## 2. Initial Assessment



The initial objective was to determine whether the warning represented:



- A temporary DNS resolution failure.

- A network connectivity issue.

- A Windows Time service problem.

- An incorrect time synchronization configuration.



No permanent system configuration changes were planned.



The investigation followed a least-change approach: collect evidence first, identify the likely cause, and apply a limited corrective action only after reviewing the diagnostic results.



---



## 3. Troubleshooting Workflow



### Step 1 - Review Windows Event Logs



**Tool:** Event Viewer



**Navigation:**



`Event Viewer > Windows Logs > System`



The log was filtered to display Warning, Error, and Critical events.



**Finding:**



- Source: Time-Service

- Event ID: 134

- Date: November 3, 2026

- Error: DNS name resolution failure for `time.windows.com,0x9`



**Decision:**



Investigate DNS resolution and Windows Time synchronization before changing service or network configuration.



### Step 2 - Verify DNS Resolution



**Command:**



```powershell

nslookup time.windows.com

```



**Observed result:**



The hostname resolved successfully to:



`51.145.123.29`



The DNS response also returned the alias:



`twc.trafficmanager.net`



**Interpretation:**



DNS resolution was working at the time of this check.



The DNS server appeared as `Unknown` because its hostname was not resolved through a reverse DNS lookup. This did not indicate that the DNS query had failed.



**Decision:**



The original DNS failure was not continuously reproducible. A temporary DNS resolution problem remained a plausible explanation.



### Step 3 - Check Windows Time Service



**Command:**



```powershell

Get-Service -Name W32Time

```



**Observed result:**



The Windows Time service was running.



**Decision:**



The service was operational. Restarting it was not necessary at this stage.



### Step 4 - Check Synchronization Status



**Command:**



```powershell

w32tm /query /status

```



**Initial findings:**



- Leap Indicator: 3 - Not synchronized

- Stratum: 0 - No synchronized source currently established

- Source: `time.windows.com,0x9`

- Last successful synchronization: 11:54:31



**Interpretation:**



Although a previous successful synchronization was recorded, the current status showed that Windows Time was not synchronized at the time of this check.



**Decision:**



Continue investigating the NTP peer and network response.



### Step 5 - Check Configured NTP Peer



**Command:**



```powershell

w32tm /query /peers

```



**Observed result:**



- Configured peer: `time.windows.com,0x9`

- Peer state: Pending



**Interpretation:**



The configured peer was present, but the output did not confirm a successful synchronization.



**Decision:**



Test whether the NTP server responded to direct NTP queries.



### Step 6 - Test NTP Server Response



**Command:**



```powershell

w32tm /stripchart /computer:time.windows.com /samples:5 /dataonly

```



**Observed result:**



The command received five NTP responses from:



`51.145.123.29:123`



The reported time offsets were approximately between -34 ms and +22 ms.



**Interpretation:**



The server responded to the NTP queries. This demonstrated that NTP communication was possible during the test.



It did not, by itself, prove that the Windows Time service had accepted a synchronization sample.



### Step 7 — Verify Windows Time Configuration



**Command:**



```powershell

w32tm /query /configuration

```



**Relevant findings:**



- NtpClient: Enabled

- Type: NTP

- NtpServer: `time.windows.com,0x9`

- NtpServer service provider: Disabled



**Interpretation:**



The NTP client was enabled and configured to use the expected time source.



The disabled NtpServer provider was consistent with a client workstation that was not configured to provide time synchronization to other computers.



No incorrect configuration was identified from these results.



### Step 8 - Check Network Connectivity



**Command:**



```powershell

Get-NetConnectionProfile

```



**Observed result:**



- Network category: Public

- IPv4 connectivity: Internet

- Interface: Wireless network



The connection was using a personal mobile hotspot.



A supplementary connectivity test was performed:



```powershell

Test-NetConnection time.windows.com -Port 123 -InformationLevel Detailed

```



**Observed result:**



- DNS resolution: Successful

- Ping: Successful

- TCP connection to port 123: Failed



**Important interpretation:**



This test checks TCP connectivity. NTP normally uses UDP port 123, so a failed TCP test does not establish that NTP traffic is blocked.



The successful NTP responses from the earlier `w32tm /stripchart` test provided more relevant evidence of NTP communication.



---



## 4. Root Cause Assessment



The original Event ID 134 recorded a DNS name resolution failure for the configured NTP server.



Subsequent tests showed that:



- DNS resolution was working.

- The Windows Time service was running.

- The NTP client configuration was present.

- The NTP server responded to direct queries.

- No persistent configuration problem was identified.



**Most likely explanation:** A temporary DNS resolution failure occurred when the original event was logged.



**Root cause certainty:** Not confirmed. The available evidence does not establish exactly why the initial DNS lookup failed.



---



## 5. Resolution



After reviewing the diagnostic results, a controlled synchronization attempt was performed.



**Command:**



```powershell

w32tm /resync /rediscover

```



**Observed result:**



`The command completed successfully.`



The command requested synchronization and rediscovery of the configured time source.



No permanent DNS, firewall, NTP server, or registry configuration changes were made.



---



## 6. Post-Resolution Validation



**Command:**



```powershell

w32tm /query /status

```



**Final results:**



| Parameter | Result |

|---|---|

| Leap Indicator | 0 — No warning |

| Stratum | 5 — Synchronized |

| Reference ID | 51.145.123.29 |

| Last successful synchronization | 16:11:38 |

| Source | time.windows.com,0x9 |



**Validation conclusion:**



Windows Time synchronization was successfully restored.



The final status confirmed that the computer was synchronized with the configured NTP source.



No further corrective action was required.



---



## 7. Escalation Criteria



A Help Desk L1 technician should consider escalation when:



- Synchronization continues to fail after basic DNS and network checks.

- The configured NTP source cannot be resolved or reached.

- The Windows Time service repeatedly stops or fails to start.

- The workstation is domain-joined and domain time hierarchy issues are suspected.

- Group Policy or organizational security controls prevent synchronization.

- Time discrepancies affect authentication, Kerberos, certificates, scheduled tasks, or business applications.

- Administrative changes are required beyond the technician's authorization.



**Before escalation, collect:**



- Relevant Event Viewer entries and event IDs.

- Output of `w32tm /query /status`.

- Output of `w32tm /query /peers`.

- Output of `w32tm /query /configuration`.

- DNS and network diagnostic results.

- Actions already performed and their outcomes.



Do not modify domain time hierarchy, firewall rules, or organizational policies without authorization.



---



## 8. Evidence References



Evidence is stored in:



`evidence/INC-002-Time-Service/`



- `01-Before-Synchronization.png` - Initial synchronization status.

- `02-After-Synchronization.png` - Successful synchronization status.

- `Evidence-Report.md` - Diagnostic findings and execution evidence.

- `INC-002-Windows-Time-Synchronization-Troubleshooting-Demo.mp4` - Video demonstration of the troubleshooting workflow and synchronization recovery.


---



## 9. Lessons Learned



1. An Event Viewer warning should be investigated rather than treated as proof of an ongoing failure.

2. DNS resolution should be checked before changing NTP configuration.

3. A running service does not necessarily mean that synchronization is successful.

4. `w32tm /stripchart` helps verify NTP server responses, but does not replace validation of Windows Time service status.

5. TCP port testing must not be confused with UDP-based NTP communication.

6. A corrective action should be followed by an independent post-resolution check.

7. Troubleshooting should prioritize evidence collection and minimal system changes.



---



## 10. Scope and Limitations



- This was a learning exercise performed on a personal Windows 11 laptop.

- No corporate domain controller or production identity infrastructure was involved.

- No permanent system configuration changes were made.

- The initial DNS failure could not be reproduced after the event.

- The exact root cause remains unconfirmed.

- The results demonstrate a successful synchronization recovery, not proof of a permanent network or DNS fix.



**Final Case Status:** Resolved - Windows Time synchronization successfully restored and validated.



**Project Status:** In Progress - Additional troubleshooting scenarios, practical validations, documentation, and evidence will be added as the lab develops.

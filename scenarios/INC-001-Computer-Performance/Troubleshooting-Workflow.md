\# INC-001 - Computer Performance Troubleshooting Workflow



\## 1. Purpose



This document describes a structured Help Desk L1 approach to investigating a simulated Windows 11 computer performance incident.



The objective is to identify potential causes of slow application launches and intermittent system freezes through systematic information gathering, resource monitoring, and evidence-based troubleshooting.



\## 2. Incident Reference



\*\*Incident ID:\*\* INC-001

\*\*Category:\*\* Windows Performance

\*\*Priority:\*\* Medium (provisional)

\*\*Affected Users:\*\* 1

\*\*Environment:\*\* Simulated Windows 11 workstation

\*\*Incident Type:\*\* Simulated troubleshooting exercise



\## 3. Step 1 - Initial User Assessment



Before making any system changes, the technician should collect relevant information from the user.



\### Questions to Ask



1\. When did the performance issue first appear?

2\. Is the computer consistently slow or does the problem occur intermittently?

3\. Which applications are affected?

4\. Does the entire system freeze, or do individual applications stop responding?

5\. Were Windows updates, software installations, or configuration changes performed recently?

6\. Are other employees experiencing similar issues?

7\. Does the issue occur immediately after startup or after the computer has been running for some time?



\### Information Already Available in This Scenario



\- The issue started in the morning.

\- Multiple applications are affected.

\- The system occasionally freezes for several seconds.

\- Windows updates were installed the previous day.

\- Only one employee has reported the issue.



\## 4. Step 2 - Initial System Assessment



The technician should inspect the workstation before attempting remediation.



\### Task Manager



Open Task Manager using `Ctrl + Shift + Esc`.



Review the following resource indicators:



\- \*\*CPU:\*\* Check for sustained high processor utilization.

\- \*\*Memory:\*\* Check memory utilization and available memory.

\- \*\*Disk:\*\* Review active time and identify sustained disk activity.

\- \*\*Processes:\*\* Identify applications or background processes consuming significant resources.



\### Initial Interpretation



\- High CPU utilization may indicate a resource-intensive application or background process.

\- High memory utilization may indicate memory pressure, particularly when available memory is low.

\- Sustained high disk active time may indicate storage bottlenecks or intensive disk I/O.

\- A single high reading is not sufficient to establish a root cause. Observe resource utilization over time and correlate it with the reported symptoms.



\## 5. Step 3 - Troubleshooting Hypotheses



Based on the initial complaint, the technician should consider several possible causes.



| Hypothesis | What to Investigate |

|---|---|

| High CPU utilization | Processes consuming excessive CPU resources |

| Memory pressure | Available memory, memory usage, and possible paging |

| High disk activity | Disk active time, response time, and processes generating disk I/O |

| Background processes | Windows Update, Microsoft Defender, indexing, and other scheduled tasks |

| Application-related issues | Application crashes, errors, or excessive resource consumption |

| Recent system changes | Windows updates, driver updates, and recently installed software |



These are investigation hypotheses, not confirmed causes.



\## 6. Troubleshooting Principles



\- Follow a structured diagnostic sequence.

\- Collect evidence before making changes.

\- Distinguish temporary resource spikes from sustained performance problems.

\- Avoid terminating critical Windows or security processes without an approved reason.

\- Do not modify system configuration unless the evidence supports the change.

\- Document findings and explain the reasoning behind each troubleshooting action.

\- Escalate issues that exceed Help Desk L1 permissions or technical scope.


## 7. Step 4 - Resource Monitor Investigation



If Task Manager indicates sustained resource utilization or the reported symptoms remain unexplained, use Resource Monitor for more detailed analysis.



Open Resource Monitor by running `resmon`.



\### CPU



\- Identify processes with sustained CPU consumption.

\- Check whether high utilization coincides with reported freezes.

\- Distinguish short-lived activity from persistent resource consumption.



\### Memory



\- Review memory usage and available memory.

\- Inspect hard faults per second when investigating possible memory pressure.

\- Identify processes with unusually high memory consumption.



Note: Hard faults are not necessarily hardware faults. They indicate that Windows must retrieve memory pages from storage.



\### Disk



\- Review processes generating disk activity.

\- Inspect read and write throughput.

\- Review disk response time and activity where available.

\- Determine whether high disk activity coincides with system freezes.



\### Interpretation



Resource Monitor provides additional context about which processes are generating resource activity.



A process generating disk I/O is not automatically responsible for a performance incident. Its activity must be evaluated in relation to overall disk utilization, response time, and the user's reported symptoms.



\## 8. Step 5 - Event Viewer Investigation



Use Event Viewer to identify system or application errors that may be related to the reported performance degradation.



Open Event Viewer by running `eventvwr.msc`.



\### Windows Logs - System



Review events around the reported time of the performance issue.



Pay attention to:



\- Disk-related warnings or errors.

\- Storage controller events.

\- Unexpected service failures.

\- Relevant Windows system errors.



\### Windows Logs - Application



Review application errors and warnings around the same period.



Look for:



\- Repeated application crashes.

\- Application hangs.

\- Recurring errors involving affected applications.



\### Investigation Principles



\- Focus on events that match the incident timeline.

\- Look for recurring patterns rather than isolated warnings.

\- Record Event IDs, timestamps, sources, and descriptions.

\- Correlate events with other diagnostic findings.



An event occurring at the same time as a performance issue does not automatically prove causation.



\## 9. Step 6 - Reliability Monitor



Use Reliability Monitor to review the workstation's stability history.



Open it by running:



`perfmon /rel`



Review the timeline for:



\- Application failures.

\- Windows failures.

\- Hardware-related errors.

\- Recent software or driver installations.

\- Windows updates.



Determine whether any recorded events coincide with the reported start of the performance issue.



Reliability Monitor provides historical context and should be used together with Task Manager, Resource Monitor, and Event Viewer.



\## 10. Step 7 - Diagnostic Interpretation



After collecting diagnostic information, compare the findings against the initial troubleshooting hypotheses.



The technician should determine:



1\. Which system resource, if any, shows sustained abnormal utilization?

2\. Which process or service is associated with the observed activity?

3\. Do the diagnostic findings coincide with the reported symptoms?

4\. Are there relevant recurring system or application errors?

5\. Is there sufficient evidence to identify a likely cause?



Do not identify a root cause based on a single metric, isolated event, or temporary resource spike.



If the evidence is inconclusive, document the uncertainty and continue investigation or escalate the incident.



\## 11. Step 8 - Recommended Remediation



Remediation must be based on the diagnostic findings.



The technician should select the least disruptive action that addresses the identified issue.



\### Possible L1 Actions



Depending on the confirmed findings and applicable support procedures:



\- Close unnecessary user applications after confirming that the user does not need them.

\- Restart an unresponsive application.

\- Perform a controlled workstation restart when appropriate.

\- Allow Windows Update or Microsoft Defender activities to complete.

\- Check available disk space if storage capacity is suspected to contribute to the issue.

\- Install approved updates according to organizational procedures.



\### Microsoft Defender Considerations



If Microsoft Defender is identified as generating significant disk activity:



\- Confirm that the activity coincides with the reported performance symptoms.

\- Check whether a scan is running.

\- Avoid terminating the Defender process or disabling real-time protection.

\- Consider scheduling scans outside business hours only when permitted by organizational policy.



Do not apply remediation solely because a process appears in the resource monitoring results.



\## 12. Step 9 - Post-Remediation Validation



After applying an approved remediation, verify whether the original symptoms have improved.



\### Validation Checklist



\- Confirm that affected applications open within an acceptable time.

\- Check whether intermittent system freezes continue.

\- Review CPU, memory, and disk utilization.

\- Confirm that no new recurring errors have appeared.

\- Ask the user to confirm whether normal performance has returned.



\### Validation Criteria



The incident can be considered resolved when:



\- The reported performance symptoms are no longer reproducible.

\- System resource utilization is within an acceptable range for the workload.

\- Affected applications operate normally.

\- The user confirms that the issue has been resolved.



If the symptoms persist, continue investigation or escalate the incident.



\## 13. Step 10 - Escalation Criteria



Escalate the incident to the appropriate support team when:



\- Sustained high disk activity or unusually high response times remain unexplained.

\- Disk-related errors suggest a possible storage device or controller problem.

\- Hardware diagnostics indicate a potential hardware failure.

\- Performance degradation persists after approved L1 troubleshooting.

\- System-level configuration changes or administrative permissions beyond L1 scope are required.

\- The issue affects additional users or appears to be part of a wider service problem.



\### Information to Include in an Escalation



Provide:



\- Incident summary and user-reported symptoms.

\- Affected device and operating system details.

\- Troubleshooting steps already performed.

\- Relevant diagnostic findings and timestamps.

\- Error messages and Event IDs, where applicable.

\- Actions taken and their results.

\- Remaining concerns and the reason for escalation.



\## 14. Step 11 - Documentation and Closure



Document the investigation in the incident record.



The final record should include:



\- Initial complaint and business impact.

\- Diagnostic steps and observations.

\- Confirmed findings and unresolved hypotheses.

\- Remediation performed, if any.

\- Post-remediation validation results.

\- User confirmation or escalation details.



Never report an issue as resolved without appropriate validation.



\## 15. Scope and Limitations



This workflow is designed for a simulated Help Desk L1 troubleshooting scenario.



All incident-specific diagnostic values and findings are synthetic unless explicitly identified otherwise.



The workflow demonstrates a structured troubleshooting approach. It does not establish that a real workstation experienced the symptoms described in INC-001 or that any particular process caused them.



Actions involving enterprise security policies, hardware replacement, advanced system configuration, or privileged administration may require escalation to an authorized support team.

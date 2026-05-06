# SOC L1 Alert Reporting

## Overview

This note summarizes three essential SOC L1 skills:

- **Alert reporting**
- **Escalation**
- **Communication**

Alert triage does not end with choosing **True Positive** or **False Positive**.

A SOC L1 Analyst must also document the investigation clearly, escalate when needed, and communicate with the right people or teams.

Good reporting helps L2 analysts continue the investigation quickly.

Good escalation ensures threats are handled in time.

Good communication makes coordination between SOC, IT, HR, management, and other teams clear and effective.

---

## Why Alert Reporting Matters

Alert reporting is important because it preserves the context of the investigation.

A report explains:

- What happened
- Who was involved
- When it happened
- Where it happened
- Why the verdict was chosen
- What evidence supports the conclusion
- Whether escalation is needed

A SOC L1 Analyst should not only investigate alerts but also document findings in a way that another analyst can understand later.

---

## L1 to L2 Alert Flow

A typical alert flow looks like this:

1. An alert arrives in SIEM, EDR, or a ticketing platform.
2. L1 Analyst reviews and triages the alert.
3. Most alerts are closed as **False Positive** or handled at L1 level.
4. Complex or threatening alerts are escalated to L2.
5. L2 performs deeper investigation and remediation.
6. Major incidents may be escalated further to DFIR or Incident Response teams.

The goal is to protect corporate data and reduce business impact.

---

## Reporting, Escalation, and Communication

### Reporting

**Reporting** means writing a clear investigation summary with evidence and reasoning.

The report should help L2 understand the case without starting from zero.

### Escalation

**Escalation** means sending the alert to a more senior analyst or another responsible team when deeper investigation, remediation, or decision-making is required.

### Communication

**Communication** means sharing the right information with the right person or team in a clear and professional way.

This may include communication with:

- L2 analysts
- L3 analysts
- SOC manager
- IT team
- HR team
- System owner
- DFIR team
- Legal or PR teams during major incidents

---

## Purpose of Alert Reports

Alert reports serve several purposes.

### Provide Context for Escalation

A well-written report saves time for L2 analysts.

It helps them quickly understand:

- What happened
- What was reviewed
- What evidence was found
- Why the alert was classified as True Positive or False Positive
- Why escalation is needed

### Save Findings for Records

Raw SIEM logs may be stored for a limited time.

Alert records or case records may be kept longer.

This means important context should be written inside the alert or ticket.

### Improve Investigation Skills

Report writing improves analyst thinking.

If an analyst cannot explain an alert clearly, they may not fully understand it.

Clear writing forces the analyst to organize evidence, timeline, and reasoning.

---

## Alert Report Checklist

A good alert report should include:

- All 5Ws
- Clear and precise language
- A single timeline
- Evidence and indicators
- Verdict reasoning
- Escalation reason if needed

---

## 5Ws Report Format

A SOC alert report should answer the 5Ws.

### Who

Which user, account, host, or entity was involved?

Example:

User S.Conway, HR Coordinator.

### What

What exact action or event sequence happened?

Example:

The user accessed a phishing website and downloaded a double-extension executable file.

### When

When did the suspicious activity happen?

Example:

At 13:56 UTC, the user accessed the website. At 13:58 UTC, the file was downloaded.

### Where

Which device, IP address, file path, domain, URL, or system was involved?

Example:

Host LPT-HR-009, domain freecatvideoshd[.]monster, file cats2025.mp4.exe.

### Why

Why was the alert classified as True Positive or False Positive?

Example:

The file was confirmed as LummaStealer malware and may be used for data theft or C2 communication.

The **Why** is the most important part because it explains the reasoning behind the verdict.

---

## Example Alert Report

At 13:56 UTC, user S.Conway accessed the phishing website freecatvideoshd[.]monster from the Windows laptop LPT-HR-009. At 13:58 UTC, the user downloaded a file named cats2025.mp4.exe, likely mistaking it for a legitimate video.

VirusTotal results confirmed that the file is LummaStealer malware, which is associated with sensitive data theft and possible command-and-control activity.

The alert was classified as **True Positive** due to the suspicious file type, malicious reputation, and potential endpoint compromise.

Escalating to L2 for deeper endpoint investigation and possible remediation.

---

## Report Writing Template

Use this structure when writing alert comments or case reports:

**At [time], user [user] performed [action] on [host/system]. The activity involved [IP/domain/file/process/hash]. Based on [evidence], the alert was classified as [verdict]. Because [reason/impact], the alert was escalated to [team/person] for [next action].**

Example:

At 13:58 UTC, user S.Conway downloaded a double-extension executable file named cats2025.mp4.exe on host LPT-HR-009. The file was downloaded from freecatvideoshd[.]monster and was identified as LummaStealer malware by VirusTotal. Based on the suspicious file type, malicious reputation, and potential data exfiltration risk, the alert was classified as True Positive and escalated to L2 for deeper endpoint investigation.

---

## When to Escalate an Alert

A SOC L1 Analyst should escalate an alert when:

- The alert indicates a major cyberattack
- The alert requires deeper investigation
- The alert requires DFIR or Incident Response
- Remediation is needed
- The analyst does not fully understand the alert
- Communication with other teams is required
- The alert involves high-risk users, systems, or data

---

## Escalation Reasons

Examples of escalation reasons:

- Malware removal is required
- Host isolation is required
- Password reset is required
- Session revocation is required
- Account disablement is required
- Firewall block is required
- EDR containment is required
- Customer or partner communication is required
- Management or legal communication is required
- The alert may indicate ransomware, data theft, or account compromise

---

## Escalation Steps

A typical escalation flow:

1. Move the alert to **In Progress**.
2. Perform initial analysis.
3. Write an alert report.
4. Set the correct verdict.
5. Explain why escalation is needed.
6. Assign the alert to the L2 analyst on shift.
7. Notify L2 through the agreed communication channel.
8. L2 continues the investigation from the report.

---

## Requesting L2 Support

It is acceptable for L1 Analysts to request support when something is unclear.

Especially in the first months, it is better to ask for senior support than to blindly close an alert.

A SOC Analyst should not hide uncertainty.

If unsure, the analyst should:

- Collect available evidence
- Document what was reviewed
- Explain what is unclear
- Ask L2 for support
- Avoid closing the alert without understanding it

Good mindset:

**Do not hide uncertainty. Document it and escalate properly.**

---

## Communication Best Practices

SOC communication should be:

- Clear
- Short
- Evidence-based
- Calm
- Action-oriented
- Sent to the correct person or team

A SOC Analyst should separate facts from assumptions.

Bad communication:

Please check.

Better communication:

Escalating due to confirmed malware reputation and possible endpoint compromise. User S.Conway downloaded cats2025.mp4.exe from a suspicious domain. VirusTotal identifies the file as LummaStealer. No execution evidence was confirmed in the available logs. L2 review is required for endpoint containment and deeper investigation.

---

## Communication With Other Teams

SOC Analysts may need to communicate with other departments.

### IT Team

Example:

Can you confirm whether administrative privileges were intentionally granted to this user?

### HR Team

Example:

Can you confirm whether this newly hired employee is expected to access this system?

### System Owner

Example:

Is this activity expected for this application or service?

### Management

Example:

This alert may indicate a confirmed security incident and requires escalation according to the incident response process.

---

## Crisis Communication Scenarios

Critical cases may require special communication procedures.

### L2 Is Unavailable During a Critical Alert

If L2 is unavailable during an urgent or critical alert, follow the emergency contact procedure.

General order:

1. Try to reach L2.
2. If unavailable, contact L3.
3. If unavailable, contact the SOC Manager.

A critical alert should not be left waiting silently.

### Compromised Chat Account

If a Slack or Teams account may be compromised, do not contact the user through that same chat platform.

Use an alternative verified channel, such as:

- Phone call
- Corporate phone
- Verified email
- Manager confirmation
- HR or IT verified contact

Rule:

**Do not validate compromise through the compromised channel.**

### Alert Flood

If many alerts arrive in a short time, continue prioritising by status, severity, and time.

Also inform the L2 analyst on shift.

An alert flood may indicate:

- A broader attack
- Malware outbreak
- Ransomware activity
- Detection issue
- Misconfigured rule

### Misclassified Alert

If an analyst later realizes that an alert may have been misclassified, they should immediately contact L2.

The analyst should explain the concern, share the original evidence, and request a review.

A mistake should not be hidden.

### Broken SIEM Logs

If SIEM logs are not parsed correctly or are not searchable, do not skip the alert.

The analyst should:

- Investigate what is possible
- Document the limitation
- Notify L2
- Report the issue to the SOC Engineer if needed

Broken logs are also a security visibility issue.

---

## L1, L2, and DFIR Responsibilities

### L1 Analyst

L1 filters noise, performs initial triage, writes reports, and escalates real threats.

### L2 Analyst

L2 performs deeper investigation, validates True Positives, communicates with other teams, and handles remediation.

### DFIR Team

DFIR handles major incidents, forensic investigation, incident response, and cross-team coordination.

---

## Analyst Summary

Alert reporting, escalation, and communication are essential SOC L1 skills.

Alert reporting preserves context and helps L2 continue the investigation quickly.

Escalation ensures real threats receive deeper investigation and remediation in time.

Communication makes coordination between SOC and other departments clear and effective.

A SOC L1 Analyst should write clear reports, escalate real threats with enough evidence, request senior support when uncertain, and communicate through safe and appropriate channels.

---

## Interview Answer

As a SOC L1 Analyst, I should not only triage alerts but also document my findings clearly. After investigating an alert, I would write a report that includes the 5Ws, timeline, affected assets, key indicators, evidence, verdict reasoning, and escalation reason if needed. If the alert indicates a real threat, requires remediation, involves a major incident, or if I am uncertain, I would escalate it to L2 with enough context so they can continue the investigation without starting from zero.

---

## Core Memory Lines

**Reporting** preserves investigation context.

**Escalation** ensures threats are remediated in time.

**Communication** coordinates SOC work with other teams.

**Good reporting saves L2 time.**

**Bad reporting slows incident response.**

**A good report includes the 5Ws.**

**The Why explains the verdict.**

**Escalate when impact is high, action is needed, communication is required, or uncertainty remains.**

**Do not hide uncertainty. Document it and escalate properly.**

**Do not validate compromise through the compromised channel.**

**Broken logs are also a security visibility issue.**

**L1 filters noise and escalates real threats.**

**L2 performs deeper investigation and remediation.**

**DFIR handles major incidents and forensic investigation.**

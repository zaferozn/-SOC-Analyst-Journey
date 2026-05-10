# SOC Workbooks and Lookups

## Overview

This note summarizes how SOC Analysts use **workbooks**, **lookups**, and different types of inventories to enrich alerts and make better triage decisions.

Alert triage is not only about reading the alert name and severity.

A SOC Analyst often needs additional context about:

- The affected user
- The affected host
- The source IP
- The destination system
- The user’s role
- The asset’s purpose
- The network location
- The reputation of IPs, domains, URLs, and hashes

The main idea is simple:

**No context, no reliable verdict.**

A strong SOC triage process follows this logic:

**Alert → Lookup → Context → Workbook → Evidence → Verdict → Report or Escalation**

---

## Why Lookups Matter

An alert alone does not always tell the full story.

Example:

A user downloads a sensitive financial report from a file server and shares it with another user.

This can be normal business activity or a suspicious data theft scenario.

To decide correctly, the analyst needs context.

Important questions:

- Who is the user?
- What is the user’s role?
- What access should the user have?
- What is the affected system?
- What is the system used for?
- Is the activity expected for this user?
- Is the system sensitive or business-critical?
- Is the IP internal, external, VPN-related, or malicious?
- Is the activity normal or suspicious based on the context?

Lookups help analysts answer these questions.

---

## Identity Inventory

**Identity inventory** is a catalogue of corporate users, service accounts, and machine accounts.

It helps analysts understand who the user is and what access they should have.

Identity inventory may include:

- Full name
- Username
- Email address
- Role
- Department
- Location
- Manager
- Access rights
- Privileges
- Contact information
- Service account details
- Normal working context

Example:

User: G.Baker

Identity inventory shows:

- Full name: Gregory Baker
- Role: Chief Financial Officer
- Location: Europe, UK
- Access: VPN, HQ, FINANCE

This context matters because a CFO accessing financial records may be expected.

However, the same activity from an unrelated user may be suspicious.

---

## Identity Inventory Sources

Identity information can come from different sources.

### Active Directory / Entra ID

Active Directory and Entra ID are common identity sources.

They may contain:

- User accounts
- Groups
- Machine accounts
- Domain roles
- Privileged access
- Authentication information

### SSO Providers

SSO means Single Sign-On.

Examples:

- Okta
- Google Workspace

SSO providers can help analysts check:

- User identity
- Login history
- MFA usage
- Application access
- Cloud identity context

### HR Systems

Examples:

- BambooHR
- SAP
- HiBob

HR systems may provide:

- Employee role
- Department
- Manager
- Work location
- Employment status
- Start date
- Termination status

HR systems are useful when the analyst needs real business context about an employee.

### Custom Solutions

Some teams maintain identity information in custom systems such as:

- CSV files
- Excel sheets
- Internal portals
- Security team documents

This is common in smaller or less mature environments.

---

## Asset Inventory

**Asset inventory** is a catalogue of computing resources in the organization.

In this room, asset inventory mainly refers to:

- Servers
- Workstations
- Laptops
- Hosts

Asset inventory helps analysts understand what a system is, who owns it, and why it matters.

Asset inventory may include:

- Hostname
- IP address
- Operating system
- Location
- Owner
- Business purpose
- Device type
- Criticality
- Subnet
- Sensitivity
- Last seen time

Example:

Asset: HQ-FINFS-02

Asset inventory shows:

- Location: UK Datacenter
- IP address: 172.16.15.89
- OS: Windows Server 2022
- Owner: Central IT
- Purpose: File server for financial records

This context matters because HQ-FINFS-02 is not a normal workstation.

It is a file server that stores financial records.

Activity involving this system may require higher attention.

---

## Asset Inventory Sources

Asset information can come from several sources.

### Active Directory / Entra ID

Active Directory may include machine accounts and domain-joined systems.

It can help identify corporate assets and their relationship to users or groups.

### SIEM / EDR

SIEM and EDR tools may collect host information from monitored systems.

They can provide:

- Hostname
- IP address
- Operating system
- Logged-in users
- Agent status
- Last seen time
- Endpoint activity
- Security alerts

### MDM Solutions

MDM means Mobile Device Management.

Examples:

- Microsoft Intune
- Jamf MDM

MDM tools are useful for managing and identifying corporate laptops, mobile devices, and Mac systems.

### Custom Solutions

Some teams maintain asset inventories in custom systems such as:

- CSV files
- Excel sheets
- Internal portals
- CMDB records
- Security team documentation

---

## Identity Inventory vs Asset Inventory

**Identity inventory** tells the analyst who the user is.

**Asset inventory** tells the analyst what the system is.

Example:

Identity context:

- G.Baker is the Chief Financial Officer.
- G.Baker has access to FINANCE systems.

Asset context:

- HQ-FINFS-02 is a file server for financial records.
- The server is owned by Central IT.

Together, these two sources help the analyst decide whether the activity is expected or suspicious.

---

## Network Diagrams

A **network diagram** is a visual map of the organization’s network.

It shows locations, subnets, systems, services, and network connections.

Network diagrams help analysts understand suspicious network activity.

A network diagram can show:

- VPN subnet
- Office subnet
- Database subnet
- DMZ
- Firewalls
- Exposed services
- Internal services
- External-facing systems
- Connections between network segments

---

## What Is a Subnet?

A **subnet** is a smaller part of a larger network.

It is like a separate network area or neighborhood inside the company.

Examples:

- VPN Subnet: 10.10.0.0/16
- Database Subnet: 172.16.15.0/24
- Office Subnet: 172.16.23.0/24

Different subnets often have different purposes and access rules.

---

## VPN Subnet

A **VPN subnet** is the internal IP range assigned to users after they connect to the corporate VPN.

Example:

External attacker IP:

- 103.61.240.174

After successful VPN access, the attacker receives an internal VPN IP:

- 10.10.0.53

This means:

- 103.61.240.174 is the external internet IP.
- 10.10.0.53 is the internal VPN IP assigned after login.

SOC interpretation:

If an external IP brute-forces VPN access and then receives a VPN subnet IP, the attacker may now behave like an internal user.

---

## Network Attack Path Example

Example timeline:

- 08:00: External IP 103.61.240.174 repeatedly connects to corporate VPN on TCP/10443.
- 08:23: The firewall shows that 103.61.240.174 was translated to internal IP 10.10.0.53.
- 08:25: Internal IP 10.10.0.53 scans the Database Subnet 172.16.15.0/24.
- 08:32: The same IP scans the Office Subnet 172.16.23.0/24.

SOC interpretation:

The external IP likely performed VPN brute force.

After successful VPN access, the attacker received an internal VPN subnet IP.

The attacker then started scanning internal subnets to find accessible systems.

This behavior may indicate post-compromise internal reconnaissance.

---

## Network Diagram Use in SOC

A network diagram helps the analyst answer:

- What service is running on this port?
- Is this IP internal or external?
- Which subnet does this IP belong to?
- Is this a VPN IP?
- Is this a database subnet?
- Is this an office subnet?
- Should this source communicate with this destination?
- Is the traffic expected or suspicious?
- Is the attacker moving deeper into the network?

SOC sentence:

**Network diagrams help analysts understand where an IP belongs and how systems are connected.**

---

## Threat Intelligence Lookup

**Threat intelligence lookup** is the process of checking whether an IP, domain, URL, or file hash is known to be suspicious or malicious.

Threat intelligence can help analysts check:

- IP reputation
- Domain reputation
- URL reputation
- File hash reputation
- Known malware family
- Known command-and-control infrastructure
- Phishing indicators
- Malicious campaigns
- Tor, VPN, or proxy usage

Examples of threat intelligence sources:

- VirusTotal
- AbuseIPDB
- URLScan
- Cisco Talos Intelligence
- AlienVault OTX
- MISP
- GreyNoise

SOC sentence:

**Threat intelligence helps analysts check whether an IP, domain, URL, or hash is known to be malicious.**

---

## Indicators and IOCs

An **indicator** is a technical clue related to suspicious or malicious activity.

Examples:

- IP address
- Domain
- URL
- File hash
- Email sender
- Process name
- Command line
- Registry key

An **IOC** means **Indicator of Compromise**.

An IOC is an indicator that is known or strongly suspected to be related to compromise.

Examples:

- Known malware hash
- Known command-and-control IP
- Phishing domain
- Ransomware file path
- Malicious URL

Not every indicator is an IOC.

An indicator becomes more meaningful when it is confirmed as suspicious or malicious.

---

## Enrichment

**Enrichment** means adding context to an alert.

Example:

Alert:

- Login from IP address 103.61.240.174

Enrichment:

- The user normally works in the UK.
- The login IP is from another country.
- The IP has suspicious reputation.
- The login happened outside normal working hours.
- The user performed a password reset after login.

Enrichment helps analysts make a stronger verdict.

SOC sentence:

**Enrichment turns raw alert data into investigation context.**

---

## Lookup

A **lookup** is a search in a context source.

Examples:

- User lookup
- Asset lookup
- IP lookup
- Domain lookup
- URL lookup
- Hash lookup
- Login history lookup
- HR lookup
- Network subnet lookup

SOC sentence:

**Lookup means searching a trusted source to understand the context of an alert.**

---

## SOC Workbooks

A **SOC workbook** is a structured investigation guide for a specific alert type.

It may also be called:

- Playbook
- Runbook
- Workflow

A workbook defines the steps required to investigate and remediate specific threats efficiently and consistently.

Workbooks help L1 Analysts avoid mistakes and follow a standard process.

They are often created by senior analysts to support junior analysts.

SOC sentence:

**Workbook = alert investigation checklist.**

---

## Why Workbooks Matter

L1 Analysts are not expected to perfectly triage every possible attack scenario from memory.

Workbooks help by providing:

- A clear investigation order
- Required lookup steps
- Evidence collection steps
- Decision points
- Escalation conditions
- Consistent triage quality
- Fewer missed details

A workbook helps prevent verdicts from being made without enough evidence.

---

## Workbook Stages

A typical workbook may include three stages:

- Enrichment
- Investigation
- Escalation

### Enrichment Stage

The analyst gathers context.

Examples:

- Check user location in HR or identity inventory.
- Check login IP in threat intelligence.
- Check if the IP is anonymous, VPN, proxy, or Tor.
- Check affected asset context.

### Investigation Stage

The analyst verifies the activity using SIEM, EDR, identity logs, network logs, or other evidence.

Examples:

- Review login timeline.
- Review user actions after login.
- Check for MFA reset.
- Check for suspicious downloads.
- Check for suspicious process execution.
- Check for impossible travel.
- Check whether the activity is expected.

### Escalation Stage

The analyst escalates the alert if risk remains, malicious activity is confirmed, or the case requires deeper analysis.

Examples:

- IP is malicious.
- User does not recognize the login.
- MFA reset happened after login.
- Suspicious actions followed the alert.
- Endpoint compromise is suspected.
- Data exfiltration is suspected.
- The analyst is uncertain.

---

## Atypical Login Workbook Example

An atypical login workbook helps analysts investigate suspicious email, web, or VPN logins.

Possible steps:

1. Receive the alert.
2. Use identity inventory or HR system to find the expected user location.
3. Look up the login IP in threat intelligence services.
4. Check whether the IP is anonymized, VPN, proxy, or Tor.
5. If the IP is malicious, escalate.
6. If not, list user actions after the login in SIEM.
7. Check for suspicious actions such as MFA reset.
8. Review the user’s login timeline.
9. Check whether the login location is atypical.
10. Check whether the login time is normal for the user.
11. Contact the user through a safe channel if needed.
12. Make a verdict.
13. Escalate if risk or uncertainty remains.

Key idea:

**Do not make a verdict before collecting enough context.**

---

## Email Analysis Workbook

Email analysis workbooks are used for phishing or suspicious email alerts.

Possible steps:

1. Take ownership of the alert.
2. Use identity inventory to understand the recipients.
3. Investigate the email using an EML analyser.
4. Check SPF, DKIM, DMARC, email content, and sender domain reputation.
5. Check URLs and attachments.
6. Gather triage evidence.
7. If there is an attachment, continue with attachment analysis.
8. Use a sandbox for suspicious binaries or scripts.
9. Write an alert report for L2 if the email is malicious or unclear.
10. Notify employees only if the campaign affects many users or requires broad awareness.

---

## EML Analyser

An **EML analyser** is a tool used to inspect email files.

It helps analysts review:

- Email headers
- Sender address
- Reply-to address
- SPF result
- DKIM result
- DMARC result
- URLs
- Attachments
- Email body
- Suspicious patterns
- Sender domain reputation

---

## SPF, DKIM, and DMARC

**SPF** checks whether the sending IP is allowed to send email for the domain.

**DKIM** checks whether the email was digitally signed by the sending domain and whether it was modified.

**DMARC** defines what should happen if SPF or DKIM fails.

Simple interpretation:

SPF or DKIM failure does not automatically prove phishing.

However, SPF/DKIM failure combined with suspicious content, suspicious links, or malicious attachments increases risk.

---

## Sandbox

A **sandbox** is an isolated environment used to safely analyze suspicious files.

A sandbox may show:

- Process execution
- Network connections
- File creation
- Registry changes
- Dropped files
- Malware behavior
- Command-and-control attempts

SOC sentence:

**Sandbox analysis helps analysts observe file behavior without risking the production environment.**

---

## PowerShell Analysis Workbook

PowerShell analysis workbooks are used when suspicious PowerShell activity is detected.

PowerShell is a legitimate Windows administration tool, but attackers often abuse it.

Attackers may use PowerShell for:

- Downloading malware
- Running encoded commands
- Credential theft
- Reconnaissance
- Persistence
- Command-and-control activity
- Lateral movement

Possible PowerShell workbook steps:

1. Assign the alert to yourself.
2. Use asset inventory to understand the affected machine.
3. Build a process tree.
4. Identify the parent process.
5. Identify the account used to start PowerShell.
6. Review the PowerShell command line.
7. Use threat intelligence to analyze any URL, IP, domain, or hash.
8. Perform static analysis on downloaded executables if needed.
9. Save process tree and login timeline evidence.
10. Decide if the file or PowerShell activity is malicious.
11. If malicious or suspicious, write a report and escalate to L2.
12. If benign but noisy, write a brief comment and notify SOC engineers for rule tuning if needed.

---

## Process Tree

A **process tree** shows parent-child process relationships.

It helps analysts understand how a process was started.

Examples:

Normal-looking chain:

- explorer.exe → powershell.exe

Suspicious chain:

- winword.exe → powershell.exe
- outlook.exe → powershell.exe
- chrome.exe → powershell.exe
- powershell.exe → cmd.exe → rundll32.exe

SOC interpretation:

If a document or browser process starts PowerShell, the activity may be suspicious.

SOC sentence:

**Process tree helps identify suspicious execution chains.**

---

## Static Analysis

**Static analysis** means examining a file without executing it.

Static analysis may include reviewing:

- File name
- File hash
- File type
- Strings
- Imports
- Metadata
- Digital signature
- Packer indicators
- Known malware reputation

SOC sentence:

**Static analysis helps analysts inspect a suspicious file without running it.**

---

## Login Timeline

A **login timeline** shows user login activity over time.

It helps analysts understand whether an action fits the user’s normal behavior.

A login timeline may show:

- Login time
- Source IP
- Location
- Device
- VPN usage
- MFA events
- Failed attempts
- Successful attempts
- Suspicious changes after login

SOC sentence:

**Login timeline helps analysts identify unusual access patterns before or after an alert.**

---

## Network Analysis Workbook

Network analysis workbooks are used for suspicious network activity such as scanning, firewall events, or unusual connections.

Possible steps:

1. Assign the alert to yourself.
2. Use network map and asset inventory to understand IP context.
3. Identify whether the source IP is internal, external, VPN, or known corporate infrastructure.
4. List ports scanned by the IP.
5. Identify services that may run on those ports.
6. Check whether the source is a corporate scanner such as Nessus or a monitoring tool such as Zabbix.
7. Verify whether the scanning pattern is expected.
8. Use EDR to check for related data exfiltration or endpoint alerts.
9. Collect evidence such as attack time range, scanned ports, and IP context.
10. Write an alert report and escalate to L2 if needed.

---

## Common Ports and Services

Common ports that may appear in network analysis:

- 22: SSH
- 80: HTTP
- 443: HTTPS
- 445: SMB
- 3389: RDP
- 3306: MySQL
- 5432: PostgreSQL
- 5900: VNC
- 6379: Redis
- 9200: Elasticsearch

Scanned ports help identify what services the attacker may be looking for.

---

## Nessus and Zabbix

**Nessus** is a vulnerability scanner.

It scans systems for vulnerabilities, misconfigurations, and exposed services.

**Zabbix** is a monitoring tool.

It monitors system health, performance, availability, and infrastructure status.

If the scanning source is Nessus or Zabbix, the activity may be expected.

However, the analyst should still verify:

- Is the source really a corporate tool?
- Is the scan scheduled?
- Is the target subnet expected?
- Is the scan pattern normal?
- Was the activity approved?

---

## Data Exfiltration

**Data exfiltration** means unauthorized transfer of data out of the organization.

Examples:

- Sensitive files uploaded to an external server
- Large outbound transfer
- Database dump
- Cloud storage upload
- Email attachment leak
- Data sent to attacker-controlled infrastructure
- Command-and-control channel used for data theft

SOC sentence:

**Data exfiltration is one of the most serious outcomes of system compromise.**

---

## Safe User Communication

Sometimes an analyst may need to contact a user to confirm activity.

However, user communication must be handled carefully.

Rules:

- Do not contact a user through a potentially compromised channel.
- Use a verified alternative channel when compromise is possible.
- Ask clear and neutral questions.
- Do not reveal unnecessary technical details.
- Document the user’s response.
- Escalate if the user does not recognize the activity.

Example:

If Teams is suspected to be compromised, do not ask the user through Teams.

Use a phone call or another verified channel.

---

## SOC Analyst Thinking Pattern

A strong SOC Analyst should not look at alert data in isolation.

The analyst should combine:

- Alert details
- Identity context
- Asset context
- Network context
- Threat intelligence
- SIEM logs
- EDR evidence
- Workbook steps
- User confirmation when needed

This creates a stronger investigation and a more reliable verdict.

---

## Analyst Summary

SOC workbooks and lookups help analysts investigate alerts consistently and efficiently.

Identity inventory helps analysts understand who the user is.

Asset inventory helps analysts understand what the system is.

Network diagrams help analysts understand where an IP belongs and how systems are connected.

Threat intelligence helps analysts check whether indicators are known malicious.

Workbooks provide structured investigation steps for specific alert types such as email analysis, PowerShell analysis, and network analysis.

A strong SOC L1 Analyst should enrich alerts with context, follow the workbook, collect evidence, make a clear verdict, and escalate when risk or uncertainty remains.

---

## Interview Answer

When triaging an alert, I would not review technical logs in isolation. I would enrich the alert with identity inventory to understand the user, asset inventory to understand the affected system, network diagrams to understand IP and subnet context, and threat intelligence lookups to check whether indicators are known malicious. If a workbook exists, I would follow its steps to ensure the investigation is consistent and evidence-based before making a verdict or escalating the alert.

---

## Core Memory Lines

**No context, no reliable verdict.**

**Inventory** = context source.

**Identity inventory** = who the user is.

**Asset inventory** = what the system is.

**Network diagram** = where the IP or subnet belongs.

**Threat intelligence** = whether an IP, domain, URL, or hash is known malicious.

**Lookup** = searching a trusted source for investigation context.

**Enrichment** = adding context to an alert.

**Workbook** = alert investigation checklist.

**Playbook, runbook, and workflow** are related terms.

**Process tree** = parent-child process relationships.

**Static analysis** = examining a file without executing it.

**Login timeline** = user login activity over time.

**Data exfiltration** = unauthorized transfer of data out of the organization.

**Alert → Lookup → Context → Workbook → Evidence → Verdict → Report or Escalation**

**Do not make a verdict before collecting enough context.**

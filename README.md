# Screenshots Guide - Security Monitoring Lab

Complete catalog of all 29 screenshots documenting the lab setup, configuration, and event analysis.

---

## Screenshot Organization

Screenshots are organized by category:
1. Domain Controller Setup (DC2)
2. Group Policy Configuration
3. Client Workstation Setup (CLIENT11)
4. Event Viewer - Event 4624 (Successful Logon)
5. Event Viewer - Event 4625 (Failed Logon)
6. Event Viewer - Event 4688 (Process Creation)
7. Audit Policy Verification
8. Network Configuration
9. Active Directory Structure

---

## 1. Domain Controller Setup (DC2)

### Screenshot 1: DC2 VM Overview
**File:** uploads/1.png
**Description:** Domain Controller (DC2) running Windows Server 2022 with Group Policy Management Console open
**Key Elements:**
- VM name: DC2
- Operating System: Windows Server 2022
- Group Policy Management Console (GPMC) interface visible
- Domain: VANTAGEHUB.COM
- Status: Active and running

**Significance:** Shows the domain controller is properly configured and ready for policy management

**MITRE Relevance:** T1078 - Valid Accounts (domain-based authentication infrastructure)

---

### Screenshot 2: DC2 Desktop Environment
**File:** uploads/2.png
**Description:** DC2 desktop showing Windows Server 2022 interface
**Key Elements:**
- Windows Server 2022 Standard Evaluation
- License valid for 180 days
- Build: 20348.1547
- System time: 4:07 PM, 1/22/2026

**Significance:** Confirms Windows Server 2022 is properly installed and running

---

### Screenshot 3: DC2 Network Configuration
**File:** uploads/3.png
**Description:** Network adapter configuration on DC2
**Key Elements:**
- NIC 1: NAT adapter (Internet access)
- NIC 2: Host-Only adapter (Internal domain network)
- Static IP: 172.16.0.1/24
- DNS: 127.0.0.1 (local DNS server)

**Significance:** Demonstrates proper network segmentation for domain infrastructure

---

## 2. Group Policy Configuration

### Screenshot 4: Group Policy Management Console
**File:** uploads/4.png
**Description:** GPMC showing domain structure and GPO creation
**Key Elements:**
- Forest: VANTAGEHUB.COM
- Domain: VANTAGEHUB.COM
- Organizational Units visible
- GPO creation menu open

**Significance:** Shows GPO management interface and domain structure

**MITRE Relevance:** T1078 - Valid Accounts (centralized policy enforcement)

---

### Screenshot 5: GPO Linked to OU
**File:** uploads/5.png
**Description:** Group Policy Object linked to Computer - Workstation OU
**Key Elements:**
- GPO Name: Security Monitoring Lab (Audit)
- Linked to: Computer - Workstation OU
- Scope: Computer Configuration
- Status: Applied

**Significance:** Confirms GPO is properly linked to the correct organizational unit

---

### Screenshot 6: Audit Policy Configuration in GPO
**File:** uploads/6.png
**Description:** Advanced Audit Policy Configuration settings in GPO
**Key Elements:**
- Audit Logon: Success + Failure
- Audit Credential Validation: Success + Failure
- Audit Process Creation: Success
- Force subcategory settings: Enabled

**Significance:** Shows all required audit policies are configured in the GPO

**MITRE Relevance:** T1110 - Brute Force (logon auditing), T1059 - Command-Line Interface (process creation)

---

### Screenshot 7: Command-Line Capture Setting
**File:** uploads/7.png
**Description:** Administrative Templates setting for command-line capture in 4688 events
**Key Elements:**
- Setting: Include command line in process creation events
- Status: Enabled
- Path: Computer Configuration → Administrative Templates → System → Audit Process Creation

**Significance:** Ensures full command-line arguments are captured in process creation events

**MITRE Relevance:** T1059 - Command-Line Interface (critical for detecting suspicious commands)

---

## 3. Client Workstation Setup (CLIENT11)

### Screenshot 8: CLIENT11 VM Overview
**File:** uploads/8.png
**Description:** Client workstation (CLIENT11) running Windows 11
**Key Elements:**
- VM name: CLIENT11
- Operating System: Windows 11
- Domain: VANTAGEHUB.COM
- Status: Domain-joined

**Significance:** Shows client workstation is properly configured and domain-joined

---

### Screenshot 9: CLIENT11 Desktop
**File:** uploads/9.png
**Description:** Windows 11 desktop on CLIENT11
**Key Elements:**
- Windows 11 interface
- System time and date visible
- Network connectivity confirmed

**Significance:** Confirms Windows 11 is properly installed and running

---

### Screenshot 10: Domain Membership Verification
**File:** uploads/10.png
**Description:** System Properties showing domain membership
**Key Elements:**
- Computer name: CLIENT11
- Domain: VANTAGEHUB.COM
- Status: Domain-joined
- Verification: Confirmed

**Significance:** Verifies CLIENT11 is properly joined to VANTAGEHUB.COM domain

---

### Screenshot 11: DNS Configuration
**File:** uploads/11.png
**Description:** Network adapter settings showing DNS configuration
**Key Elements:**
- DNS Server: 172.16.0.1 (Domain Controller)
- DHCP: Enabled
- Network: Host-Only (internal domain network)

**Significance:** Confirms DNS is properly configured to point to domain controller

---

## 4. Event Viewer - Event 4624 (Successful Logon)

### Screenshot 12: Event 4624 - Overview
**File:** uploads/12.png
**Description:** Event Viewer showing Event ID 4624 (Successful Logon)
**Key Elements:**
- Event ID: 4624
- Task Category: Logon
- Level: Information
- Computer: CLIENT11.VANTAGEHUB.COM

**Significance:** Demonstrates successful logon event capture

**MITRE Relevance:** T1078 - Valid Accounts

---

### Screenshot 13: Event 4624 - Details
**File:** uploads/13.png
**Description:** Detailed view of Event 4624 with full event information
**Key Elements:**
- Logon Type: 2 (Interactive)
- Account Name: VANTAGEHUB\Administrator
- Workstation Name: CLIENT11
- Source IP: 172.16.0.100
- Logon Time: [Timestamp visible]

**Significance:** Shows complete logon event details for baseline analysis

**MITRE Relevance:** T1078 - Valid Accounts (baseline normal logons)

---

### Screenshot 14: Event 4624 - XML View
**File:** uploads/14.png
**Description:** XML representation of Event 4624
**Key Elements:**
- Event data in XML format
- All fields structured and parseable
- Suitable for SIEM ingestion

**Significance:** Shows event structure for log forwarding and SIEM integration

---

### Screenshot 15: Event 4624 - Multiple Events
**File:** uploads/15.png
**Description:** Event Viewer showing multiple 4624 events
**Key Elements:**
- Multiple successful logon events
- Timestamps visible
- Event count: Multiple entries

**Significance:** Demonstrates event generation and log accumulation

---

## 5. Event Viewer - Event 4625 (Failed Logon)

### Screenshot 16: Event 4625 - Overview
**File:** uploads/16.png
**Description:** Event Viewer showing Event ID 4625 (Failed Logon)
**Key Elements:**
- Event ID: 4625
- Task Category: Logon
- Level: Warning
- Computer: CLIENT11.VANTAGEHUB.COM

**Significance:** Demonstrates failed logon event capture

**MITRE Relevance:** T1110 - Brute Force

---

### Screenshot 17: Event 4625 - Details
**File:** uploads/17.png
**Description:** Detailed view of Event 4625 with failure information
**Key Elements:**
- Failure Reason: User account locked out / Wrong password
- Account Name: [Username attempted]
- Workstation Name: CLIENT11
- Source IP: 172.16.0.100
- Failure Count: 2-3 attempts

**Significance:** Shows complete failed logon event details for brute force detection

**MITRE Relevance:** T1110 - Brute Force (multiple failed attempts)

---

### Screenshot 18: Event 4625 - XML View
**File:** uploads/18.png
**Description:** XML representation of Event 4625
**Key Elements:**
- Event data in XML format
- Failure reason field populated
- Suitable for SIEM ingestion

**Significance:** Shows event structure for log forwarding and SIEM integration

---

### Screenshot 19: Event 4625 - Multiple Events
**File:** uploads/19.png
**Description:** Event Viewer showing multiple 4625 events
**Key Elements:**
- Multiple failed logon events
- Timestamps visible
- Event count: 2-3 entries (brute force simulation)

**Significance:** Demonstrates brute force event generation and detection opportunity

---

## 6. Event Viewer - Event 4688 (Process Creation)

### Screenshot 20: Event 4688 - Notepad
**File:** uploads/20.png
**Description:** Event 4688 showing Notepad process creation
**Key Elements:**
- Event ID: 4688
- Task Category: Process Creation
- Process Name: C:\Program Files\WindowsApps\Microsoft.WindowsNotepad.exe
- Command Line: notepad.exe
- Parent Process: explorer.exe

**Significance:** Demonstrates process creation event capture with command-line arguments

**MITRE Relevance:** T1059 - Command-Line Interface

---

### Screenshot 21: Event 4688 - Calculator
**File:** uploads/21.png
**Description:** Event 4688 showing Calculator process creation
**Key Elements:**
- Event ID: 4688
- Process Name: C:\Windows\System32\calc.exe
- Command Line: calc.exe
- Parent Process: explorer.exe

**Significance:** Shows process creation for system utility

---

### Screenshot 22: Event 4688 - Command Prompt
**File:** uploads/22.png
**Description:** Event 4688 showing Command Prompt process creation
**Key Elements:**
- Event ID: 4688
- Process Name: C:\Windows\System32\cmd.exe
- Command Line: cmd.exe
- Parent Process: explorer.exe

**Significance:** Demonstrates command-line tool execution capture

**MITRE Relevance:** T1059 - Command-Line Interface (cmd.exe execution)

---

### Screenshot 23: Event 4688 - Details
**File:** uploads/23.png
**Description:** Detailed view of Event 4688 with full process information
**Key Elements:**
- New Process Name: [Full path]
- Process ID: [PID]
- Parent Process Name: [Parent]
- Command Line: [Full command]
- User Context: VANTAGEHUB\Administrator

**Significance:** Shows complete process creation event details for threat detection

---

### Screenshot 24: Event 4688 - XML View
**File:** uploads/24.png
**Description:** XML representation of Event 4688
**Key Elements:**
- Event data in XML format
- Command-line field populated
- Parent process information included
- Suitable for SIEM ingestion

**Significance:** Shows event structure for log forwarding and SIEM integration

---

### Screenshot 25: Event 4688 - Multiple Events
**File:** uploads/25.png
**Description:** Event Viewer showing multiple 4688 events
**Key Elements:**
- Multiple process creation events
- Timestamps visible
- Event count: 3+ entries (Notepad, Calculator, Command Prompt)

**Significance:** Demonstrates process creation event generation and tracking

---

## 7. Audit Policy Verification

### Screenshot 26: auditpol /get /category:*
**File:** uploads/26.png
**Description:** Command-line output of auditpol showing all audit policies
**Key Elements:**
- Audit Logon: Success and Failure
- Audit Credential Validation: Success and Failure
- Audit Process Creation: Success
- All policies enabled as configured

**Significance:** Verifies audit policies are properly enforced at system level

**MITRE Relevance:** T1078, T1110, T1059 (all techniques covered)

---

### Screenshot 27: gpresult /r /scope computer
**File:** uploads/27.png
**Description:** Group Policy result showing applied GPOs
**Key Elements:**
- Applied Group Policy Objects:
  - GPO – Security Monitoring Lab (Audit)
  - Default Domain Policy
  - Default Domain Controllers Policy
- Status: Applied successfully

**Significance:** Verifies GPO is properly applied to CLIENT11

---

## 8. Network Configuration

### Screenshot 28: Network Topology Diagram
**File:** uploads/28.png
**Description:** Network architecture diagram showing lab topology
**Key Elements:**
- INTERNET connection
- DC2 with dual NICs (NAT + Host-Only)
- CLIENT11 with Host-Only connection
- IP ranges and DNS configuration
- Virtual network isolation

**Significance:** Shows complete network architecture and segmentation

---

## 9. Active Directory Structure

### Screenshot 29: Active Directory Users and Computers
**File:** uploads/29.png
**Description:** Active Directory Users and Computers showing domain structure
**Key Elements:**
- Domain: VANTAGEHUB.COM
- Organizational Units:
  - Computer - Workstation (contains CLIENT11)
  - Default containers
- User accounts visible
- Computer objects visible

**Significance:** Shows AD structure and OU organization

---

## Event ID Summary

### Event 4624 - Successful Logon
- Screenshots: 12, 13, 14, 15
- MITRE Technique: T1078 - Valid Accounts
- Key Fields: Logon Type, Account Name, Workstation, Source IP
- Detection: Baseline normal logons, detect anomalies

### Event 4625 - Failed Logon
- Screenshots: 16, 17, 18, 19
- MITRE Technique: T1110 - Brute Force
- Key Fields: Failure Reason, Account Name, Workstation, Source IP
- Detection: Alert on 5+ failures in 10 minutes

### Event 4688 - Process Creation
- Screenshots: 20, 21, 22, 23, 24, 25
- MITRE Technique: T1059 - Command-Line Interface
- Key Fields: Process Name, Command Line, Parent Process, User Context
- Detection: Monitor command-line arguments, LOLBins, parent-child relationships

---

## Configuration Summary

### Domain Controller (DC2)
- Screenshots: 1, 2, 3
- Status: Properly configured
- Role: Active Directory Domain Services, DNS Server

### Group Policy
- Screenshots: 4, 5, 6, 7
- Status: Properly configured
- Policies: Audit Logon, Credential Validation, Process Creation, Command-line capture

### Client Workstation (CLIENT11)
- Screenshots: 8, 9, 10, 11
- Status: Properly configured
- Domain: VANTAGEHUB.COM
- DNS: 172.16.0.1

### Verification
- Screenshots: 26, 27
- Status: All policies verified
- Tools: auditpol, gpresult

### Network
- Screenshots: 28
- Status: Properly segmented
- Architecture: Domain-based with network isolation

### Active Directory
- Screenshots: 29
- Status: Properly structured
- OUs: Computer - Workstation (contains CLIENT11)

---

## MITRE ATT&CK Coverage

### T1078 - Valid Accounts
- Evidence: Screenshots 12, 13, 14, 15
- Coverage: FULL
- Detection: Baseline logons, detect anomalies

### T1110 - Brute Force
- Evidence: Screenshots 16, 17, 18, 19
- Coverage: FULL
- Detection: Alert on multiple failed logons

### T1059 - Command-Line Interface
- Evidence: Screenshots 20, 21, 22, 23, 24, 25
- Coverage: FULL
- Detection: Monitor command-line arguments, LOLBins

---

## Best Practices Demonstrated

1. Domain-based audit policy enforcement (Screenshots 4, 5, 6, 7)
2. Centralized policy management via GPO (Screenshots 4, 5)
3. Command-line argument capture (Screenshots 7, 20-25)
4. Event verification and validation (Screenshots 26, 27)
5. Network segmentation (Screenshot 28)
6. Organizational unit structure (Screenshot 29)

---

## Next Steps

1. Implement SIEM ingestion for centralized log collection
2. Create detection rules based on captured events
3. Expand to additional workstations
4. Implement Windows Event Forwarding
5. Develop threat hunting procedures

---

Last Updated: January 2026

Total Screenshots: 29

Lab Status: COMPLETE

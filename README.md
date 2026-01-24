# Security-Monitoring-Lab

A hands-on cybersecurity lab project demonstrating Windows security event monitoring, audit policy configuration, and log analysis using Event Viewer. This lab maps security events to MITRE ATT&CK techniques and provides practical experience with threat detection and incident response.

## Project Overview

This lab provides practical experience with Windows security monitoring by:
- Configuring advanced audit policies on Windows 10/11
- Generating security events through various user actions
- Analyzing and interpreting Windows security logs
- Understanding key security event IDs and their significance
- Mapping security events to MITRE ATT&CK framework techniques
- Building detection strategies for common attack patterns

Time Required: 1-2 hours
Skills Gained: Windows security, event logging, audit policies, log analysis, threat detection, MITRE ATT&CK framework

---

## Learning Objectives

By completing this lab, you will:
- Understand Windows audit policies and their configuration
- Generate and capture security events in Event Viewer
- Interpret critical security event IDs (4624, 4625, 4688)
- Analyze authentication and process creation logs
- Map security events to MITRE ATT&CK techniques
- Develop foundational skills for security monitoring and incident response
- Understand how adversaries operate and how to detect them
- Create detection rules based on observed security events

---

## Prerequisites

Host Machine: Windows

Virtualization Software:
- VirtualBox (free, recommended)

Windows ISO:
- Windows 10/11 evaluation ISO (free from Microsoft)
- Or licensed Windows installation media

System Requirements:
- Minimum 4GB RAM allocated to VM
- 2+ CPU cores
- 30GB disk space for Windows VM

---

## Setup Instructions

### Step 1: Install Virtualization Software

VirtualBox (Recommended):
```
Download from: https://www.virtualbox.org/wiki/Downloads
Follow the installation wizard for your OS
```

### Step 2: Download Windows ISO

1. Visit Microsoft Evaluation Center: https://www.microsoft.com/en-us/evalcenter
2. Download Windows 10 or Windows 11 evaluation ISO
3. Save to a known location on your host machine

### Step 3: Create Virtual Machine

In VirtualBox:
1. Click "New" - Name: "Security-Lab-VM"
2. Type: Microsoft Windows
3. Version: Windows 10 (64-bit) or Windows 11
4. Memory: 4096 MB (4GB)
5. Storage: 30GB dynamic allocation
6. Mount the ISO and complete Windows installation

### Step 4: Install Guest Additions (VirtualBox)

VirtualBox:
```
Devices - Insert Guest Additions CD Image
Run the installer inside the VM
```

---

## Lab Configuration

### Step 1: Open Local Security Policy

1. Press Win + R
2. Type: secpol.msc
3. Press Enter

### Step 2: Configure Audit Policies

Navigate to: Advanced Audit Policy Configuration - Audit Policies

Enable Logon/Logoff Auditing:
```
Path: Audit Policies - Logon/Logoff
Policy: Audit Logon
Settings: Success, Failure
```

Enable Account Logon Auditing:
```
Path: Audit Policies - Account Logon
Policy: Audit Credential Validation
Settings: Success, Failure
```

Enable Detailed Tracking:
```
Path: Audit Policies - Detailed Tracking
Policy: Audit Process Creation
Settings: Success
```

Apply all policies and close Local Security Policy.

### Step 3: Verify Audit Policies with Command Line

Open Command Prompt as Administrator and run:

```
auditpol /get /category:*
```

This verifies that your audit policies are properly configured at the system level.

---

## Lab Execution

### Generate Security Events

Event 1: Successful Logon (4624)
```
1. Log out of the current user session
   - Press Ctrl + Alt + Delete - Sign out
2. Log back in with correct credentials
3. This generates Event ID 4624
```

Event 2: Failed Logon (4625)
```
1. At the login screen, attempt to log in
2. Enter WRONG password 2-3 times
3. Each failed attempt generates Event ID 4625
```

Event 3: Process Creation (4688)
```
1. Open Notepad
   - Press Win + R - type "notepad" - Enter
2. Open Calculator
   - Press Win + R - type "calc" - Enter
3. Open Command Prompt
   - Press Win + R - type "cmd" - Enter
4. Each application launch generates Event ID 4688
```

---

## Log Analysis

### Step 1: Open Event Viewer

1. Press Win + R
2. Type: eventvwr.msc
3. Press Enter

### Step 2: Navigate to Security Logs

```
Event Viewer (Local)
- Windows Logs
  - Security
```

### Step 3: Filter by Event ID

Filter for Event ID 4624 (Successful Logon):
1. Right-click "Security" - Filter Current Log
2. Event IDs: 4624
3. Click OK

Filter for Event ID 4625 (Failed Logon):
1. Right-click "Security" - Filter Current Log
2. Event IDs: 4625
3. Click OK

Filter for Event ID 4688 (Process Creation):
1. Right-click "Security" - Filter Current Log
2. Event IDs: 4688
3. Click OK

---

## Key Event IDs Explained

### Event ID 4624 - Successful Logon

What it means: A user successfully logged in to the system

Key Fields:
- Logon Type: 2 (Interactive), 3 (Network), 10 (RemoteInteractive)
- Account Name: Username that logged in
- Workstation Name: Computer name
- Source IP Address: Where the login came from

Example:
```
Event ID: 4624
Task Category: Logon
Level: Information
Computer: SECURITY-LAB-VM
Description: An account was successfully logged on.
```

MITRE ATT&CK Mapping: T1078 - Valid Accounts
- Adversaries use valid accounts to maintain access
- Monitoring 4624 helps detect unauthorized account usage
- Baseline normal logon patterns to identify anomalies

### Event ID 4625 - Failed Logon

What it means: A login attempt failed (wrong password, account locked, etc.)

Key Fields:
- Failure Reason: Why the login failed
- Account Name: Username attempted
- Workstation Name: Computer name
- Source IP Address: Where the attempt came from

Example:
```
Event ID: 4625
Task Category: Logon
Level: Warning
Computer: SECURITY-LAB-VM
Description: An account failed to log on.
Failure Reason: User account locked out
```

MITRE ATT&CK Mapping: T1110 - Brute Force
- Adversaries attempt multiple login failures to guess credentials
- Multiple 4625 events in short timeframe indicate brute force attack
- Monitoring this event enables early detection of credential attacks
- Threshold alerting (e.g., 5+ failures in 10 minutes) triggers investigation

### Event ID 4688 - Process Creation

What it means: A new process was created on the system

Key Fields:
- New Process Name: Full path to the executable
- Process ID: Unique identifier for the process
- Parent Process Name: Which process launched this one
- Command Line: Arguments passed to the process

Example:
```
Event ID: 4688
Task Category: Process Creation
Level: Information
Computer: SECURITY-LAB-VM
Description: A new process has been created.
New Process Name: C:\Windows\System32\notepad.exe
Process ID: 0x1234
```

MITRE ATT&CK Mapping: T1059 - Command and Scripting Interpreter
- Adversaries execute commands to perform actions on compromised systems
- Monitoring 4688 reveals what processes are running and their command-line arguments
- Suspicious processes (cmd.exe, powershell.exe with encoded commands) indicate compromise
- Parent-child process relationships reveal attack chains

---

## MITRE ATT&CK Framework Integration

### Why MITRE ATT&CK Knowledge is Beneficial

The MITRE ATT&CK framework is a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations. Understanding this framework is critical for security professionals because:

1. Common Language: Provides standardized terminology for discussing threats across organizations and teams

2. Threat Intelligence: Maps real-world adversary behavior to specific techniques, enabling better threat hunting

3. Detection Strategy: Helps identify which events and logs to monitor for specific attack techniques

4. Risk Assessment: Prioritizes security controls based on techniques most relevant to your organization

5. Incident Response: Quickly correlates observed events to known attack patterns for faster response

6. Career Development: Industry-standard framework used by security teams, SOCs, and incident responders

### Event-to-Technique Mapping

This lab demonstrates three critical MITRE ATT&CK techniques:

Technique 1: T1078 - Valid Accounts
- Tactic: Defense Evasion, Persistence, Privilege Escalation, Initial Access
- Event ID: 4624 (Successful Logon)
- Why it matters: Attackers prefer using legitimate credentials over exploits
- Detection: Monitor for logons outside business hours, from unusual locations, or to sensitive accounts
- Real-world example: Compromised credentials used to access cloud services

Technique 2: T1110 - Brute Force
- Tactic: Credential Access
- Event ID: 4625 (Failed Logon)
- Why it matters: Brute force is a common initial attack vector
- Detection: Alert on multiple failed logon attempts (threshold-based)
- Real-world example: Attackers targeting RDP, SSH, or web application login pages

Technique 3: T1059 - Command and Scripting Interpreter
- Tactic: Execution
- Event ID: 4688 (Process Creation)
- Why it matters: Command execution is how attackers perform actions on compromised systems
- Detection: Monitor for suspicious process names, command-line arguments, and parent-child relationships
- Real-world example: PowerShell scripts downloading malware, cmd.exe spawning unusual processes

### Building a Detection Strategy

Using MITRE ATT&CK to improve detection:

1. Identify relevant techniques for your organization
2. Map techniques to observable events (4624, 4625, 4688, etc.)
3. Define baseline behavior (normal logons, expected processes)
4. Create detection rules for anomalies
5. Implement alerting thresholds
6. Correlate multiple events to identify attack chains

Example Detection Rule:
```
IF (Event 4625 count > 5 in 10 minutes) AND (Source IP = External)
THEN Alert: Possible brute force attack on [Account]
```

---

## Best Practices for Security Monitoring

### 1. Baseline Your Environment

Before you can detect anomalies, you need to understand normal behavior:
- Document typical logon times and locations
- Identify expected processes and their command lines
- Record normal user and service account activity
- Create a baseline of system behavior during normal operations

### 2. Implement Threshold-Based Alerting

Don't alert on every event. Use thresholds to reduce noise:
- Alert on 5+ failed logons in 10 minutes (brute force indicator)
- Alert on unusual process creation patterns
- Alert on logons outside business hours to sensitive accounts
- Alert on processes spawning from unexpected parent processes

### 3. Correlate Multiple Events

Single events are less meaningful than patterns:
- Correlate failed logons followed by successful logon (possible compromise)
- Correlate process creation with command-line arguments (malware execution)
- Correlate logons from multiple locations in short timeframe (credential theft)
- Build attack chains by correlating multiple event types

### 4. Monitor Command-Line Arguments

Process names alone are insufficient for detection:
- cmd.exe is normal, but cmd.exe /c "powershell -enc ..." is suspicious
- Legitimate processes may have unusual command-line arguments
- Encoded PowerShell commands are common malware delivery mechanism
- Monitor for LOLBins (Living Off The Land Binaries) with suspicious arguments

### 5. Maintain Audit Logs

Proper log management is critical:
- Ensure sufficient disk space for log retention
- Archive logs regularly for long-term storage
- Protect logs from tampering (immutable storage)
- Implement log forwarding to centralized SIEM
- Retain logs for minimum 90 days (compliance requirement)

### 6. Regular Review and Tuning

Monitoring is not a set-and-forget activity:
- Review detection rules monthly for false positives
- Update rules based on new threat intelligence
- Test detection rules with simulated attacks
- Document all changes to audit policies
- Train team on new detection capabilities

---

## Screenshots and Documentation

Add your lab work screenshots here:

### Screenshot 1: Event ID 4624 - Successful Logon

[Add screenshot of Event Viewer showing Event ID 4624]

Location: screenshots/4624_successful_logon.png

Description: This screenshot shows a successful logon event with logon type, account name, and source information.

---

### Screenshot 2: Event ID 4625 - Failed Logon

[Add screenshot of Event Viewer showing Event ID 4625]

Location: screenshots/4625_failed_logon.png

Description: This screenshot shows failed logon attempts with failure reason and account information.

---

### Screenshot 3: Event ID 4688 - Process Creation

[Add screenshot of Event Viewer showing Event ID 4688]

Location: screenshots/4688_process_creation.png

Description: This screenshot shows process creation events with process name, command line, and parent process information.

---

### Screenshot 4: Local Security Policy Configuration

[Add screenshot of Local Security Policy showing audit policy settings]

Location: screenshots/local_security_policy_config.png

Description: This screenshot shows the configured audit policies in Local Security Policy.

---

### Screenshot 5: Audit Policy Verification

[Add screenshot of auditpol command output]

Location: screenshots/auditpol_verification.png

Description: This screenshot shows the output of auditpol /get /category:* verifying audit policies are enabled.

---

## Lab Report Template

Create a LAB_REPORT.md file documenting your findings:

```markdown
# Security Monitoring Lab Report

## Objective
Configure Windows audit policies and analyze security events using MITRE ATT&CK framework.

## Environment
- Host OS: [Your OS]
- Virtualization: [VirtualBox/VMware]
- Guest OS: [Windows 10/11]
- Lab Date: [Date]

## Events Captured

### 1. Event ID 4624 - Successful Logon
- Time: [Timestamp]
- User: [Username]
- Logon Type: [Type]
- Source: [IP/Workstation]
- MITRE Technique: T1078 - Valid Accounts
- Significance: Indicates successful authentication; baseline for detecting unauthorized access

### 2. Event ID 4625 - Failed Logon
- Time: [Timestamp]
- User: [Username]
- Failure Reason: [Reason]
- Attempts: [Number]
- MITRE Technique: T1110 - Brute Force
- Significance: Security indicator for credential attack attempts

### 3. Event ID 4688 - Process Creation
- Time: [Timestamp]
- Process: [Process Name]
- Parent Process: [Parent]
- Command Line: [Command]
- MITRE Technique: T1059 - Command and Scripting Interpreter
- Significance: Tracks application execution for forensics and malware detection

## Key Findings
- [Your observations about event patterns]
- [Security implications of observed events]
- [How events map to MITRE techniques]
- [Lessons learned about Windows security monitoring]

## Detection Opportunities
- [Potential detection rules based on observed events]
- [Threshold-based alerting strategies]
- [Correlation opportunities between events]

## Conclusion
[Summary of lab experience and understanding of security monitoring]
```

---

## Security Insights

### Why These Events Matter

Event ID | Purpose | Security Value | MITRE Technique
---------|---------|-----------------|----------------
4624 | Track successful logins | Detect unauthorized access patterns | T1078 - Valid Accounts
4625 | Track failed logins | Identify brute force attacks | T1110 - Brute Force
4688 | Track process creation | Detect malware execution, unauthorized tools | T1059 - Command and Scripting Interpreter

### Real-World Applications

Incident Response: Correlate events to identify attack timelines and reconstruct adversary actions

Forensics: Reconstruct user actions and system activity to understand what happened during a breach

Threat Detection: Identify suspicious patterns (failed logins, unusual processes) that indicate compromise

Compliance: Meet audit requirements (HIPAA, PCI-DSS, SOC 2) by maintaining security logs

Threat Hunting: Proactively search for indicators of compromise using MITRE ATT&CK techniques

---

## Troubleshooting

### Audit Policies Not Appearing in Event Viewer

Solution:
1. Ensure policies are enabled in secpol.msc
2. Restart the Event Log service:
   ```powershell
   Restart-Service EventLog
   ```
3. Generate new events after restart

### Event Viewer Shows No Security Logs

Solution:
1. Verify you are in the correct log: Windows Logs - Security
2. Check that audit policies are enabled
3. Ensure you have administrator privileges
4. Generate new events (logon, process creation)

### Cannot Access Local Security Policy

Solution:
1. Run Command Prompt as Administrator
2. Type: secpol.msc
3. If unavailable, you may be on Windows Home Edition (requires Pro/Enterprise)

---

## Additional Resources

### Microsoft Documentation
- Windows Event IDs: https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/audit-events
- Advanced Audit Policy Configuration: https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/advanced-security-audit-policy-settings
- Event Viewer Documentation: https://docs.microsoft.com/en-us/shows/inside/event-viewer

### MITRE ATT&CK Framework
- MITRE ATT&CK Home: https://attack.mitre.org/
- T1078 - Valid Accounts: https://attack.mitre.org/techniques/T1078/
- T1110 - Brute Force: https://attack.mitre.org/techniques/T1110/
- T1059 - Command and Scripting Interpreter: https://attack.mitre.org/techniques/T1059/

### Related Topics
- Windows Security Event Forwarding (WSEF)
- SIEM Integration (Splunk, ELK Stack)
- Log Aggregation and Analysis
- Incident Response Procedures
- Threat Hunting Methodologies

---

## Lab Checklist

- [ ] VirtualBox/VMware installed
- [ ] Windows 10/11 VM created
- [ ] Guest Additions/VMware Tools installed
- [ ] Local Security Policy opened
- [ ] Audit Logon enabled (Success + Failure)
- [ ] Audit Credential Validation enabled (Success + Failure)
- [ ] Audit Process Creation enabled (Success)
- [ ] Logged out and back in (Event 4624)
- [ ] Failed login attempts (Event 4625)
- [ ] Opened Notepad, Calculator, Command Prompt (Event 4688)
- [ ] Event Viewer opened
- [ ] Filtered for Event ID 4624
- [ ] Filtered for Event ID 4625
- [ ] Filtered for Event ID 4688
- [ ] Screenshots captured (all 3 event types)
- [ ] Lab report completed with MITRE mappings
- [ ] Portfolio documentation ready

---

## Next Steps

After completing this lab:

1. Advanced Auditing: Configure additional audit policies (file access, registry changes)

2. MITRE ATT&CK Expansion: Research additional techniques relevant to your organization

3. Log Forwarding: Set up Windows Event Forwarding to a central server

4. SIEM Integration: Forward logs to Splunk or ELK Stack

5. Threat Hunting: Analyze logs for suspicious patterns using MITRE techniques

6. Detection Rules: Create custom detection rules based on MITRE ATT&CK techniques

7. Automation: Create PowerShell scripts to parse and analyze events

---

## License

This lab guide is provided for educational purposes. Use responsibly and only on systems you own or have permission to test.

---

## Author

Created as a hands-on cybersecurity learning project.

Last Updated: January 2026

---

## Tips for Success

- Take detailed notes during the lab for your portfolio
- Screenshot everything - visual evidence is valuable
- Understand the "why" behind each event, not just the "what"
- Experiment safely - try different scenarios to see how events change
- Document your findings - this becomes your portfolio piece
- Research MITRE ATT&CK techniques to understand adversary behavior
- Think about how attackers would exploit each technique
- Consider how to detect each technique in your environment

---

This lab is a great foundation for security monitoring, threat detection, and incident response skills. Understanding both Windows security events and the MITRE ATT&CK framework will significantly enhance your ability to detect and respond to threats.

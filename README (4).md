# ApexPlanet Task 4 — Exploitation & System Security

## Overview
This repository documents a controlled cybersecurity lab assessment performed against a deliberately vulnerable **Metasploitable2** virtual machine from a **Kali Linux** assessment VM.

The work follows the Task 4 brief, which covers:
- Exploitation and vulnerability assessment
- Password/security assessment concepts
- Social-engineering awareness simulation
- Malware basics (static vs dynamic analysis)
- System hardening
- Evidence-backed reporting and mitigations

> **Lab scope:** All assessment activity documented here was performed against the intentionally vulnerable Metasploitable2 lab target (`192.168.56.102`) on a private VirtualBox network.

## Lab Environment

| Component | Role |
|---|---|
| Kali Linux | Assessment / testing VM |
| Metasploitable2 | Intentionally vulnerable target VM |
| Network | VirtualBox private/host-only lab network |
| Target IP | `192.168.56.102` |
| Kali lab IP | `192.168.56.101` |

## 1. Reconnaissance and Scanning

The initial host discovery identified the Metasploitable2 target on the private lab subnet.

The service/version scan identified numerous exposed services, including:
- FTP — vsftpd 2.3.4
- SSH — OpenSSH 4.7p1
- Telnet
- SMTP
- DNS/BIND
- HTTP/Apache
- SMB/Samba
- MySQL
- PostgreSQL
- VNC
- IRC
- Apache Tomcat/AJP

Evidence:
- `01-Metasploitable2-Recon-Scan.png`
- `03-Vulnerability-Scan-Overview.png`
- `03-Vulnerability-Assessment.png`

## 2. Vulnerability Assessment

The assessment identified several services with outdated or intentionally vulnerable versions. The most clearly documented finding in this evidence set is **VSFTPD 2.3.4**.

Metasploit was opened and the VSFTPD 2.3.4 backdoor module was identified. The module information displayed the vulnerability description and references **CVE-2011-2523**.

Evidence:
- `02-Metasploit-Console.png`
- `04-Vulnerability-Finding-1.png`
- `05-Vulnerability-Finding-2.png`
- `06-Vulnerability-Finding-3.png`
- `07-Vulnerability-Finding-4.png`
- `14-Metasploit-Vsftpd-Module.png`
- `15-Vsftpd-Module-Info.png`
- `16-Vsftpd-Exploit-Options.png`

### VSFTPD finding

**Risk:** Critical in an exposed/untrusted environment because a vulnerable FTP service can permit unauthorized command execution.

**Observed evidence:** The target exposes TCP/21 and reports `vsftpd 2.3.4`; the Metasploit module information identifies the historical backdoor and CVE-2011-2523.

**Recommended mitigation:**
1. Remove the backdoored/outdated version.
2. Upgrade to a vendor-supported, verified package.
3. Prefer SFTP/SSH or another secure file-transfer mechanism where appropriate.
4. Restrict FTP access with network controls.
5. Monitor authentication and service logs.

**Evidence boundary:** The screenshots document identification and configuration of the Metasploit module. They do **not** claim successful remote-shell or post-exploitation access.

## 3. SSH Security Assessment

SSH was found open on TCP/22. Algorithm enumeration showed legacy cryptographic options including SHA-1 based key-exchange algorithms and older CBC/legacy cipher and MAC options.

Evidence:
- `08-SSH-Security-Assessment.png`
- `08-SSH-Algorithm-Output.png`

### Recommended mitigation
- Disable obsolete SHA-1/legacy key-exchange options where supported.
- Disable deprecated CBC and legacy ciphers.
- Use current OpenSSH releases and strong modern algorithms.
- Restrict SSH access to trusted management networks.
- Use key-based authentication and appropriate account controls.

## 4. System Security / Hardening Assessment

The lab evidence includes checks for:
- Firewall state
- Listening services
- Patch/upgrade status

Evidence:
- `09-Firewall-Current-Status.png`
- `10-Listening-Services.png`
- `11-Patch-Assessment.png`

### Hardening recommendations
**Firewall**
- Permit only required inbound traffic.
- Restrict management services to the lab/administrative network.
- Deny unnecessary exposed ports.

**Unused services**
- Identify services that are not required.
- Disable/remove unnecessary services after validating dependencies.
- Re-scan after changes to verify the attack surface has been reduced.

**Patching**
- Apply security updates through the normal package-management process.
- Maintain a regular patch cycle.
- Re-scan after patching.

## 5. Social Engineering — Awareness Simulation

A local, clearly labelled training page was created for phishing awareness. It does not collect, store, or transmit credentials.

The page highlights common warning signs:
- Unexpected login requests
- Urgent or threatening language
- Suspicious links/domains
- Requests for passwords or sensitive information

Evidence:
- `13-Phishing-Awareness-Simulation.png`

## 6. Malware Basics

A harmless text sample was created for a controlled static-analysis demonstration.

The sample was identified as ASCII text and its SHA-256 hash was recorded:

`fbe0bc72ac2e53e449c6ef65f1f6c2acc9b3fe16e1758f33d25a8535fec7f6d6`

Evidence:
- `14-Benign-Sample-Static-Analysis.png`
- `15-Benign-Sample-Analysis-Hash.png`

### Static vs Dynamic Analysis
**Static analysis** examines a file without executing it, for example by inspecting its type, strings, metadata, hashes, or other properties.

**Dynamic analysis** observes behavior while a sample executes in an isolated environment. For safety, this project uses a benign non-executable sample and does not execute malware.

## 7. Findings Summary

| Area | Observation | Recommended action |
|---|---|---|
| FTP | vsftpd 2.3.4 exposed | Upgrade/remove vulnerable service |
| SSH | Legacy algorithms exposed | Disable deprecated algorithms; update OpenSSH |
| Attack surface | Many services exposed | Disable unnecessary services |
| Firewall | Current state assessed | Restrict inbound traffic |
| Patching | Patch state assessed | Apply security updates |
| Phishing | Awareness simulation created | Train users to verify links/senders |
| Malware | Benign static-analysis sample | Use isolated sandboxes for real samples |

## 8. Evidence

All screenshots are stored in the `screenshots/` directory.

The evidence is intentionally mapped to the activity it supports; conclusions are not extended beyond what the captured output demonstrates.

## 9. Conclusion

The assessment demonstrates the value of reconnaissance, service/version identification, vulnerability validation, SSH configuration review, system-hardening checks, phishing awareness, and basic malware-analysis concepts in a controlled lab.

The strongest documented technical finding is the intentionally vulnerable **VSFTPD 2.3.4** service. The recommended remediation is to remove or upgrade vulnerable software, reduce exposed services, harden SSH, maintain firewall controls, and keep systems patched.

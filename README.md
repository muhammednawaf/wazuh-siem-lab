# 🛡️ Wazuh SIEM Home Lab — SOC Analyst Portfolio

> A fully functional Security Information and Event Management (SIEM) home lab built with Wazuh on Ubuntu VM, monitoring a Windows 11 endpoint. This project simulates real SOC analyst workflows including log ingestion, attack simulation, alert triage, custom detection rules, File Integrity Monitoring, and professional incident reporting.

![Wazuh](https://img.shields.io/badge/SIEM-Wazuh%204.7.5-blue)
![OS](https://img.shields.io/badge/Server-Ubuntu%2022.04-orange?logo=ubuntu)
![Agent](https://img.shields.io/badge/Agent-Windows%2011-0078D6?logo=windows)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![MITRE](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-red)
![Analyst](https://img.shields.io/badge/Analyst-Nawaf-purple)

---

## 👤 About This Project

| Field | Details |
|---|---|
| **Analyst** | Nawaf |
| **Agent IP** | 192.168.1.35 |
| **Server** | wazuh-VirtualBox (Ubuntu 22.04) |
| **Agent** | Nawaf — Windows 11 Home (Wazuh v4.7.5) |
| **Project Duration** | April 26–30, 2026 |
| **Goal** | Build hands-on SOC analyst portfolio |

---

## 🏗️ Lab Architecture

```
┌─────────────────────────────────────────────────────┐
│                  HOME NETWORK                        │
│                                                      │
│  ┌──────────────────────┐   ┌────────────────────┐  │
│  │   Windows 11 PC      │   │   Ubuntu 22.04 VM  │  │
│  │   Agent: Nawaf       │──▶│   Wazuh SIEM       │  │
│  │   IP: 192.168.1.35   │   │   IP: 192.168.1.38 │  │
│  │   Wazuh Agent v4.7.5 │   │   Manager          │  │
│  └──────────────────────┘   │   Indexer          │  │
│                              │   Dashboard        │  │
│                              └────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

---

## 📸 Lab Setup Proof

### Wazuh Login Page
![Wazuh Login](screenshots/wazuh.png)

### Wazuh Home Dashboard
![Dashboard](screenshots/dashboard.png)

### Windows Agent Connected (Active)
![Agent Connected](screenshots/agent_connected.png)

### Agent Dashboard — Nawaf (192.168.1.35)
![Agent Dashboard](screenshots/agent_dashbaord.png)

---

## 🧪 Attack Simulations & Detections

All attacks simulated in isolated home lab for educational purposes only.

---

### ⚔️ Attack 1 — Brute Force Login
**MITRE: T1110.001 — Brute Force: Password Guessing**

```powershell
$i=0; while($i -lt 10){
  net use \\localhost\IPC$ /user:fakeuser wrongpassword 2>$null
  $i++
}
```

| Field | Details |
|---|---|
| Rule ID | 63103 |
| Event ID | 4625 |
| MITRE | T1110.001 |
| Severity | Level 5 |

![Brute Force Alert](screenshots/bruteforce.png)
![T1070 Detail](screenshots/t1070.png)

---

### ⚔️ Attack 2 — New User Account Created
**MITRE: T1136.001 — Create Account: Local Account**

```powershell
net user hacker Password123! /add
```

| Field | Details |
|---|---|
| Rule ID | 100002 (Custom!) |
| Event ID | 4720 |
| MITRE | T1136.001 |
| Severity | Level 8 |
| Timestamp | April 28, 2026 @ 02:50:03 |

![New User Alert](screenshots/previlage_1.png)

---

### ⚔️ Attack 3 — Privilege Escalation
**MITRE: T1078.003 — Valid Accounts: Local Accounts**

```powershell
net localgroup Administrators hacker /add
```

| Field | Details |
|---|---|
| Rule ID | 100003 (Custom!) |
| Event ID | 4732 |
| MITRE | T1078.003 |
| Severity | Level 12 |
| Timestamp | April 28, 2026 @ 02:47:47 |

![Privilege Escalation](screenshots/previlage_2.png)
![T1078 Alert](screenshots/t1078.png)
![All T1078](screenshots/all_t1078.png)

---

### ⚔️ Attack 4 — Security Log Cleared
**MITRE: T1070.001 — Indicator Removal: Clear Windows Event Logs**

```powershell
wevtutil cl Security
```

| Field | Details |
|---|---|
| Rule ID | 100004 (Custom!) |
| Event ID | 1102 |
| MITRE | T1070.001 |
| Severity | Level 14 — CRITICAL |
| Timestamp | April 28, 2026 @ 03:35:35 |
| Times Fired | 9 times |

![Log Cleared](screenshots/log_clear.png)
![T1070.001 Detailed](screenshots/t10070_001_detailed.png)

---

### ⚔️ Additional Detections

| Screenshot | Technique | Description |
|---|---|---|
| ![T1098](screenshots/t1098.png) | T1098 | Account Manipulation |
| ![T1484](screenshots/t1484.png) | T1484 | Administrators group changed |

---

## 🔧 Custom Detection Rules

Written in `/var/ossec/etc/rules/local_rules.xml`

```xml
<group name="local,">

  <rule id="100001" level="10" frequency="8" timeframe="120">
    <if_matched_sid>60642</if_matched_sid>
    <same_source_ip/>
    <description>Possible brute force attack detected</description>
    <mitre><id>T1110.001</id></mitre>
  </rule>

  <rule id="100002" level="8">
    <if_sid>60154</if_sid>
    <description>New local user account created</description>
    <mitre><id>T1136.001</id></mitre>
  </rule>

  <rule id="100003" level="12">
    <if_sid>60170</if_sid>
    <description>User added to Administrators group</description>
    <mitre><id>T1078.003</id></mitre>
  </rule>

  <rule id="100004" level="14">
    <if_sid>60106</if_sid>
    <description>CRITICAL: Security event log cleared</description>
    <mitre><id>T1070.001</id></mitre>
  </rule>

</group>
```

---

## 🔍 File Integrity Monitoring (FIM)

### FIM Security Events (4 syscheck events)
![FIM Security Events](screenshots/fim1.png)

### FIM Integrity Monitoring Dashboard
![FIM Dashboard](screenshots/fim2.png)

**FIM Alert — Rule 554: File added to system**
```
File:            /etc/fim-new-file.txt
Event:           added
Detection time:  68 seconds
MD5:             d41d8cd98f00b204e9800998ecf8427e
SHA1:            da39a3ee5e6b4b0d3255bfef95601890afd80709
Owner:           root
Permissions:     rw-r--r--
```

---

## 🗺️ MITRE ATT&CK Coverage

| Technique | ID | Rule |
|---|---|---|
| Brute Force: Password Guessing | T1110.001 | 63103 + 100001 |
| Create Account: Local Account | T1136.001 | 100002 |
| Valid Accounts: Local Accounts | T1078.003 | 100003 |
| Indicator Removal: Clear Logs | T1070.001 | 60106 + 100004 |
| Account Manipulation | T1098 | 60109 |
| Domain Policy Modification | T1484 | 60154 |
| File Added to System | — | 554 |

---

## 📝 Incident Reports

| ID | Incident | Severity | MITRE | Status |
|---|---|---|---|---|
| [IR-001](incident-reports/IR-001-brute-force.md) | Brute Force Attack | High | T1110.001 | ✅ Resolved |
| [IR-002](incident-reports/IR-002-privilege-escalation.md) | Privilege Escalation | High | T1078.003 + T1136.001 | ✅ Resolved |
| [IR-003](incident-reports/IR-003-log-cleared.md) | Security Log Cleared | Critical | T1070.001 | ✅ Resolved |

---

## 📚 What I Learned

```
✅ Deploying Wazuh SIEM from scratch
✅ VirtualBox networking (NAT vs Bridged)
✅ Connecting Windows agent to Wazuh
✅ Writing custom XML detection rules
✅ Simulating real attack techniques
✅ Mapping alerts to MITRE ATT&CK
✅ Configuring File Integrity Monitoring
✅ Reading raw JSON alert data
✅ Identifying false positives
✅ Writing professional incident reports
```

---

## 🔗 Contact

| Platform | Link |
|---|---|
| TryHackMe | [Your Profile] |
| LinkedIn | [Your Profile] |

---
*All simulations performed in isolated personal lab environment for educational purposes only.*

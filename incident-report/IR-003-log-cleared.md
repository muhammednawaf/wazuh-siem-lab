# Incident Report — IR-003: Security Log Cleared

|Field|Details|
|-|-|
|**Incident ID**|IR-003|
|**Date**|2026-04-28|
|**Analyst**|Nawaf|
|**Agent**|Nawaf (192.168.1.35)|
|**Severity**|Critical|
|**Status**|Resolved|

\---

## Summary

Windows Security event log cleared indicating attacker attempting to hide malicious activity. Both default and custom Wazuh rules detected this immediately. Most critical alert of the entire simulation.

\---

## Detection

|Field|Details|
|-|-|
|**Tool**|Wazuh SIEM|
|**Rule ID**|60106 + 100004|
|**Event ID**|1102|
|**MITRE**|T1070.001 — Indicator Removal|
|**Agent**|Nawaf (192.168.1.35)|
|**Times Fired**|9 times|

\---

## Timeline

|Time|Event|
|-|-|
|April 28, 2026 03:29:06|wevtutil cl Security executed|
|Immediately|Event ID 1102 generated|
|Immediately|Rule 60106 fired (default rule)|
|Immediately|Rule 100004 fired (custom rule)|
|Same day|Alert investigated|
|Same day|Confirmed as simulation|

\---

## Evidence

```
Event ID 1102 = audit log cleared
Rule 60106 fired (default Wazuh rule)
Rule 100004 fired (our custom rule)
T1070.001 Defense Evasion confirmed ✅
Alert fired 9 times total
Agent: Nawaf (192.168.1.35)
Screenshot taken as evidence
```

\---

## Raw Alert Data

```
agent.name:        Nawaf
agent.ip:          192.168.1.35
rule.id:           100004
rule.level:        14 (Critical)
rule.description:  CRITICAL: Security event log cleared
mitre.id:          T1070.001
mitre.tactic:      Defense Evasion
mitre.technique:   Clear Windows Event Logs
firedtimes:        9
```

\---

## Analysis

Security event log was cleared using wevtutil cl Security command. This is one of the most critical security events in any SOC environment. Attackers clear logs to hide evidence of their previous activity such as failed logins, new user creation, and privilege escalation. Both default Wazuh rule 60106 and our custom rule 100004 detected this immediately confirming detection engineering worked correctly.

Important note: In a real attack scenario the attacker would typically:

```
Step 1: Get initial access
Step 2: Escalate privileges
Step 3: Do damage
Step 4: Clear logs to hide tracks ← this alert
```

\---

## Verdict

```
TRUE POSITIVE
Simulated attack correctly detected by SIEM
Both default and custom rules fired correctly
Critical severity correctly assigned
```

\---

## Containment

```
→ Simulation confirmed and stopped
→ Log clearing noted in report
→ No real damage (lab environment)
→ Wazuh still had copies of all logs
   (logs sent to SIEM before clearing!)
```

\---

## Why This is Critical

```
When attacker clears logs:
→ Local Windows logs = gone
→ But Wazuh already has copies!
→ SIEM is remote = attacker can't delete
→ This is why SIEM is essential!

First questions SOC analyst asks:
→ Who cleared the log?
→ What time was it cleared?
→ What happened BEFORE clearing?
→ Are there other suspicious events?
```

\---

## Recommendations

```
→ Always send logs to remote SIEM immediately
→ Never rely on local logs only
→ Alert on Event 1102 as critical always
→ Investigate what happened before log clear
→ Check System and Application logs too
→ Implement log backup policy
→ Consider Windows Event Forwarding (WEF)
```

\---

## MITRE ATT\&CK

```
Tactic:    Defense Evasion
Technique: T1070.001 — Indicator Removal: Clear Windows Event Logs
```


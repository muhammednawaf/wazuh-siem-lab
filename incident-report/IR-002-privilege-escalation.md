# Incident Report — IR-002: Privilege Escalation

|Field|Details|
|-|-|
|**Incident ID**|IR-002|
|**Date**|2026-04-28|
|**Analyst**|Nawaf|
|**Agent**|Nawaf (192.168.1.35)|
|**Severity**|High|
|**Status**|Resolved|

\---

## Summary

Unauthorized user account created and added to Administrators group detected on Windows endpoint. Two MITRE techniques identified: T1136.001 (Create Account) and T1078.003 (Valid Accounts).

\---

## Detection

|Field|Details|
|-|-|
|**Tool**|Wazuh SIEM|
|**Rule ID**|60154 + 60170|
|**Event ID**|4720 + 4732|
|**MITRE**|T1136.001 + T1078.003|
|**Agent**|Nawaf (192.168.1.35)|

\---

## Timeline

|Time|Event|
|-|-|
|April 28, 2026 02:50:03|New user hacker created|
|Immediately after|User added to Administrators|
|Immediately|Rule 60154 fired (new user)|
|Immediately|Rule 60170 fired (admin group)|
|Same day|Both alerts investigated|
|Same day|Account deleted — resolved|

\---

## Evidence

```
Event ID 4720 = new user hacker created
Event ID 4732 = added to Administrators group
Rule 60154 fired in Wazuh
Rule 60170 fired in Wazuh
T1136.001 MITRE technique confirmed ✅
T1078.003 MITRE technique confirmed ✅
Screenshots taken as evidence
```

\---

## Attack Steps Identified

```
Step 1: net user hacker Password123! /add
        → Created new local user account
        → Event ID 4720 fired
        → MITRE T1136.001

Step 2: net localgroup Administrators hacker /add
        → Added hacker to Admin group
        → Event ID 4732 fired
        → MITRE T1078.003
```

\---

## Analysis

Attacker created new user account named hacker then immediately added it to local Administrators group gaining full system privileges. This is a classic persistence + privilege escalation combo used by real attackers. Wazuh detected both events and fired appropriate alerts. Custom rules 100002 and 100003 also confirmed working correctly.

\---

## Verdict

```
TRUE POSITIVE
Simulated attack correctly detected by SIEM
Both MITRE techniques correctly mapped
```

\---

## Containment

```
→ Account hacker deleted
→ net user hacker /delete
→ Admin group membership restored
→ No real damage (lab environment)
```

\---

## Recommendations

```
→ Monitor all admin group changes immediately
→ Alert on new user creation always
→ Implement least privilege policy
→ Review admin group members weekly
→ Require approval for new admin accounts
→ Enable Windows audit policy for group changes
```

\---

## MITRE ATT\&CK

```
Technique 1: T1136.001 — Create Account: Local Account
Tactic:      Persistence

Technique 2: T1078.003 — Valid Accounts: Local Accounts
Tactic:      Privilege Escalation
```


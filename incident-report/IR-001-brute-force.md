# Incident Report — IR-001: Brute Force Attack

|Field|Details|
|-|-|
|**Incident ID**|IR-001|
|**Date**|2026-04-28|
|**Analyst**|Nawaf|
|**Agent**|Nawaf (192.168.1.35)|
|**Severity**|High|
|**Status**|Resolved|

\---

## Summary

Multiple failed login attempts detected from local machine simulating brute force attack against Windows system.

\---

## Detection

|Field|Details|
|-|-|
|**Tool**|Wazuh SIEM|
|**Rule ID**|63103|
|**Event ID**|4625|
|**MITRE**|T1110.001 — Brute Force|
|**Agent**|Nawaf (192.168.1.35)|

\---

## Timeline

|Time|Event|
|-|-|
|April 30, 2026 01:09:30|Brute force attack started|
|Immediately|Rule 63103 fired|
|Same day|Alert investigated|
|Same day|Simulation stopped|

\---

## Evidence

```
Event ID 4625 fired multiple times
Same source attempting repeated logins
Wazuh Rule 63103 triggered
Custom Rule 100001 also fired
```

\---

## Analysis

Brute force attack simulated using net use command attempting repeated failed logins. Wazuh detected pattern of multiple failures and fired high severity alert correctly. Custom rule 100001 also triggered confirming detection engineering worked.

\---

## Verdict

```
TRUE POSITIVE
Simulated attack correctly detected by SIEM
```

\---

## Containment

```
→ Simulation stopped
→ No real damage (lab environment)
→ Rule validated as working correctly
```

\---

## Recommendations

```
→ Enable account lockout after 5 failures
→ Implement fail2ban on Linux systems
→ Monitor for repeated 4625 events
→ Alert on same IP failing 5+ times
→ Use strong passwords on all accounts
```

\---

## MITRE ATT\&CK

```
Tactic:    Credential Access
Technique: T1110.001 — Brute Force: Password Guessing
```


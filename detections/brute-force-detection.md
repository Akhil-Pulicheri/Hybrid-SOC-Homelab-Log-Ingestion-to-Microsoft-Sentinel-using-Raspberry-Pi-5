# SSH Brute Force Detection – Microsoft Sentinel

## Objective

Detect multiple failed SSH login attempts within a short time window to identify potential brute-force attacks.

---

## Log Source

- Linux Syslog
- Process: sshd
- Facility: authpriv

---


## Simulation Method

Triggered multiple failed SSH login attempts manually by entering an incorrect password several times for a test account:

```bash
ssh testuser@localhost
```

---

## Detection Query (KQL)

```kql
Syslog
| where ProcessName == "sshd"
| where SyslogMessage contains "Failed password"
| summarize FailedAttempts=count() by Computer, bin(TimeGenerated, 10m)
| where FailedAttempts >= 3
```

---

## Expected Behavior

If 3 or more failed attempts occur within 10 minutes.

---

## MITRE ATT&CK Mapping

- T1110 – Brute Force
- T1078 – Valid Accounts (potential follow-up scenario)

---

## Evidence

Screenshot of detected failed SSH attempts:

![My Photo](https://github.com/user-attachments/assets/8b900f26-cc8b-4ffc-9be8-28f600e34f19)


Screenshot of generated Sentinel incident:

![My Photo](https://github.com/user-attachments/assets/ba7c7178-2ac7-4791-ab6d-40845eb136c0)


---

## Outcome

Successfully detected simulated brute-force activity and validated detection logic within Microsoft Sentinel.

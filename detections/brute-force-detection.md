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

Generated multiple failed SSH login attempts intentionally:

```bash
for i in {1..10}; do ssh fakeuser@localhost; done
```

---

## Detection Query (KQL)

```kql
Syslog
| where ProcessName == "sshd"
| where SyslogMessage contains "Failed password"
| summarize FailedAttempts=count() by HostIP, bin(TimeGenerated, 5m)
| where FailedAttempts > 5
```

---

## Expected Behavior

If more than 5 failed login attempts occur within 5 minutes, the query identifies potential brute-force activity.

---

## MITRE ATT&CK Mapping

- T1110 – Brute Force
- T1078 – Valid Accounts (potential follow-up scenario)

---

## Evidence

Screenshot of detected failed SSH attempts:

![My Photo]![Screenshot 2026-02-16 at 11 09 48 AM](https://github.com/user-attachments/assets/8b900f26-cc8b-4ffc-9be8-28f600e34f19)


Screenshot of generated Sentinel incident:

![My Photo]<img width="3360" height="1926" alt="image" src="https://github.com/user-attachments/assets/6574e010-7a5d-4044-97e5-321d6dc18bac" />


---

## Outcome

Successfully detected simulated brute-force activity and validated detection logic within Microsoft Sentinel.

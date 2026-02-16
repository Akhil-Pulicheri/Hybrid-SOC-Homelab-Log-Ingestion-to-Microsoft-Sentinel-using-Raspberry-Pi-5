## Hybrid SOC Homelab – Raspberry Pi 5 + Microsoft Sentinel

> A hybrid security monitoring homelab built using Raspberry Pi 5 (Ubuntu 24.04) integrated with Microsoft Sentinel to simulate real-world log ingestion, detection engineering, and incident response workflows.

---

## 📌 Project Overview

This project demonstrates the design and deployment of a hybrid SOC homelab using Raspberry Pi 5 connected to Microsoft Sentinel via Azure Arc and Azure Monitor Agent (AMA).

The lab simulates enterprise-level log ingestion and detection engineering processes, focusing on authentication monitoring and brute-force detection scenarios.

---

## 🎯 Project Objectives

- Build a low-cost hybrid SOC monitoring lab
- Register Raspberry Pi as an Azure Arc-enabled machine
- Deploy Azure Monitor Agent (AMA)
- Configure Data Collection Rule (DCR)
- Ingest Linux syslog into Microsoft Sentinel
- Simulate authentication attack scenarios
- Develop detection logic using KQL
- Map detections to MITRE ATT&CK framework

---

## 🏛 Architecture

```
User Activity (SSH / System Events)
        ↓
Raspberry Pi 5 (Ubuntu 24.04)
        ↓
Azure Arc (Hybrid Machine Registration)
        ↓
Azure Monitor Agent (AMA)
        ↓
Data Collection Rule (Syslog Collection)
        ↓
Log Analytics Workspace
        ↓
Microsoft Sentinel (SIEM)
        ↓
Analytics Rules → Incidents → Investigation
```

---

## 🖥 Lab Environment

| Component | Details |
|-----------|----------|
| Hardware | Raspberry Pi 5 |
| OS | Ubuntu 24.04 LTS |
| Cloud Platform | Microsoft Azure |
| SIEM | Microsoft Sentinel |
| Log Source | Linux Syslog |
| Agent | Azure Monitor Agent (AMA) |
| Log Ingestion | Data Collection Rule (DCR) |

---

## ⚙ Implementation Steps

### 1️⃣ Ubuntu Installation & Configuration

- Installed Ubuntu 24.04 LTS
- Configured SSH access
- Verified internet connectivity

---

### 2️⃣ Azure Arc Onboarding

- Registered Raspberry Pi as Azure Arc-enabled machine
- Authenticated via Azure portal
- Verified status:

```bash
sudo azcmagent show
```

Status: Connected

---

### 3️⃣ Azure Monitor Agent Deployment

- Installed AMA extension from Azure Portal
- Verified service:

```bash
systemctl status azuremonitoragent
```

Status: Active (running)

---

### 4️⃣ Data Collection Rule Configuration

Configured DCR to collect:

- LOG_AUTH
- LOG_AUTHPRIV
- LOG_DAEMON

Minimum log level: LOG_INFO  
Destination: Log Analytics Workspace  
Assigned to Azure Arc Raspberry Pi resource.

---

## 🧪 Threat Simulation

Simulated brute-force login attempts using invalid SSH credentials.

Example:

Repeated invalid SSH login attempts to generate authentication failure events:

```bash
ssh invaliduser@localhost

```
Generated authentication failure logs successfully ingested into Sentinel.

---

## 🔎 Detection Engineering

### Brute Force Detection Query

```kql
Syslog
| where ProcessName == "sshd"
| where SyslogMessage contains "Failed password"
| summarize FailedAttempts=count() by Computer, bin(TimeGenerated, 10m)
| where FailedAttempts >= 3

```

Purpose:
Detect potential SSH brute-force attacks based on repeated failed authentication attempts.

---

## 🚨 Analytics Rule Creation

Created a Scheduled Analytics Rule in Microsoft Sentinel:

- Query: Brute-force detection KQL
- Frequency: Every 5 minutes
- Lookup period: Last 5 minutes
- Trigger threshold: FailedAttempts >= 3
- Incident creation: Enabled
- The analytics rule runs every 5 minutes and evaluates the previous 5 minutes of data. Even though the rule evaluates the last 5 minutes of data, the query groups events into a 10-minute bucket — effectively aggregating those 5-minute events into a single detection evaluation window.

Result:
Simulated brute-force attempts generated a Sentinel incident for investigation.

---

## 🛡 MITRE ATT&CK Mapping

| Technique | Description |
|------------|-------------|
| T1110 | Brute Force |
| T1078 | Valid Accounts (Potential follow-up scenario) |

Detection logic aligns with ATT&CK technique T1110 – Credential Access via brute force.

---

## 🔍 Log Investigation Example

Query to review recent authentication events:

```kql
Syslog
| where Facility in ("auth", "authpriv")
| sort by TimeGenerated desc
| take 20
```

---

## 📊 Log Verification

Verified successful ingestion within Microsoft Sentinel Logs blade.

Observed:

- Authentication failures
- SSH activity
- System daemon logs

Confirmed real-time visibility of the hybrid Linux endpoint.

---

## 📸 Screenshots

### 1️⃣ Azure Arc Machine Registration

<img width="3356" height="1924" alt="image" src="https://github.com/user-attachments/assets/5d02bb03-5d89-4d8b-9c16-41ee5b5918d9" />



---

### 2️⃣ Data Collection Rule Configuration

<img width="3354" height="1926" alt="image" src="https://github.com/user-attachments/assets/413aed55-ba77-441a-beae-c80b3d1e2a57" />
<img width="3360" height="1928" alt="image" src="https://github.com/user-attachments/assets/108d5a41-1d4b-4853-b7c3-cb86a66e4216" />




---

### 3️⃣ Sentinel Log Ingestion

<img width="3356" height="1924" alt="image" src="https://github.com/user-attachments/assets/e26155df-fa92-4857-99e2-9c9ded23de0b" />


---

### 4️⃣ Analytics Rule Configuration

<img width="3354" height="1924" alt="image" src="https://github.com/user-attachments/assets/9354e0e7-32ca-4add-957e-3385d3f98377" />


---

### 5️⃣ Generated Security Incident

<img width="3360" height="1926" alt="image" src="https://github.com/user-attachments/assets/b243c005-34e0-4869-b21f-3471fea95ff3" />

---

## ⚠ Challenges & Lessons Learned

- Data Collection Rule must be configured before log ingestion occurs
- Workspace scope selection affects DCR visibility
- Log ingestion delay can occur after deployment
- Correct syslog facility selection is critical for targeted monitoring

---

## 🚀 Future Improvements

- Integrate Suricata IDS alerts into Sentinel
- Implement alert enrichment with IP reputation
- Create Sentinel Workbooks dashboard
- Simulate lateral movement detection
- Implement automated response using Logic Apps

---

## 🧠 Skills Demonstrated

- Hybrid Cloud Security Architecture
- Microsoft Sentinel SIEM Configuration
- Azure Arc Machine Management
- Log Ingestion Pipeline Design
- Linux Syslog Monitoring
- KQL Query Development
- Threat Detection Engineering
- Incident Creation & Investigation
- MITRE ATT&CK Mapping
- SOC Workflow Simulation

---

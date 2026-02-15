# Hybrid SOC Homelab – Raspberry Pi 5 + Microsoft Sentinel

> A hybrid security monitoring homelab built using Raspberry Pi 5 (Ubuntu 24.04) integrated with Microsoft Sentinel to simulate real-world log ingestion and detection engineering workflows.

---

## 📌 Project Overview

This project demonstrates the deployment of a hybrid SOC homelab using Raspberry Pi 5 connected to Microsoft Sentinel via Azure Arc and Azure Monitor Agent (AMA).

The goal was to simulate enterprise SOC operations by ingesting Linux system logs into a cloud SIEM and performing threat detection using KQL queries.

---

## 🎯 Project Objectives

- Build a low-cost hybrid SOC lab using Raspberry Pi 5
- Register Raspberry Pi as an Azure Arc-enabled machine
- Deploy Azure Monitor Agent (AMA)
- Configure Data Collection Rule (DCR)
- Ingest Linux syslog into Microsoft Sentinel
- Perform authentication-based threat detection using KQL
- Simulate brute-force attack scenarios

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
KQL Detection & Investigation
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
| Log Ingestion Method | Data Collection Rule (DCR) |

---

## ⚙ Implementation Steps

### 1️⃣ Ubuntu Setup

- Installed Ubuntu 24.04 LTS on Raspberry Pi 5
- Configured SSH access
- Verified outbound internet connectivity

---

### 2️⃣ Azure Arc Onboarding

- Registered Raspberry Pi as Azure Arc-enabled machine
- Authenticated using Azure login
- Verified connection status:

```bash
sudo azcmagent show
```

Status: Connected

---

### 3️⃣ Azure Monitor Agent Deployment

- Installed Azure Monitor Agent extension via Azure Portal
- Verified service status:

```bash
systemctl status azuremonitoragent
```

Service status: Active (running)

---

### 4️⃣ Data Collection Rule (DCR) Configuration

Configured DCR to collect:

- LOG_AUTH
- LOG_AUTHPRIV
- LOG_DAEMON

Minimum log level: LOG_INFO  
Destination: Azure Monitor Logs (Log Analytics Workspace)

Assigned DCR to Raspberry Pi Azure Arc resource.

---

## 🧪 Threat Simulation

Simulated authentication failures to generate security-relevant logs.

Example test:

```bash
ssh invaliduser@localhost
```

This generated failed login attempts which were successfully ingested into Microsoft Sentinel.

---

## 🔎 Detection Queries

### Detect Multiple Failed SSH Logins

```kql
Syslog
| where Facility == "authpriv"
| where SyslogMessage contains "Failed password"
| summarize FailedAttempts = count() by Computer, bin(TimeGenerated, 5m)
| where FailedAttempts > 3
```

Purpose: Identify potential brute-force activity.

---

### View Recent Logs

```kql
Syslog
| sort by TimeGenerated desc
| take 20
```

---

## 📊 Log Verification

Confirmed successful log ingestion within Microsoft Sentinel Logs blade.

Authentication and daemon logs were visible and queryable in real time.

---

## ⚠ Challenges & Lessons Learned

- Azure Monitor Agent requires Data Collection Rule before logs appear
- Workspace must be correctly selected during DCR configuration
- Log ingestion delay can occur after initial deployment
- Understanding syslog facilities is critical for accurate filtering

---

## 🚀 Future Improvements

- Integrate Suricata IDS logs
- Create Sentinel Analytics Rules for automated incident generation
- Build SOC dashboards using Sentinel Workbooks
- Implement MITRE ATT&CK mapping for detections
- Simulate lateral movement detection scenarios

---

## 🧠 Skills Demonstrated

- SIEM Configuration (Microsoft Sentinel)
- Hybrid Cloud Security (Azure Arc)
- Log Ingestion & Monitoring
- Linux Syslog Analysis
- KQL Query Development
- Threat Detection Engineering
- Troubleshooting & Log Pipeline Debugging

---

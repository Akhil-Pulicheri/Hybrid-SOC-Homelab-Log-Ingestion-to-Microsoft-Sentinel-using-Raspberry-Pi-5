# Hybrid SOC Homelab – Raspberry Pi 5 + Microsoft Sentinel

## 📌 Project Overview

This project demonstrates the deployment of a hybrid SOC homelab using Raspberry Pi 5 integrated with Microsoft Sentinel via Azure Arc and Azure Monitor Agent.

The goal was to simulate real-world log ingestion and detection engineering workflows used in enterprise SOC environments.

---

## 🏗 Architecture

Raspberry Pi 5 (Ubuntu 24.04)
→ Azure Arc
→ Azure Monitor Agent
→ Data Collection Rule
→ Log Analytics Workspace
→ Microsoft Sentinel

---

## ⚙ Environment Setup

- Raspberry Pi 5
- Ubuntu 24.04 LTS
- Azure Subscription
- Microsoft Sentinel enabled

---

## 🔌 Azure Arc Onboarding

The Raspberry Pi was onboarded as a hybrid machine using Azure Arc.

```bash
sudo azcmagent show .

## 🎯 Project Objectives

- Build a hybrid SOC lab using low-cost hardware
- Ingest Linux system logs into Microsoft Sentinel
- Simulate authentication events and system activity
- Practice KQL-based threat hunting
- Develop detection logic for suspicious behavior



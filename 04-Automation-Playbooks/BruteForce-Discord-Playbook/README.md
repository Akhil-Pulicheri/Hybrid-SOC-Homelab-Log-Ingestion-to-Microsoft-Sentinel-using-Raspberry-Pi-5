
# Automated Response – Brute Force Discord Playbook

## Overview

This playbook automates incident response in Microsoft Sentinel.

When a Brute Force incident is created, the playbook:

- Triggers automatically
- Extracts incident details
- Sends formatted alert to Discord via webhook
- Includes null-check logic to prevent failures

---

## Architecture Flow

Raspberry Pi (SSH logs)
→ Microsoft Sentinel Analytics Rule
→ Incident Created
→ Automation Rule
→ Logic App Playbook
→ Discord Webhook Alert

---

## Incident Fields Sent to Discord

- Incident Title
- Severity
- Created Time (UTC)
- IP Address (if available)

---

## Example Discord Output

 — Sentinel Incident Triggered!

- Title: Brute Force attack
- Severity: Medium
- Created: 2026-02-18T10:04:53.67Z
- IP Address: X.X.X.X


---

## Key Technical Concepts Used

- Azure Logic Apps (Incident Trigger)
- HTTP Webhook Integration
- Dynamic Expressions
- Null-safe entity extraction
- Sentinel Automation Rules

# Deployment Steps – Brute Force Discord Playbook

## 1. Create Playbook

- Go to Microsoft Sentinel → Automation
- Create Playbook with Incident Trigger
- Use Managed Identity

---

## 2. Assign Permissions

- Go to Logic App → Access Control (IAM)
- Add role: Microsoft Sentinel Responder
- Assign to Playbook Managed Identity

---

## 3. Add HTTP Action

Method: POST  
Headers:
Content-Type: application/json  

---

## 4. HTTP Body Configuration

Paste:

{
  "content": "Sentinel Incident Triggered!\n\nTitle: @{triggerBody()?['object']?['properties']?['title']}\nSeverity: @{triggerBody()?['object']?['properties']?['severity']}\nCreated: @{triggerBody()?['object']?['properties']?['createdTimeUtc']}\nIP Address: @{if(empty(triggerBody()?['object']?['properties']?['relatedEntities']), 'No IP found', first(triggerBody()?['object']?['properties']?['relatedEntities'])?['properties']?['address'])}"
}

---

## 5. Create Automation Rule

- Trigger: When incident is created
- Condition: Analytic rule name = Brute Force attack
- Action: Run Playbook

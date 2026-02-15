Designed and deployed a hybrid security monitoring homelab using Raspberry Pi 5 (Ubuntu 24.04) integrated with Microsoft Sentinel via Azure Arc and Azure Monitor Agent. Implemented syslog ingestion, authentication monitoring, and detection queries to simulate real-world SOC operations.
Architecture Overview

Raspberry Pi 5 (Ubuntu 24.04)
→ Azure Arc (Hybrid Machine Registration)
→ Azure Monitor Agent
→ Data Collection Rule (Syslog: auth, authpriv, daemon)
→ Log Analytics Workspace
→ Microsoft Sentinel (SIEM)

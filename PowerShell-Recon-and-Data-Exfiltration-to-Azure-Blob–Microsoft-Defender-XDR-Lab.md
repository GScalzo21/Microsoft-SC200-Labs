# 🔎 PowerShell Recon and Data Exfiltration to Azure Blob – Microsoft Defender XDR Lab

This hands-on lab simulates a real-world threat investigation using Microsoft Defender XDR. The attacker abuses PowerShell to perform internal reconnaissance and exfiltrates data using Azure Blob storage. The lab strengthens SC-200 competencies and builds practical threat hunting skills using native Microsoft security tooling.

---

## 🎯 Lab Objective

Investigate a port scanning and data exfiltration attack using Microsoft Defender XDR.

**Key Goals:**
- Identify malicious PowerShell script activity (e.g., `portscan.ps1`, `exfiltratedata.ps1`)
- Track attacker behavior using alerts and timeline analysis
- Build and refine KQL queries in Advanced Hunting
- Confirm blob storage exfiltration
- Practice end-to-end SOC investigation workflow

---

## 🛠️ Tools & Environment

- **Microsoft Defender XDR Portal**: [security.microsoft.com](https://security.microsoft.com)
- **Azure Tenant with Defender onboarding**
- **Permissions Needed**: Incidents, Alerts, Advanced Hunting, Device Timeline

> ✅ _No Microsoft 365 E5 Sandbox or Security Copilot used_

---

## 📌 Incident Overview

- **Incident Title**: Multi-stage incident on one endpoint  
- **Incident ID**: 11517  
- **Severity**: High  
- **User**: `francis_philip`  
- **Device**: `fphilip-edr-tes`  
- **Timeframe**: June 20, 2025 - 7:14 PM to 11:49 PM  

---

## 🧭 Step-by-Step Guide

### 📂 Step 1: Navigate to the Incident

- Go to [Microsoft Defender XDR](https://security.microsoft.com)
- Select `Incidents & Alerts` → `Incidents`
- Open the incident: **"Multi-stage incident on one endpoint"**
- Review the Attack Story and alerts
<img width="1380" height="818" alt="Screenshot 2025-07-19 at 9 33 50 PM" src="https://github.com/user-attachments/assets/c7860ce3-ac9d-4631-aa67-3bc2083a5abe" />
<img width="1321" height="831" alt="Screenshot 2025-07-19 at 9 34 23 PM" src="https://github.com/user-attachments/assets/26d64b6b-d1f7-487f-ab0d-f58c8f319d82" />
<img width="1324" height="795" alt="Screenshot 2025-07-19 at 9 39 31 PM" src="https://github.com/user-attachments/assets/5aab6294-bbfe-4d24-9466-361da1ac46c8" />


---

### 🚨 Step 2: Analyze PowerShell Alerts

- Review alerts titled:
  - `powershell Hercules-soc`
  - `RCE-Detection Rule`
- Check for suspicious command-line usage like:
  - `portscan.ps1`
  - `exfiltratedata.ps1`
<img width="1308" height="830" alt="Screenshot 2025-07-19 at 10 35 41 PM" src="https://github.com/user-attachments/assets/3463e7dd-b35a-4098-b6f4-7839d64a6968" />


---

### 🔍 Step 3: Investigate with Advanced Hunting (Initial Recon)

```kql
DeviceProcessEvents
| where FileName == "powershell.exe"
| where ProcessCommandLine has_any ("portscan.ps1", "exfiltratedata.ps1", "Invoke-WebRequest")
| project Timestamp, DeviceName, FileName, ProcessCommandLine, InitiatingProcessAccountName
| order by Timestamp asc
```

<img width="1399" height="852" alt="Screenshot 2025-07-19 at 10 24 44 PM" src="https://github.com/user-attachments/assets/6c7805d6-14cf-41e8-830b-bb0c2556086c" />


### 🎯 Additional KQL Filters (Refine Search by Date and Path)

```
DeviceProcessEvents
| where FileName == "powershell.exe"
| where ProcessCommandLine contains @"C:\programdata"
| where Timestamp between (datetime(2025-06-20T00:00:00Z) .. datetime(2025-06-21T00:00:00Z))
| project Timestamp, DeviceName, ProcessCommandLine
| order by Timestamp asc
```

### ☁️ Step 4: Confirm Exfiltration to Azure Blob

Look for outbound connections to:
sacyberrangedanger.blob.core.windows.net
sacyberrange00.blob.core.windows.net
Script used:
powershell.exe -ExecutionPolicy Bypass -File C:\programdata\exfiltratedata.ps1
<img width="1039" height="216" alt="Screenshot 2025-07-19 at 10 36 02 PM" src="https://github.com/user-attachments/assets/09f663b2-d102-48fb-81a2-e65e505d069b" />

### 🕵️ Step 5: Pivot to Device Timeline

Navigate to Device Timeline for fphilip-edr-tes
Filter to Command Line Events
Investigate PowerShell executions, custom scripts, or persistence attempts
<img width="1340" height="806" alt="Screenshot 2025-07-19 at 9 40 52 PM" src="https://github.com/user-attachments/assets/23b54efd-e702-4ffd-aa90-010659d53d09" />
<img width="1386" height="704" alt="Screenshot 2025-07-19 at 10 36 42 PM" src="https://github.com/user-attachments/assets/d9365390-4d44-4e7e-84d4-5b96e312a956" />


### 🧪 Key Findings Summary

The attacker used Invoke-WebRequest to download:
portscan.ps1
pwncrypt.ps1
Ran internal port scans across the network
Exfiltrated data to Azure Blob Storage using:
exfiltratedata.ps1
✅ No evidence of mimikatz.exe or scheduled tasks

### 🧠 MITRE ATT&CK Mapping

Technique	Description
T1046	Network Service Scanning
T1041	Exfiltration Over C2 Channel
T1059	Command and Scripting Interpreter (PowerShell)

### ✅ Skills Practiced

Defender XDR incident triage
KQL hunting & filtering
Timeline analysis
PowerShell abuse detection
Blob exfiltration investigation

### 🎓 SC-200 Exam Relevance

This lab covers SC-200 objectives including:

Investigate threats with Microsoft Defender XDR
Hunt threats using KQL
Analyze threat actor behavior via telemetry
Confirm endpoint and cloud-based attacker activity


### Giuseppe Scalzo
LinkedIn: https://www.linkedin.com/in/giuseppe-scalzo-/


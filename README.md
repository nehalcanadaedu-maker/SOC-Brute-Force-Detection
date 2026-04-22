# SOC Brute Force Detection using Splunk

## 📌 Project Overview

This project demonstrates a Security Operations Center (SOC)-style detection system built in Splunk to identify brute force attacks followed by successful authentication attempts. It correlates Windows authentication logs to detect potential account compromise scenarios.

---

## 🧠 Attack Scenario

The detection focuses on the following pattern:

* Multiple failed login attempts (brute force / password guessing)
* Followed by a successful login for the same user

This may indicate:

* Credential stuffing
* Brute force attacks
* Account takeover

---

## 🛠️ Technology Stack

* Splunk Enterprise
* Windows Security Event Logs
* Splunk Universal Forwarder
* SPL (Search Processing Language)

---

## 🏗️ Project Workflow

Windows Machine → Splunk Forwarder → Splunk Index (main) → Correlation Search → Alert → Dashboard

---

# 🖥️ Splunk Installation

### Splunk Installation


**Description:** Splunk Enterprise is successfully installed and running, providing the SIEM platform used for log collection, search, and security monitoring.

---

# 🔐 Splunk Login Interface

### Splunk Login

**Description:** Shows successful login to Splunk Web UI, confirming access to the Search & Reporting environment for security analysis.

---

# 📡 Universal Forwarder Installation

### Splunk Universal Forwarder Installation

![Forwarder Install](screenshots/forwarder-install.png)

**Description:** Splunk Universal Forwarder installed on a Windows endpoint to collect and forward Security Event Logs to the Splunk server.

---

# 📥 Data Ingestion Verification

### Data Ingestion into Splunk

![Data Ingestion](screenshots/data-ingestion.png)

**Description:** Confirms successful ingestion of Windows Security Event Logs into Splunk index (main), validating data pipeline setup.

---

# 🔍 Raw Log Search

### Raw Authentication Logs

![Raw Logs](screenshots/raw-logs.png)

**Description:** Displays raw Windows authentication events including failed (4625) and successful (4624) login attempts used for detection engineering.

---

# 🧠 Correlation Search (Brute Force Detection)

### Brute Force Correlation Search

![Correlation Search](screenshots/correlation-search.png)

**Description:** SPL query correlating multiple failed login attempts followed by a successful login for the same user to detect potential brute force attacks.

---

## ⚙️ Detection Logic (SPL Query)

```spl
index=main (EventCode=4625 OR EventCode=4624)
| stats 
    count(eval(EventCode=4625)) as failures,
    count(eval(EventCode=4624)) as successes,
    values(src_ip) as ip
    by Account_Name
| where failures >= 3 AND successes >= 1
```

---

# 🚨 Alert Triggered

### Alert Triggered

![Alert Triggered](screenshots/alert-triggered.png)

**Description:** Splunk alert triggered when correlation rule detects suspicious authentication behavior indicating possible credential compromise.

---

# 📊 Dashboard Overview

### SOC Brute Force Dashboard

![Dashboard](screenshots/Dashboard.png)

**Description:** SOC dashboard visualizing authentication activity, including failed logins, successful logins, and suspicious user behavior trends.

---

# ⚖️ Risk Score Panel

### Risk-Based Scoring

![Risk Score](screenshots/risk-score.png)

**Description:** Displays calculated risk scores based on failed login attempts and successful authentication patterns to prioritize suspicious activity.

---

## 🧮 Risk Scoring Model

* Failed login = +10 points
* Successful login after failures = +40 points

Used to prioritize suspicious activity.

---

# 🧭 MITRE ATT&CK Mapping

### MITRE ATT&CK Mapping

![MITRE](screenshots/mitre.png)

**Description:** Maps detected brute force activity to MITRE ATT&CK framework techniques T1110 (Brute Force) and T1078 (Valid Accounts).

---

## 🧹 Security Log Cleared Event

### Security Log Cleared Event

![Log Cleared Event](screenshots/log-cleared.png)

**Description:** Displays Windows Security Event ID 1102, indicating that the security event log was cleared. This is a high-risk event often associated with attempts to hide malicious activity or erase forensic evidence.

---

## 🚨 Alert Configuration

* Trigger condition: Number of results > 0
* Schedule: Every 5 minutes
* Time range: Last 10–15 minutes
* Severity: High

---

## 📈 Key Features

* Brute force detection
* Correlation of failed and successful logins
* Risk scoring model
* MITRE ATT&CK mapping
* SOC-style dashboard

---

## 🧠 Skills Demonstrated

* SIEM (Splunk) analysis
* Threat detection engineering
* SPL query development
* MITRE ATT&CK mapping
* Security monitoring & alerting

---

## 🚀 Future Improvements

* Geo-location anomaly detection
* Password spraying detection
* Lateral movement detection
* SOAR integration (ServiceNow/Jira simulation)

---

## 👤 Author

SOC Lab Project – Splunk Brute Force Detection

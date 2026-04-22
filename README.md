# SOC-Brute-Force-Detection
SOC detection project using Splunk to identify brute force attacks by correlating failed and successful login attempts. Includes MITRE ATT&amp;CK mapping (T1110, T1078), risk scoring, and a SOC-style dashboard for threat visibility and analysis.
# SOC Brute Force Detection using Splunk

## 📌 Project Overview

This project demonstrates a Security Operations Center (SOC)-style detection system built in Splunk to identify potential brute force attacks followed by successful authentication attempts. The goal is to simulate a real-world SIEM use case where failed login attempts are correlated with successful logins to detect possible account compromise.

---

## 🎯 Objective

To detect suspicious authentication patterns where multiple failed login attempts are followed by a successful login for the same user within a short time window, indicating potential brute force attacks and credential compromise.

---

## 🧠 Attack Scenario

Attack pattern detected:

1. Multiple failed login attempts (possible password guessing)
2. Successful login after failures (possible compromise)

This pattern aligns with:

* Credential stuffing
* Brute force attacks
* Account takeover attempts

---

## 🛠️ Technology Stack

* Splunk Enterprise / Splunk Free
* Windows Security Event Logs
* SPL (Search Processing Language)

---

## 📊 Detection Logic (SPL Query)

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

## ⚖️ Risk Scoring Model

To prioritize alerts, a simple risk scoring model was implemented:

* Each failed login = +10 points
* Successful login after failures = +40 points

Example SPL:

```spl
index=main (EventCode=4625 OR EventCode=4624)
| stats
    count(eval(EventCode=4625)) as failures,
    count(eval(EventCode=4624)) as successes
    by Account_Name, src_ip
| eval risk_score = (failures * 10) + (successes * 40)
| where failures >= 3 AND successes >= 1
| sort - risk_score
```

---

## 🧭 MITRE ATT&CK Mapping

* **T1110** – Brute Force (Password Guessing)
* **T1078** – Valid Accounts (Successful authentication after compromise)

---

## 🚨 Alert Configuration

* Trigger condition: Number of results > 0
* Schedule: Every 5 minutes
* Time range: Last 10–15 minutes
* Severity: High

---

## 📊 Dashboard Components

The following panels were created:

1. Brute Force Detection Overview (Correlation results)
2. Failed Login Trends
3. Successful Login Activity
4. Risk Score Table (Top suspicious users/IPs)

---

## 🏗️ Architecture

```
Windows Machines (Event Logs)
        ↓
Splunk Index (main)
        ↓
Correlation Search (SPL)
        ↓
Alert Trigger (5 min schedule)
        ↓
Dashboard Visualization
```

---

## 📸 Evidence (Screenshots)

* Dashboard view
* Alert triggered
* Email notification
* Splunk search results

---

## 🔍 Key Features

* Real-time brute force detection
* Correlation of failed and successful logins
* Risk-based scoring model
* MITRE ATT&CK mapping
* SOC-style dashboard visualization

---

## 📈 Skills Demonstrated

* SIEM analysis (Splunk)
* SPL query development
* Threat detection engineering
* MITRE ATT&CK framework mapping
* SOC alert configuration
* Dashboard creation

---

## 👤 Author

SOC Lab Project – Brute Force Detection using Splunk

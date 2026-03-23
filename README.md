📋 Sections

🏷️ Header & Badges
Title, team names, tech stack badges (Python, Scikit-learn, Flask, VirtualBox, MIT License)
📌 Project Overview
1-paragraph summary of what the system does — great for recruiters browsing your profile
❓ Problem Statement
Why traditional IDS fails — zero-day, polymorphic malware, APT threats, false positives
🏗️ System Architecture
3-VM setup explained with an ASCII diagram (VM1 Victim, VM2 Attacker, VM3 AI Engine)
⚙️ How It Works
Data flow from packet capture → feature extraction → ML classification → response
🤖 ML Models
Table: Random Forest, Isolation Forest, Logistic Regression, Autoencoders (future)
🛠️ Tech Stack
Full table of tools — Python, TCPDump, Cowrie, Flask, Kibana, TensorFlow
📊 Results & Performance
>95% accuracy, <10ms API response, F1 score ~1.0, 24hr auto-retraining
🚀 Future Roadmap
LSTM, CNN, Apache Airflow, live Cowrie streaming, AWS/Azure cloud deployment
👥 Team & License
All 4 authors, department, institution, MIT license    


📄 Raw Markdown
# 🛡️ AI-Powered Intrusion Detection & Attack Prediction System

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat&logo=tensorflow&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat&logo=flask&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-183A61?style=flat&logo=virtualbox&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat)

> A real-time, ML-powered Intrusion Detection System that combines Honeypot technology with
> supervised and unsupervised Machine Learning to detect known attacks, predict zero-day threats,
> and automatically respond — all in under 10 milliseconds.

**Team:** Manojkumar M · Thirumalai Raman N · Harish Raj S · Balaji E
**Program:** B.Tech – Artificial Intelligence & Data Science | 1st Year

---

## 📌 Project Overview

Traditional Intrusion Detection Systems rely on fixed rule-sets and signature databases that fail
against modern threats like zero-day exploits, polymorphic malware, and Advanced Persistent
Threats (APTs). This project builds an AI-powered IDS that **learns** what normal network traffic
looks like and flags deviations in real time — catching threats that no existing signature has
ever described.

The system is deployed across **3 isolated Virtual Machines**, uses a **Cowrie Honeypot** to
capture authentic attacker behaviour, and runs **Random Forest + Isolation Forest** models via
a **Flask REST API** with sub-10ms response time.

---

## ❓ Problem Statement — Why Traditional IDS Fails

| Threat Type | Traditional IDS | This System |
|---|---|---|
| Known attacks (DoS, Brute Force) | ✅ Detects (if rule exists) | ✅ Detects (>95% accuracy) |
| Zero-day exploits | ❌ Blind — no signature | ✅ Isolation Forest anomaly detection |
| Polymorphic malware | ❌ Evades rule engine | ✅ Behavioural baseline detection |
| APT (slow, multi-stage) | ❌ Below all thresholds | 🔄 LSTM planned |
| High false positive rate | ❌ Alert fatigue | ✅ F1-score approaching 1.0 |

**Real-world motivation:**
- 2020 — SolarWinds: 18,000+ organisations compromised, undetected for months
- 2021 — Colonial Pipeline: $4.4M ransom, US fuel supply disrupted
- 2022 — Uber: Social engineering bypassed MFA, exposed internal systems

---

## 🏗️ System Architecture

The system runs across 3 isolated Virtual Machines for a safe, repeatable, and realistic
environment:

```
┌─────────────────────────────────────────────────────────────┐
│                       Network Segment                        │
│                                                             │
│  ┌────────────┐    attack traffic    ┌────────────────────┐  │
│  │   VM 2     │ ──────────────────► │       VM 1         │  │
│  │  Attacker  │                     │  Victim / Sensor   │  │
│  │            │                     │  • Cowrie Honeypot │  │
│  │  • Hydra   │                     │  • TCPDump capture │  │
│  │  • Nmap    │                     │  • SSH/Telnet lure │  │
│  └────────────┘                     └────────┬───────────┘  │
│                                              │ .pcap files  │
│                                              ▼              │
│                                    ┌─────────────────────┐  │
│                                    │        VM 3         │  │
│                                    │    AI Engine        │  │
│                                    │  • ML Models        │  │
│                                    │  • Flask REST API   │  │
│                                    │  • Auto-retraining  │  │
│                                    │  • Dashboard        │  │
│                                    └─────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## ⚙️ How It Works

### 1. Data Collection
- **Cowrie Honeypot** emulates SSH/Telnet. Every attacker command, credential attempt, and
  file transfer is silently logged as JSON — all interactions are malicious by definition.
- **TCPDump** runs continuously, capturing all packets as binary `.pcap` files.

### 2. Feature Extraction
**Tshark / Wireshark** converts `.pcap` → `.csv` with features:
- Source / destination IP and port
- Protocol type (TCP, UDP, ICMP)
- Packet length and TCP flag combinations
- Connection rate and inter-arrival timing

### 3. ML Classification (3-Level Cascade)
```
Incoming Traffic
      │
      ▼
[Logistic Regression] ──── obvious normal ──► Allow
      │ uncertain
      ▼
[Random Forest] ──────────── ATTACK (>95%) ──► Block + Alert
      │ uncertain
      ▼
[Isolation Forest] ────── anomaly / zero-day ──► Flag + Investigate
```

### 4. Automated Response
- IP blocked immediately
- Security dashboard notified
- Event logged and queued for 24-hour model retraining
- If retrained model outperforms old model → auto-deployed

---

## 🤖 Machine Learning Models

| Model | Type | Role | Performance |
|---|---|---|---|
| **Random Forest** | Supervised | Primary classifier — known attacks | >95% accuracy |
| **Isolation Forest** | Unsupervised | Zero-day & anomaly detection | No labels needed |
| **Logistic Regression** | Supervised | Fast baseline + validation | ~90% accuracy |
| **Autoencoders** | Deep Learning | Normal traffic reconstruction *(planned)* | — |
| **LSTM Networks** | Deep Learning | Slow multi-stage APT detection *(planned)* | — |

---

## 🛠️ Technology Stack

| Category | Tools |
|---|---|
| **Programming** | Python 3, Bash |
| **Machine Learning** | Scikit-learn, TensorFlow / Keras |
| **Data Processing** | Pandas, NumPy, Matplotlib, Seaborn |
| **Network Capture** | TCPDump, Tshark, Wireshark |
| **Honeypot** | Cowrie (SSH/Telnet emulation) |
| **Infrastructure** | VirtualBox (3-VM setup) |
| **API Layer** | Flask REST API |
| **Dashboard** | Kibana, Power BI |
| **Automation** | Cron (24hr retraining pipeline) |

---

## 📊 Results & Performance

```
┌──────────────────────────────────────┐
│          Key Metrics                 │
│                                      │
│  Detection Accuracy   >95%           │
│  API Response Time    <10 ms         │
│  F1 Score             ~1.0           │
│  Retraining Cycle     Every 24 hrs   │
│  False Positive Rate  Very Low       │
└──────────────────────────────────────┘
```

### Attack Scenario — Brute Force SSH (End-to-End)

1. **Attack initiated** — Attacker runs Hydra targeting SSH on VM1
2. **Honeypot traps** — Cowrie logs every attempt; attacker is unaware
3. **Traffic captured** — TCPDump records high-rate SSH connections from single IP
4. **Feature extraction** — High connection rate, repeated auth failures, short sessions
5. **ML classifies** — Random Forest: `ATTACK = 1`, confidence **99.2%**, in **<10ms**
6. **Response** — IP blocked, dashboard alerted, event queued for retraining

---

## 🌍 Real-World Applications

| Industry | Use Case |
|---|---|
| ☁️ Cloud / AWS / Azure | Monitor thousands of cloud nodes for intrusions |
| 🏦 Banking & Finance | Detect credential attacks and insider threats |
| 📡 IoT Networks | Identify compromised edge devices early |
| 🏛️ Government & Defence | APT and nation-state threat detection |
| 🏭 Industrial Control | Protect SCADA and OT environments |
| 📶 Telecommunications | Monitor traffic at scale for DDoS |

---

## 🚀 Future Roadmap

- [ ] **LSTM Networks** — Detect slow multi-stage APT attacks unfolding over days/weeks
- [ ] **CNN Packet Analysis** — Local pattern detection for DDoS and protocol anomalies
- [ ] **Apache Airflow** — Fully automated retrain → validate → deploy pipeline
- [ ] **Live Cowrie Streaming** — Real-time log feed into detection engine (no 24hr delay)
- [ ] **Kibana Dashboard** — Live monitoring of attack trends and model confidence
- [ ] **Cloud Deployment** — AWS Security Hub + Azure Sentinel integration

---

## 👥 Team

| Name | Role |
|---|---|
| **Manojkumar M** | ML Pipeline & Model Training |
| **Thirumalai Raman N** | Network Capture & Feature Engineering |
| **Harish Raj S** | Honeypot Setup & Data Collection |
| **Balaji E** | Flask API & Dashboard Integration |

**Department:** Artificial Intelligence & Data Science
**Year:** 1st Year B.Tech

---

## 📄 License

This project is licensed under the **MIT License** — free to use, modify, and distribute
with attribution.

---

*Built with a focus on real-world cybersecurity problems — combining authentic attacker
data from Cowrie honeypots with adaptive machine learning for the next generation of
intrusion detection.*


👁 Preview
🛡️ AI-Powered Intrusion Detection & Attack Prediction System
Python
Scikit-learn
TensorFlow
Flask
VirtualBox
MIT License
Status: Active
A real-time, ML-powered IDS that combines Honeypot technology with supervised and unsupervised Machine Learning to detect known attacks, predict zero-day threats, and automatically respond — all in under 10 milliseconds.
Team: Manojkumar M · Thirumalai Raman N · Harish Raj S · Balaji E
Program: B.Tech – Artificial Intelligence & Data Science | 1st Year

📌 Project Overview
Traditional IDS rely on fixed rule-sets that fail against modern threats. This project builds an AI-powered IDS that learns normal network traffic and flags deviations in real time — catching threats that no existing signature has ever described.

❓ Problem Statement
Threat Type	Traditional IDS	This System
Known attacks	✅ Detects	✅ >95% accuracy
Zero-day exploits	❌ Blind	✅ Isolation Forest
Polymorphic malware	❌ Evades	✅ Behavioural baseline
APT (multi-stage)	❌ Missed	🔄 LSTM planned
🤖 ML Models
Model	Type	Performance
Random Forest	Supervised	>95% accuracy
Isolation Forest	Unsupervised	Zero-day detection
Logistic Regression	Supervised	~90% baseline
Autoencoders	Deep Learning	Planned
📊 Key Results
Detection Accuracy: >95%
API Response Time: <10 ms
F1 Score: ~1.0
Retraining Cycle: Every 24 hours (auto)
🚀 Future Roadmap
☐ LSTM Networks for APT detection
☐ CNN Packet Analysis for DDoS
☐ Apache Airflow pipeline automation
☐ AWS / Azure cloud deployment




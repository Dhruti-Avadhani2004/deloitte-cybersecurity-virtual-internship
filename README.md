# Deloitte Cybersecurity Virtual Internship

This repository documents my completion of the **Deloitte Australia Cybersecurity Virtual Experience Program**, hosted on Forage. It serves as a portfolio record of the tasks completed, the analytical work performed, and the skills developed throughout the program.

---

## Program Overview

**Provider:** Deloitte Australia (via Forage)
**Duration:** 30–60 minutes
**Difficulty:** Introductory
**Focus Area:** Cybersecurity — breach investigation and log analysis

The program simulates the experience of joining Deloitte's cybersecurity team, working on behalf of a client (Daikibo Industrials / Macora Industries) who suspects their internal operational dashboard has been compromised following a sensitive data leak to the press.

---

## Tasks

### Task 1 — Identify the source of a data breach

**Scenario:** A major news publication revealed sensitive internal information about a client's assembly line failure. The client suspects the security of their real-time operational dashboard has been breached and engaged Deloitte to investigate.

**What I did:**
- Assessed whether a direct attack from the internet was possible given the dashboard's architecture (intranet-hosted, VPN-gated)
- Analysed a `web_requests.log` file covering the suspected breach window
- Identified anomalous automated behaviour from a specific internal IP address
- Traced the activity to a specific user account

**Key finding:** IP `192.168.0.101` was running an automated script that polled machine status APIs across all four factories every hour at an exact interval — behaviour that is impossible to produce manually and inconsistent with the dashboard's design (no auto-refresh functionality). After the associated user (`mdB7yD2dp1BFZPontHBQ1Z`) logged in at `16:04`, the script began successfully exfiltrating data.

**Full write-up:** [`cybersecurity-breach-analysis.md`](./cybersecurity-breach-analysis.md)

---

## Repository Structure

```
deloitte-cybersecurity-virtual-internship/
│
├── README.md
├── cybersecurity-breach-analysis.md
│
└── assets/
    ├── web_activity.log
    ├── suspicious-logs.txt
    ├── successful-requests-after-401
    ├── deloitte internship quiz.pdf
    ├── deloitte_internship_completion_certificate.pdf
    └── .gitignore
```

---

## Skills Demonstrated

| Skill | Description |
|---|---|
| Web log analysis | Reading and interpreting HTTP request logs to identify patterns |
| Anomaly detection | Distinguishing automated scripted behaviour from human browsing sessions |
| Authentication & session analysis | Understanding login flows, session tokens, and authorisation headers |
| Network security fundamentals | Intranet vs internet exposure, VPN access control |
| Client communication | Translating technical findings into clear, non-technical explanations |

---

## Certificate

Program completed via [Forage](https://www.theforage.com/) — Deloitte Australia Cybersecurity Virtual Experience Program.

[View Certificate](./assets/deloitte_internship_completion_certificate.pdf)
---

## Disclaimer

All data, IP addresses, user IDs, and company names used in this program are fictional and provided solely for educational purposes as part of the virtual internship simulation.

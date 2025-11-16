# 🚨 Web Attack Investigation Report – EventID: 116  
**Prepared by: Jukuri Nithin Kumar – SOC Analyst**  
**Platform: LetsDefend SOC Analyst Environment**  
**Case Type: Web Attack – JavaScript Code Detected in URL**  
**Status: Closed – Unsuccessful Attack**

---

## 🛑 1️⃣ Alert Details (Initial Trigger)

| Field | Value |
|------|------|
| Event ID | 116 |
| Event Time | Feb 26, 2022 – 06:56 PM |
| Rule | SOC166 – JavaScript Code Detected in Requested URL |
| Severity | Medium |
| Source IP | 112.85.42.13 (External – Internet) |
| Destination IP | 172.16.17.17 (Internal WebServer1002) |
| Hostname | WebServer1002 |
| Method | GET |
| URL Requested | https://172.16.17.17/search/?q=<$script>javascript:$alert(1)<$/script> |
| Device Action | Allowed |
| Alert Trigger Reason | JavaScript payload detected in URL |

---

## 🎯 2️⃣ Investigation Summary

An external IP attempted multiple malicious JavaScript injection payloads through the `search` query field of the internal web server.  
Objective was to test whether the server is vulnerable to **Cross-Site Scripting (XSS)**.

➡️ This was a **reconnaissance scanning** behavior  
➡️ Multiple payload variations indicate automation

---

## 💡 3️⃣ What is Malicious in the URL?

The attacker included **JavaScript code inside URL parameters**:

Example payload:
```
<script>javascript:alert(1)</script>
```

### ❌ Why this is dangerous
If a website **does not sanitize** user input:
- JavaScript executes inside victim's browser
- Attacker can steal session cookies
- Perform account takeovers
- Deface web pages
- Redirect users to malware/phishing

➡️ This is a **Common OWASP XSS attack**

---

## 🔍 4️⃣ Log Analysis Findings

Logs show multiple payload attempts:

| Payload example | Response | Result |
|----------------|----------|--------|
| `?q=test` | 200 OK | Normal |
| `?q=prompt(8)` | 302 Redirect | Blocked |
| `<script>alert(1)` | 302 Redirect | Blocked |
| `<svg><script>alert()` | 302 Redirect | Blocked |

📌 Common pattern:
- **JavaScript payloads always return 302 Redirect with 0B size**
- Means server **blocked/sanitized** the script input

✔ **Attack unsuccessful**

---

## 🌍 5️⃣ Threat Intelligence – Source IP: 112.85.42.13

| Indicator | Result |
|----------|--------|
| ISP | AS 4837 – China Unicom |
| Reputation | Negative community trust score |
| Geolocation | China |
| Behavior | Automated malicious scanning |

### Analyst Conclusion:
This IP is **malicious** and should be **blocked** at perimeter defenses.

---

## 🖥️ 6️⃣ EDR Analysis – WebServer1002

### What we checked:
✔ Network connections  
✔ Running processes  
✔ System activity  
✔ Signs of remote command execution

### Results:
- No suspicious processes detected
- No new files or privilege escalation
- No malware observed

📌 Web server did **not** execute attacker JavaScript code  
➡ System remains **not compromised**

---

## 🛡️ 7️⃣ Containment Actions

| Action | Status |
|--------|--------|
| Block malicious IP (112.85.42.13) | ✔ Recommended |
| Web server isolated via defensive rules | ✔ Completed |
| Review input sanitization | ✔ Required follow-up |
| No escalation to Tier-2 necessary | ✔ True |

---

## 🧠 8️⃣ Final Verdict

**Classification: Unsuccessful Web Attack – XSS Scan Attempt**

➡️ Attacker attempted to inject JavaScript  
➡️ Server **successfully blocked script execution**  
➡️ No confirmed compromise  
➡️ Preventive actions taken

**Case Closed 🛑**

---

## 📌 9️⃣ IOCs (Indicators of Compromise)

| Type | Value |
|------|------|
| Attacker IP | 112.85.42.13 |
| Target Host | 172.16.17.17 (WebServer1002) |
| Payload | `<script>alert(1)</script>` |
| User-Agent | Firefox/40.1 – Common in scanning tools |

---

## 🧩  🔟 Skills Demonstrated

- SOC Alert Triage & Prioritization
- Firewall & Proxy Log Analysis
- URL / JavaScript Attack inspection
- Threat Intel & IP Reputation Lookup
- EDR System Investigation
- Web Attack Classification (OWASP XSS)
- Containment and Incident Documentation
- Report Writing for Security Incidents

---

## ✨ Conclusion Statement

This investigation showcases how to analyze:
- A suspected **Web Application Attack**
- By reviewing logs & threat intel
- To distinguish **harmless traffic vs real threats**

WebServer1002 is **secure for now**, but:
➡ Strengthening **input sanitization** is strongly recommended  
➡ Ensuring **WAF protection** is ideal going forward

---

📌 Report Prepared by:  
**🛡️  Jukuri Nithin Kumar — SOC Analyst**


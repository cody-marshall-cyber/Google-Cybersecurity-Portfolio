# [SECURITY INVESTIGATION: PYRAMID OF PAIN ANALYSIS]
**Analyst:** marshall_c  
**Date:** May 3, 2026  
**Subject Hash:** 54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b

---

#### **[VERDICT: MALICIOUS]**
**Determination:** Based on the VirusTotal Intelligence report, this file is classified as **Malicious**.
**Reasoning:** The artifact displays a critically high vendor detection ratio (58/72 or similar) and a significant negative community score. Static and behavioral analysis confirms the file is an Emotet or similar Trojan variant designed to establish persistence and download secondary payloads. Sandbox reports indicate unauthorized registry modifications and connections to known malicious Command and Control (C2) infrastructure.

---

#### **[INDICATORS OF COMPROMISE (IOCS)]**


| IoC Type | Technical Data | Analysis / Significance |
| :--- | :--- | :--- |
| **Hash Values** | **MD5:** 69630e45749969066551527c79010103 | Unique cryptographic fingerprint used for signature-based detection across endpoint solutions. |
| **IP Addresses** | **185.224.128.162** | A hard-coded remote address contacted by the malware on port 8080 for initial C2 beaconing. |
| **Domain Names** | **coop-unions.ru** | A malicious domain identified in the Relations tab as a distribution point for the payload's second-stage executable. |
| **Host Artifacts** | **regidle.exe** | A malicious executable file dropped in the user's temporary directory to facilitate system-wide persistence. |

---

#### **[TTPS: MITRE ATT&CK ALIGNMENT]**
*   **Phishing: Spearphishing Attachment (T1566.001):** Initial delivery via a password-protected spreadsheet to evade automated mail gateways.
*   **User Execution: Malicious File (T1204.002):** Exploiting social engineering to trick the employee into entering a password and executing the payload.
*   **Command and Scripting Interpreter (T1059):** Observed usage of PowerShell or WScript to download and execute secondary malicious binaries.

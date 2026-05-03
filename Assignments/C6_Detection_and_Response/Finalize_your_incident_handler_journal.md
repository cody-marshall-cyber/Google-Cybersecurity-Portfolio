# [INCIDENT HANDLER JOURNAL: COMPREHENSIVE]
**Analyst:** marshall_c  
**Date:** May 3, 2026  
**Case Reference:** [Healthcare Ransomware & SOC Operational Response]  

---

#### **[ENTRY #01: DOCUMENTING A CYBERSECURITY INCIDENT]**
**Date:** May 3, 2026  
**Description:** This entry documents the intake of a critical ransomware incident impacting a healthcare facility. This incident spans two NIST phases:
1. **Detection and Analysis:** The organization identified the ransomware at 09:00 a.m. and initiated contact with external technical partners for forensic assistance.
2. **Containment, Eradication, and Recovery:** Immediate containment was attempted by shutting down computer systems to prevent further encryption, while recovery efforts were deferred to specialized third-party responders.

**Tools Used:** None (Initial Intake)
**The 5 Ws:**
*   **Who:** An organized cybercriminal syndicate targeting the healthcare and transportation sectors.
*   **What:** A catastrophic ransomware infection resulting in encrypted PHI and operational shutdown.
*   **Where:** A small, U.S. based primary care medical clinic.
*   **When:** Tuesday at approximately 09:00 a.m.
*   **Why:** The breach occurred due to a successful spear phishing campaign where employees downloaded a malicious attachment, allowing threat actors to execute ransomware and extort a financial payout.

---

#### **[ENTRY #02: ANALYZING A PACKET CAPTURE FILE]**
**Date:** May 3, 2026  
**Description:** For this activity, I utilized **Wireshark** to analyze a packet capture (.pcap) file. Wireshark is a robust network protocol analyzer that provides a graphical user interface for visualizing traffic. Its primary value is allowing analysts to perform deep packet inspection to identify anomalies that signify malicious activity within the network.

**Tools Used:** Wireshark
**The 5 Ws:** [N/A - Technical Tool Analysis]
**Additional Notes:** The GUI is initially complex, but the ability to colorize protocols and follow TCP streams makes it an essential tool for understanding the "how" behind an intrusion.

---

#### **[ENTRY #03: CAPTURING MY FIRST PACKET]**
**Date:** May 3, 2026  
**Description:** In this exercise, I employed **tcpdump** to capture and analyze live network traffic. Unlike Wireshark, tcpdump is accessed via the command line interface (CLI). It is a lightweight, high-performance tool used to capture and filter traffic on remote servers where a graphical interface is unavailable.

**Tools Used:** tcpdump
**The 5 Ws:** [N/A - Technical Tool Analysis]
**Additional Notes:** Learning the CLI syntax for filters was challenging. Mis-typing commands lead to incorrect outputs, reinforcing the need for precision when capturing data in high-stakes environments.

---

#### **[ENTRY #04: INVESTIGATE A SUSPICIOUS FILE HASH]**
**Date:** May 3, 2026  
**Description:** Documentation of a suspicious file hash investigation within a financial services SOC context. This incident occurred during the **Detection and Analysis** phase, as it involved evaluating a system alert to determine the true nature of a threat.
**Tools Used:** VirusTotal
**The 5 Ws:**
*   **Who:** An unknown malicious actor utilizing automated delivery systems.
*   **What:** A phishing email containing a malicious spreadsheet attachment with SHA-256 hash: 54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b.
*   **Where:** An employee workstation at a financial services organization.
*   **When:** 1:20 p.m. (Alert generation time following user execution).
*   **Why:** An employee bypassed perimeter defenses by entering a password provided in a phishing email to open a malicious spreadsheet.

---

#### **[REFLECTIONS AND NOTES]**
1. **Challenging Activities:** I found the **tcpdump** exercises particularly challenging. Being new to the command line, the learning curve for syntax and specific flags was steep. I experienced frustration when the output didn't match expectations, but re-reading the documentation and working slowly allowed me to successfully capture the traffic. 
2. **Evolution of Understanding:** My understanding of incident response has evolved significantly. I now recognize that response is not just a single action, but a structured lifecycle. Learning the importance of the plans, people, and specific tools required at each phase has equipped me with a much more complex and professional perspective on security operations.
3. **Preferred Tools:** I most enjoyed learning about **Network Traffic Analysis**. Using Wireshark and tcpdump to see data moving in real-time was fascinating. This experience has ignited a strong interest in network forensics, and I intend to continue building my proficiency in protocol analysis to support future threat-hunting roles.

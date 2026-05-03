# NIST CSF Incident Response Analysis
**Date:** 2026-05-02  
**Analyst:** marshall_c  

---

### Scenario Summary
A multimedia company providing web and graphic design services experienced a two-hour network blackout due to a Distributed Denial of Service (DDoS) attack. A malicious actor exploited an unconfigured firewall to launch an ICMP flood, overwhelming the internal network and rendering all services unresponsive. The response team mitigated the attack by blocking ICMP traffic, offlining non-critical services, and eventually restoring the environment through new firewall rules and monitoring systems.

---

### 1. IDENTIFY
The security team identified a malicious ICMP flood attack targeting the internal network. The attack exploited a vulnerability in an unconfigured firewall, causing a total loss of availability for all critical network resources and services.

### 2. PROTECT
To secure internal assets, the team implemented technical safeguards:
*   **Firewall Rate-Limiting:** A new rule was established to limit the frequency of incoming ICMP packets.
*   **Intrusion Prevention:** An IDS/IPS system was deployed to filter and drop traffic based on suspicious characteristics and known attack signatures.

### 3. DETECT
To improve detection speed and visibility, the following measures were integrated:
*   **Source Verification:** Configured the firewall to perform IP address verification to detect and block spoofed packets.
*   **Traffic Baselining:** Implemented network monitoring software to identify abnormal traffic patterns and trigger immediate alerts upon detection of volume spikes.

### 4. RESPOND
The response strategy for future incidents was formalized to include:
*   **System Isolation:** Immediate isolation of affected segments to contain the disruption.
*   **Log Analysis:** Post-incident review of network logs to identify the source and refine defense signatures.
*   **Reporting:** Escalation protocols were established to notify upper management and legal authorities as required.

### 5. RECOVER
Recovery focused on phased restoration to maintain business continuity:
*   **Prioritized Restoration:** Critical network services are brought online first to restore business-essential functions.
*   **Phased Re-entry:** Non-critical services are held offline until traffic stabilizes and the ICMP flood has timed out.
*   **Integrity Verification:** A final system audit is performed to ensure the network is in a known-good state before resuming full operations.

---

### Reflections / Notes
The root cause of this incident was an unconfigured firewall, emphasizing that even basic configuration gaps can lead to total service disruption. By moving beyond simple packet blocking and implementing rate-limiting, source verification, and IDS/IPS filtering, the organization has shifted from a reactive to a proactive security posture. This ensures that infrastructure is resilient enough to withstand future volumetric attacks while maintaining service availability.

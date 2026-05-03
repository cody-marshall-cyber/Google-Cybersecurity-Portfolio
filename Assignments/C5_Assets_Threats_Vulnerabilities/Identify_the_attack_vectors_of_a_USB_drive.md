# [SECURITY INVESTIGATION REPORT: C5-USB-BAITING]
**Analyst:** marshall_c  
**Date:** May 3, 2026  
**Case Reference:** [Rhetorical Hospital / Jorge Bailey Incident]  
**Control Alignment:** [NIST SP 800-53 (AC-6 / PL-4)]

---

#### **[CONTENTS]**
The recovered device contains a sensitive aggregation of **Personally Identifiable Information (PII)** and organizational data belonging to Jorge Bailey. Specifically, the drive holds personal family media alongside critical HR assets, such as new hire letters and employee shift schedules. This mixture of data represents a significant leak of the hospital's internal administrative structure and personnel records.

#### **[ATTACKER_MINDSET]**
An adversary could leverage the shift schedules to conduct **Social Engineering** or physical tailgating by identifying specific staff presence on-site. Furthermore, the personal photos and HR documents provide a high-value foundation for crafting targeted **Spear-Phishing** campaigns. By masquerading as a known contact found within the files, an attacker could manipulate Jorge into disclosing network credentials or executing malicious payloads.

#### **[RISK_ANALYSIS]**
To mitigate these risks, the hospital should implement **Managerial Controls** focused on specialized security awareness training regarding the "Left-Behind" USB threat. **Technical Controls** must be enforced through Group Policy to disable AutoRun/AutoPlay and restrict unauthorized USB mounting. Finally, an **Operational Control** should be established requiring all found media to be processed exclusively in isolated, air-gapped environments to prevent potential lateral movement across the production network.

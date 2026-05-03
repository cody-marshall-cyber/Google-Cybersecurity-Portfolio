# Access Control Investigation: Payroll Incident
**Date:** 2026-05-03  
**Analyst:** marshall_c  

---

### [INVESTIGATIVE SUMMARY]
An unauthorized payroll modification was detected involving a fraudulent bank deposit. The investigation focused on cross-referencing system event logs with the employee directory to identify the threat actor and exploit vector.

---

### [ACCESS CONTROL WORKSHEET]


| Category | Note(s) | Issue(s) | Recommendation(s) |
| :--- | :--- | :--- | :--- |
| **Authorization / Authentication** | • Event Date: 10/03/23.<br>• User: Legal/Administrator.<br>• IP Address: 152.207.255.255. | • Robert Taylor Jr. maintains active Admin privileges.<br>• Account accessed payroll systems in 2023 despite contract ending in 2019. | • Implement automated account expiration (e.g., 30-day cycles).<br>• Enforce Principle of Least Privilege (PoLP) for contractors.<br>• Enable Multi-Factor Authentication (MFA). |

---

### [EVIDENCE DATA]

**Event Log Detail:**
*   **Event ID:** 1227
*   **User:** Legal\Administrator
*   **Computer:** Up2-NoGud
*   **IP:** 152.207.255.255
*   **Description:** Payroll event added (FAUX_BANK).

**Directory Discrepancy:**
*   **Name:** Robert Taylor Jr. (Legal Attorney)
*   **Status:** Contractor (End Date: 12/27/2019)
*   **Authorization:** Admin (Excessive)

---

### [OPERATIONAL NOTES]
The primary failure was a total lack of **Lifecycle Management** and **Role-Based Access Control (RBAC)**. A terminated contractor retained high-level "Admin" permissions for nearly four years, allowing for unauthorized financial manipulation. Transitioning from "Admin-for-all" to granular, time-bound permissions is critical to hardening this environment.

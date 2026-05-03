# Data Leak Analysis: NIST SP 800-53 Compliance
**Date:** 2026-05-03  
**Analyst:** marshall_c  

---

### [INCIDENT SUMMARY]
An EdTech company experienced a sensitive data leak when internal business plans and customer analytics were posted to social media by an external partner. The breach occurred because a manager failed to revoke temporary folder access from a sales representative, who then inadvertently shared the entire internal directory instead of a specific marketing file during a sales call.

---

### [DATA LEAK WORKSHEET]


| Control | Issue(s) | Review (NIST SP 800-53: AC-6) |
| :--- | :--- | :--- |
| **Least Privilege** | Excessive permissions were maintained post-meeting. The representative had the authority to share parent directories externally, and the manager failed to implement a "need-to-know" restriction for the business partner. | AC-6 defines the principle of least privilege, requiring that users/processes are granted only the minimum access necessary for authorized tasks. It includes enhancements to improve the granularity and auditing of data access. |

---

### [RECOMMENDATIONS & JUSTIFICATION]

**Recommendations:**
1. **Role-Based Access Control (RBAC):** Restrict the ability to generate external sharing links to specific user roles and ensure internal folders are inaccessible to external accounts by default.
2. **Privileged Access Audits:** Implement regular, automated audits of user privileges to identify and revoke stagnant or excessive permissions.

**Justification:**
Restricting file-sharing capabilities based on user roles minimizes the impact of human error, ensuring that accidental link sharing does not expose parent directories. Furthermore, consistent auditing ensures that the "Least Privilege" posture remains current, closing security gaps left by temporary access grants before they can be exploited.

---

### [SECURITY PLAN SNAPSHOT]


| Function | Category | Subcategory | Reference |
| :--- | :--- | :--- | :--- |
| **PROTECT** | Data Security (PR.DS) | Protections against data leaks (PR.DS-5) | NIST SP 800-53: AC-6 |

---

### [OPERATIONAL NOTES]
This leak was a procedural failure rather than a technical exploit. The lack of a "revocation of access" protocol after the initial meeting created the vulnerability. Moving forward, the organization must automate the expiration of temporary permissions to align with NIST PR.DS-5 standards.

# Asset Classification: Home Office Network
**Date:** 2026-05-02  
**Analyst:** marshall_c  

---

### [ACTIVITY OVERVIEW]
Asset management is the foundational process of tracking assets and the risks that affect them. This activity focuses on creating an asset inventory for a home-based multimedia business to determine which devices contain sensitive information requiring enhanced protection.

---

### [ASSET INVENTORY]


| Asset | Network Access | Owner | Location | Notes | Sensitivity |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Network Router** | Continuous | ISP | On-premises | Dual-band (2.4 GHz & 5 GHz); connects all home devices. | Confidential |
| **Desktop** | Occasional | Homeowner | On-premises | Contains private information and personal photos. | Restricted |
| **Guest Smartphone** | Occasional | Friend | On/Off-premises | Connects to the home network periodically. | Internal-only |
| **External Hard Drive** | Occasional | Homeowner | On-premises | Primary storage for media (music and movies). | Confidential |
| **Streaming Media Player** | Continuous | Homeowner | On-premises | Stores payment card information for movie rentals. | Internal-only |
| **Portable Game Console** | Occasional | Friend | On/Off-premises | Includes active sensors (camera and microphone). | Internal-only |

---

### [CLASSIFICATION REFERENCE]


| Category | Access Designation | Definition |
| :--- | :--- | :--- |
| **Restricted** | Need-to-know | Highly sensitive; breach causes catastrophic impact. |
| **Confidential** | Limited to specific users | Sensitive information with controlled access. |
| **Internal-only** | Authorized users | Standard business/personal data. |
| **Public** | General access | No specific relationship or risk to integrity. |

---

### [OPERATIONAL NOTES]
Effective asset management allows a practitioner to prioritize security controls. In this scenario, the **Desktop** is classified as **Restricted** due to the presence of PII (Personally Identifiable Information). Identifying that the **Streaming Media Player** stores financial data (payment cards) highlights a potential vulnerability where a non-traditional IT device could lead to financial exfiltration if the network is compromised.

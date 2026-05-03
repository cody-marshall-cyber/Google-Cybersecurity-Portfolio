# Risk Assessment: Financial Institution Risk Register
**Date:** 2026-05-02  
**Analyst:** marshall_c  

---

### [OPERATIONAL ENVIRONMENT]
The organization is a commercial bank located in a coastal area with low physical crime rates. The environment consists of 100 on-premise and 20 remote employees managing 2,000 individual and 200 commercial accounts. Operations are subject to strict federal financial regulations requiring high availability and data integrity.

---

### [RISK REGISTER]
**Formula:** Likelihood (1-3) x Severity (1-3) = Priority Score (1-9)


| Asset | Risk(s) | Description | Likelihood | Severity | Priority |
| :--- | :--- | :--- | :---: | :---: | :---: |
| **Funds** | Business Email Compromise | Employee is social engineered into sharing credentials. | 2 | 2 | **4** |
| **Funds** | Compromised User Database | Customer data vulnerability due to poor encryption. | 2 | 3 | **6** |
| **Funds** | Financial Records Leak | Database server/backups are publicly accessible. | 3 | 3 | **9** |
| **Funds** | Theft | Physical security failure (bank safe left unlocked). | 1 | 3 | **3** |
| **Funds** | Supply Chain Disruption | Coastal natural disasters causing delivery delays. | 1 | 2 | **2** |

---

### [RISK MATRIX REFERENCE]


| Likelihood \ Severity | Low (1) | Moderate (2) | Catastrophic (3) |
| :--- | :---: | :---: | :---: |
| **Certain (3)** | 3 | 6 | **9** |
| **Likely (2)** | 2 | 4 | 6 |
| **Rare (1)** | 1 | 2 | 3 |

---

### [OPERATIONAL NOTES]
The bank's expansion through third-party marketing and a hybrid workforce significantly increases the digital attack surface. While physical theft (unlocked safe) is a high-severity event, the coastal location's low crime rate results in a lower overall priority compared to digital threats. 

The **Financial Records Leak** is the primary concern (Priority: 9). The high likelihood of exposure in a large data environment, combined with catastrophic regulatory fines and reputational damage, necessitates that resources be prioritized toward securing database access and hardening backup servers.

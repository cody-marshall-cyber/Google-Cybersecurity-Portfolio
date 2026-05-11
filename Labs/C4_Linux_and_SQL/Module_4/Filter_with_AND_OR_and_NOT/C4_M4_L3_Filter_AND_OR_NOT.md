# Lab Report: Filter with AND, OR, and NOT

## Scenario
**Objective:** responsible for filtering specific information from an organization's database to investigate potential security issues and identify machines that require updates. This involves querying login activity and employee data to isolate high-risk events and specific departmental needs.

---

### Task 1: Retrieve after hours failed login attempts
The objective is to isolate failed authentication events occurring after standard office hours (18:00) to identify potential brute-force activity during low-traffic periods.

**Query:**
```sql
SELECT * FROM log_in_attempts WHERE login_time > '18:00' AND success = 0;
```

**![Evidence](C4_M4_L3_Task1_Success_Logins)**
*Initial baseline audit: Retrieving successful logins after 18:00 to establish normal late-hour activity patterns.*

**![Evidence](C4_M4_L3_Task1_Failed_Logins)**
*Risk Identification: Isolating 19 unsuccessful attempts occurring after hours for further forensic scrutiny.*

**Technical Analysis:**
By utilizing the `AND` operator, the query successfully correlates two distinct security parameters: time-of-day and authentication failure. Filtering for `success = 0` (False) during non-business hours allows a security practitioner to bypass the "noise" of legitimate after-hours work and focus exclusively on high-risk telemetry. This precision is vital for detecting automated credential attacks that rely on the cover of darkness.

---

### Task 2: Retrieve login attempts on specific dates
The objective is to investigate a suspicious event by retrieving all authentication telemetry from the date of the incident ('2022-05-09') and the day immediately preceding it ('2022-05-08').

**Query:**
```sql
SELECT * FROM log_in_attempts WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```

**![Evidence](C4_M4_L3_Task2_Login_Dates_Initial)**
*Initial forensic sweep: Capturing the first segment of authentication attempts for the May 8th and May 9th window.*

**![Evidence](C4_M4_L3_Task2_Login_Dates_Full)**
*Complete dataset verification: Confirming 75 total records were retrieved for the 48-hour investigation period.*

**Technical Analysis:**
The use of the `OR` operator is critical in this context because an individual login event cannot exist on two dates at once. By applying `OR`, the query instructs the database to aggregate all records matching either specific date. This method ensures a comprehensive forensic timeline is established, allowing the security team to analyze both the primary event and any potential preparatory activity that occurred 24 hours prior.

---

### Task 3: Retrieve login attempts outside of Mexico
The objective is to isolate authentication attempts originating from outside of Mexico to investigate potential external or international unauthorized access.

**Query:**
```sql
SELECT * FROM log_in_attempts WHERE NOT country LIKE 'mex%';
```

**![Evidence](C4_M4_L3_Task3_Non_Mexico_Logins_Initial)**
*Initial geo-filter audit: The first attempt resulted in a syntax error (ERROR 1064) due to the omission of the NOT operator. The corrected query successfully began filtering out 'MEX' and 'MEXICO' entries.*

**![Evidence](C4_M4_L3_Task3_Non_Mexico_Logins_Full)**
*Global traffic verification: Confirmed 144 total records originating from countries other than Mexico, providing a targeted dataset for external threat hunting.*

**Technical Analysis:**
The use of the `NOT` operator combined with the `LIKE` operator and the `%` wildcard is a powerful forensic tool. By using `'mex%'`, the query accounts for data entry inconsistencies (e.g., both "MEX" and "MEXICO"). The initial syntax error documented in the evidence highlights that the `NOT` operator must precede the column and condition to properly negate the logic. This approach allows an analyst to quickly reduce "noise" from known safe regions and focus on anomalies from other geographical locations.

---

### Task 4: Retrieve employees in Marketing
The objective is to identify all employees within the Marketing department who are stationed in the East building to facilitate targeted computer updates.

**Query:**
```sql
SELECT * FROM employees WHERE department = 'Marketing' AND office LIKE 'East%';
```

**![Evidence](C4_M4_L3_Task4_Marketing_East_Building)**
*Targeted Audit: After an initial logic error (resulting in an empty set and 5 warnings), the corrected query successfully isolated 7 employees matching both the departmental and building location criteria.*

**Technical Analysis:**
The transition from the failed query to the successful one highlights a key SQL fundamental: column conditions must be explicit. The first attempt failed because it lacked a comparison operator for the 'department' column. By using `department = 'marketing'` and `AND office LIKE 'East%'`, the query effectively narrowed the scope to a specific subset of employees. Using the wildcard `%` after 'East' ensures that any room number within that wing is included in the update list.

---

### Task 5: Retrieve employees in Finance or Sales
The objective is to gather comprehensive records for all personnel within the Finance and Sales departments to execute specific computer updates.

**Query:**
```sql
SELECT * FROM employees WHERE department = 'Finance' OR department = 'Sales';
```

**![Evidence](C4_M4_L3_Task5_Finance_Sales_Initial)**
*Initial Multi-Department Audit: Successfully initiating the retrieval of employee data for both Finance and Sales units using the OR operator.*

**![Evidence](C4_M4_L3_Task5_Finance_Sales_Full)**
*Update List Verification: Confirming a final count of 71 employees requiring the specific departmental update.*

**Technical Analysis:**
The `OR` operator is the primary tool for expanding the scope of a query to include multiple independent criteria. Because an employee's department is a single value, an `AND` operator would return zero results (as an employee cannot be in two departments at once). This task reinforces the necessity of repeating the column name (`department`) for each condition, ensuring the SQL engine treats each department as a valid, independent match for the final result set.

---

### Task 6: Retrieve all employees not in IT
The objective is to identify all employees outside of the Information Technology department to finalize a staged organization-wide computer update.

**Query:**
```sql
SELECT * FROM employees WHERE NOT department LIKE 'Information Technology';
```

**![Evidence](C4_M4_L3_Task6_Non_IT_Employees_Initial)**
*Exclusion Audit: The initial phase was hindered by syntax errors, including a typo in the FROM keyword ("fome"). Once corrected, the NOT operator successfully began isolating non-IT personnel.*

**![Evidence](C4_M4_L3_Task6_Non_IT_Employees_Full)**
*Final Rollout Verification: Confirmed 161 total records for employees in all departments except Information Technology, completing the data gathering for the update cycle.*

**Technical Analysis:**
The `NOT` operator is highly efficient for excluding a specific, known data group. In this scenario, because the IT department had already been updated, filtering for everyone except that group prevents redundant work. The initial syntax errors serve as a critical reminder that SQL is literal; a simple misspelling like "fome" instead of "FROM" will stall an entire investigation. Successful execution requires precise command structure and careful negation of the target criteria.


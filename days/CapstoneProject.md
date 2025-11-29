## SQL Murder Mystery — “Who Killed the CEO?”

#### A 21-day SQL Challenge Capstone Project

---



## 📌 Overview

This project is the capstone challenge for my 21-day SQL Challenge.

The CEO of TechNova Inc. was found dead on 15 October 2025 at 9:00 PM in suspicious circumstances.
My task was to solve the entire mystery using SQL, by analyzing:

- Keycard swipe logs

- Employees database

- Phone call records

- Alibi claims

- Physical evidence


##  Dataset

The database contains the following tables:

- employees — ID, name, role, department

- keycard_logs — entry/exit logs for each room

- calls — call events with timestamps and duration

- alibis — claimed location/time by employees

- evidence — physical evidence found in specific rooms

All data was imported using the supplied SQL dump file.


## Investigation Flow

I followed the investigation structure given with the capstone.
Each step had a clear objective and SQL concepts to apply.

###  Step 1 - Identify Crime Scene & Time


- **Objective: Use the evidence table to confirm where the murder happened.**

#### I ran:
```sql
SELECT room, description, found_time FROM evidence ORDER BY found_time;
```
#### output:

| **room**      | **description**                 | **found_time**          |
|---------------|---------------------------------|--------------------------|
| CEO Office    | Fingerprint on desk             | 2025-10-15 21:05:00      |
| CEO Office    | Keycard swipe logs mismatch     | 2025-10-15 21:10:00      |
| Server Room   | Unusual access pattern          | 2025-10-15 21:15:00      |

- This confirmed the crime room and the approximate time window.


### Step 2 - Find Who Accessed the Crime Scene

- **Objective: Identify all employees who entered the crime room around the time of the murder.**

#### I ran:
```sql
SELECT e.name, k.* FROM keycard_logs k JOIN employees e ON k.employee_id = e.employee_id 
WHERE k.room = 'CEO Office' AND k.entry_time BETWEEN '2025-10-15 20:45' AND '2025-10-15 21:10';
```

#### output:

| **name**      | **log_id** | **employee_id** | **room**     | **entry_time**          | **exit_time**           |
|---------------|------------|------------------|--------------|--------------------------|--------------------------|
| David Kumar   | 11         | 4                | CEO Office   | 2025-10-15 20:50:00      | 2025-10-15 21:00:00      |

- This gave me the first set of suspects.


### Step 3 - Verify Alibis

- **Objective: Check who lied about their whereabouts.**

#### I compared alibi claims with actual logs:
```sql
SELECT a.employee_id, e.name, a.claimed_location, a.claim_time FROM alibis a 
JOIN employees e ON a.employee_id = e.employee_id;
```

#### output:

| **employee_id** | **name**        | **claimed_location** | **claim_time**          |
|------------------|------------------|------------------------|--------------------------|
| 1                | Alice Johnson    | Office                 | 2025-10-15 20:50:00      |
| 4                | David Kumar      | Server Room            | 2025-10-15 20:50:00      |
| 5                | Eva Brown        | Marketing Office       | 2025-10-15 20:50:00      |
| 6                | Frank Li         | Office                 | 2025-10-15 20:50:00      |

#### Then I cross-checked logs:
```sql
SELECT * FROM keycard_logs WHERE employee_id = 4 AND DATE(entry_time) = '2025-10-15';
```

#### output:

| **log_id** | **employee_id** | **room**       | **entry_time**          | **exit_time**           |
|------------|-----------------|----------------|-------------------------|-------------------------|
| 4          | 4               | Server Room    | 2025-10-15 08:50:00     | 2025-10-15 09:10:00     |
| 11         | 4               | CEO Office     | 2025-10-15 20:50:00     | 2025-10-15 21:00:00     |

- This revealed false alibis.


### Step 4 - Analyze Suspicious Calls

- **Objective: Check who communicated right before the murder.**

#### I filtered calls around the murder time:
```sql
SELECT c.*, e.name AS caller_name, r.name AS receiver_name FROM calls c 
JOIN employees e ON c.caller_id = e.employee_id JOIN employees r ON c.receiver_id = r.employee_id 
WHERE c.call_time BETWEEN '2025-10-15 20:50' AND '2025-10-15 21:00';
```
#### output:

| **call_id** | **caller_id** | **receiver_id** | **call_time**          | **duration_sec** | **caller_name** | **receiver_name** |
|-------------|---------------|----------------|------------------------|-----------------|----------------|------------------|
| 1           | 4             | 1              | 2025-10-15 20:55:00    | 45              | David Kumar    | Alice Johnson    |

- This identified suspicious communication patterns.


### Step 5 - Match Evidence With Movement

- **Objective: Link physical evidence with movements & lies.**

- I checked room access vs. where evidence was found.
- I checked if anyone was in multiple suspicious rooms.
- I checked inconsistencies between calls, logs, and alibis.

This helped narrow down suspects.

### Step 6 - Final Killer Identification

- **Objective: Combine all findings into a single SQL query.**
```sql
SELECT e.name AS killer FROM employees e WHERE e.employee_id IN
(SELECT employee_id FROM keycard_logs WHERE room = 'CEO Office' AND entry_time BETWEEN '2025-10-15 20:45' AND '2025-10-15 21:10')
AND e.employee_id IN
( SELECT caller_id FROM calls WHERE call_time BETWEEN '2025-10-15 20:50' AND '2025-10-15 21:00'
    UNION SELECT receiver_id FROM calls WHERE call_time BETWEEN '2025-10-15 20:50' AND '2025-10-15 21:00' )
AND e.employee_id IN
( SELECT a.employee_id FROM alibis a LEFT JOIN keycard_logs k ON a.employee_id = k.employee_id AND DATE(a.claim_time) = DATE(k.entry_time)
    WHERE a.claimed_location <> k.room )
AND e.employee_id IN
( SELECT DISTINCT k.employee_id FROM keycard_logs k WHERE k.room IN (SELECT room FROM evidence) );
```

## Final Result
| **killer**    |
|---------------|
| David Kumar   |

---

## What I Learned

- Building queries step-by-step instead of jumping to conclusions

- Using JOINs, subqueries, and filtering intelligently

- Cross-checking data from multiple tables

- Solving a real narrative puzzle with SQL



## Final Note

This was the most fun and analytical challenge of the SQL bootcamp.
The mystery forces you to think logically, connect tables, and verify every assumption with SQL.

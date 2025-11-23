## Day 18 – 21 Days SQL Challenge

---


🧠 Topic: Combining Query Results Using UNION

**Key Concepts**

- UNION removes duplicates → slower

- UNION ALL keeps duplicates → faster

**Both require:**

- Same number of columns

- Same data types

- Column names from the first SELECT are used

- ORDER BY applies after all UNION parts finish

- We can use literal values like 'Patient' or 'Staff' to identify sources

---


## Practice Questions

**1. Combine patient names and staff names into a single list.**
```sql
SELECT name AS full_name FROM patients UNION SELECT staff_name AS full_name FROM staff;
```


**2. Create a union of high satisfaction (>90) and low satisfaction (<50) patients.**
```sql
SELECT patient_id, name, satisfaction, 'High' AS category FROM patients WHERE satisfaction > 90 UNION ALL
SELECT patient_id, name, satisfaction, 'Low' AS category FROM patients WHERE satisfaction < 50;
```

**3. List all unique names from both patients and staff tables.**
```sql
SELECT name AS full_name FROM patients UNION SELECT staff_name AS full_name FROM staff;
```


## Daily Challenge
**Question:
Create a comprehensive personnel and patient list showing: identifier (patient_id or staff_id), full name, type ('Patient' or 'Staff'), and associated service. Include only those in 'surgery' or 'emergency' services. Order by type, then service, then name.**

```**My Approach..
~ Step 1 — Identify the two tables
We need both:
            -patients
            -staff
~ Step 2 — Match columns
Both SELECTs must output 4 columns:
                            -identifier
                            -full_name
                            -type
                            -service

~ Step 3 — Add filters (WHERE service IN …)
~ Step 4 — UNION ALL (We want a combined list, duplicates not an issue.)
~ Step 5 — ORDER BY (at the end only)
```
```sql
SELECT patient_id AS identifier, name AS full_name, 'Patient' AS type, service FROM patients WHERE service IN ('emergency', 'surgery')
UNION ALL
SELECT staff_id AS identifier, staff_name AS full_name, 'Staff' AS type, service FROM staff WHERE service IN ('emergency', 'surgery')
ORDER BY type, service, full_name;
```

## Output:

| **identifier** | **full_name**           | **type**  | **service** |
|----------------|--------------------------|-----------|-------------|
| PAT-76b1239a   | Adam Taylor              | Patient   | surgery     |
| PAT-4428a64a   | Alexis Harris            | Patient   | surgery     |
| PAT-0a87540a   | Alicia Roth              | Patient   | surgery     |
| PAT-32bcc4bc   | Amber Cooper             | Patient   | surgery     |
| PAT-8029775b   | Amber Wright             | Patient   | surgery     |
| PAT-51264ef0   | Amy Ramirez              | Patient   | surgery     |

--(more rows)--


---

## Reflection

Today I learned how UNION and UNION ALL help combine results from multiple SELECT queries into a single dataset. This is useful when data exists in different tables (patients, staff, services) and we want a unified view without joins
I learned how to:

- Combine datasets without joins

- Label rows using fixed string values

- Ensure column alignment across multiple SELECT queries

- Decide when to use UNION vs UNION ALL

- Apply ORDER BY correctly after a UNION chain

The challenge question especially helped me think in terms of:

- Structuring outputs

- Matching column types

- Filtering before combining

- Maintaining readable, well-organized SQL


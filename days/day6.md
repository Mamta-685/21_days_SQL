# Day 6 - 21 Days SQL Challenge

---

## 🧠 Practice Questions

**1. Count the number of patients by each service.**
```sql
SELECT service, COUNT(*) AS service_count 
FROM patients 
GROUP BY service;

```

**2. Calculate the average age of patients grouped by service.**
```sql
SELECT service, ROUND(AVG(age), 2) AS avg_age 
FROM patients 
GROUP BY service;
```

**3. Find the total number of staff members per role.**
```sql
SELECT role, COUNT(*) AS total_staff 
FROM staff 
GROUP BY role;
```

## Challenge Question
**Question:
For each hospital service, calculate the total number of patients admitted, total patients refused, and the admission rate (percentage of requests that were admitted).
Order by admission rate descending.**
```sql
SELECT 
    service, 
    SUM(patients_admitted) AS total_p_admitted, 
    SUM(patients_refused) AS total_p_refused, 
    ROUND(SUM(patients_admitted) * 100.0 / SUM(patients_request), 2) AS admission_rate
FROM services_weekly
GROUP BY service
ORDER BY admission_rate DESC;
```

## Output

| **service**        | **total_p_admitted** | **total_p_refused** | **admission_rate** |
|---------------------|----------------------|---------------------|--------------------|
| ICU                 | 648                  | 141                 | 82.00              |
| Surgery             | 1686                 | 555                 | 75.00              |
| General_Medicine    | 2332                 | 1938                | 54.00              |
| Emergency           | 1185                 | 5008                | 19.00              |


## Reflection

Today’s challenge helped me connect aggregate functions like SUM(), AVG(), and COUNT() with real analytical use cases.
Understanding how GROUP BY works alongside these functions gave me a clear view of how to summarize and compare data between categories.

I also learned the importance of calculated fields like the admission rate — turning raw data into meaningful insights.

@indiandataclub
#DPDzero
#IDCWithSQL

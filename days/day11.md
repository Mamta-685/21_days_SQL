## Day 11 — 21 Days SQL Challenge

---
##  Concepts Learned

Today was all about extracting unique values using DISTINCT, and understanding when you need to use it with multiple columns.
Also learned how to combine filtering + grouping to get unique combinations.


## Practice Questions

**1. List all unique services in the patients table.**
```sql
SELECT DISTINCT service
FROM patients;
```

**2. Find all unique staff roles in the hospital.**
```sql
SELECT DISTINCT role
FROM staff;
```

**3. Get distinct months from the services_weekly table.**
```sql
SELECT DISTINCT month
FROM services_weekly;
```

## Daily Challenge
**Question:
Find all unique combinations of service and event from the services_weekly table where events are NOT NULL and not 'none', along with the count of occurrences for each combination. Order by count descending.**
```sql
SELECT service, event, COUNT(*) AS combinations
FROM services_weekly
WHERE event IS NOT NULL 
  AND event != 'none'
GROUP BY service, event
ORDER BY combinations DESC;
```

## Output:
| **service**        | **event**  | **combinations** |
|--------------------|------------|------------------|
| general_medicine   | flu        | 6                |
| ICU                | flu        | 5                |
| surgery            | donation   | 5                |
| emergency          | flu        | 5                |
| surgery            | strike     | 4                |
| emergency          | strike     | 4                |
| emergency          | donation   | 4                |
| general_medicine   | donation   | 3                |
| general_medicine   | strike     | 3                |
| surgery            | flu        | 3                |
| ICU                | donation   | 2                |


---


🧠 Reflection — What I Messed Up & What I Fixed

I tried to use COUNT(DISTINCT service, event) but SQL doesn’t allow DISTINCT on multiple columns like that.
→ Learned that I must GROUP BY service, event instead.

I didn’t filter out 'none' because I treated it like NULL.
→ Finally understood that 'none' is just text, so I must explicitly exclude it.


I was unsure about the month column in my table.
→ Realized that overthinking column names causes silly errors; I just needed SELECT DISTINCT month.

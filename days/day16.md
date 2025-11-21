## Day 16 — 21 Days SQL Challenge

---

## Practice Questions

**1. Find patients who are in services with above-average staff count.**
**My approach:**

- Count staff per service

- Find the average staff-per-service

- Select services above that average

- Return patients from those services

```sql
SELECT p.patient_id, p.name AS patient_name, p.service FROM patients p
WHERE p.service IN (
    SELECT service FROM (
        SELECT service, COUNT(staff_id) AS staff_count
        FROM staff
        GROUP BY service
    ) s WHERE staff_count > (
        SELECT AVG(staff_per_service) FROM (
            SELECT COUNT(staff_id) AS staff_per_service FROM staff GROUP BY service
        ) a
    )
);
```

**2. List staff who work in services that had any week with patient satisfaction below 70.**
**My approach:**

- Identify services where any week has satisfaction < 70

- Select staff whose service is in that list

```sql
SELECT s.staff_id, s.staff_name, s.service FROM staff s WHERE s.service IN (
    SELECT DISTINCT service FROM services_weekly WHERE patient_satisfaction < 70
);
```

**3. Show patients from services where total admitted patients exceed 1000
My approach:**

- Sum admitted patients per service

- Filter services with total_admitted > 1000

- Return patients in those services

```sql
SELECT p.patient_id, p.name AS patient_name, p.service FROM patients p WHERE p.service IN (
    SELECT service FROM services_weekly GROUP BY service HAVING SUM(patients_admitted) > 1000
);
```


## Daily Challenge
**Question:
Find all patients who were admitted to services that had at least one week where patients were refused AND the average patient satisfaction for that service was below the overall hospital average satisfaction. Show patient_id, name, service, and their personal satisfaction score.**

**My Approach:**
- Step 1: Services with any patients refused
```sql
SELECT service FROM services_weekly WHERE patients_refused > 0;
```

- Step 2: Overall hospital average satisfaction
```sql
SELECT AVG(patient_satisfaction) FROM services_weekly;
```

- Step 3: Services whose avg satisfaction < overall avg
```sql
SELECT serviceFROM services_weekly GROUP BY service HAVING AVG(patient_satisfaction) < (
    SELECT AVG(patient_satisfaction) FROM services_weekly
);
```
- Step 4: Final — patients from services meeting BOTH conditions
```sql
SELECT p.patient_id, p.name AS patient_name, p.service, p.satisfaction FROM patients p
WHERE p.service IN (
    SELECT service
    FROM services_weekly
    WHERE patients_refused > 0
)
AND p.service IN (
    SELECT service FROM services_weekly GROUP BY service
    HAVING AVG(patient_satisfaction) <
           (SELECT AVG(patient_satisfaction) FROM services_weekly)
);
```

## Output:
| **patient_id**  | **patient_name**     | **service** | **satisfaction** |
|-----------------|-----------------------|-------------|------------------|
| PAT-09484753    | Richard Rodriguez     | surgery     | 61               |
| PAT-3dda2bb5    | Crystal Johnson       | emergency   | 81               |
| PAT-283cda07    | William Herrera       | emergency   | 66               |
| PAT-13c9b20f    | Robert Potter         | surgery     | 71               |
| PAT-64a781e0    | Jamie Walton          | surgery     | 66               |

... (more rows)

---


## Reflection 

Today was one of the most logically tricky days so far.
Everything depended on combining multiple layers of conditions inside subqueries, and using them together correctly.
The real challenge was breaking the question into layers:

- service-level conditions

- overall averages

- combining conditions

- and finally mapping them back to patients

Initially it felt confusing, but once I separated the logic step-by-step, everything made sense.
This day made me more confident about multi-condition, nested-query problems.

Slow, clear thinking > rushing.
I’m starting to see SQL as structured logic rather than syntax, which honestly feels good.





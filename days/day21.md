## Day 21 — 21 Days SQL Challenge

---


## Practice Questions 

**1. Create a CTE to calculate service statistics, then query from it.**
```sql
WITH service_stats AS ( SELECT service, COUNT(*) AS patient_count, AVG(satisfaction) AS avg_satisfaction
    FROM patients GROUP BY service)
SELECT * FROM service_stats WHERE avg_satisfaction > 75 ORDER BY patient_count DESC;
```

**2. Use multiple CTEs to break down a complex query into logical steps.**
```sql
WITH patient_counts AS ( SELECT service, COUNT(*) AS total_patients FROM patients GROUP BY service),
service_satisfaction AS ( SELECT service, AVG(satisfaction) AS avg_satisfaction FROM patients GROUP BY service )
SELECT pc.service, pc.total_patients, ss.avg_satisfaction
FROM patient_counts pc JOIN service_satisfaction ss ON pc.service = ss.service;
```

**3. Build a CTE for staff utilization and join it with patient data.**
```sql
WITH staff_util AS ( SELECT s.service, COUNT(s.staff_id) AS total_staff, ROUND(AVG(ss.weeks_present), 2) AS avg_weeks_present FROM staff s
    LEFT JOIN ( SELECT staff_id , SUM(present) AS weeks_present FROM staff_schedule GROUP BY staff_id) ss
    ON s.staff_id = ss.staff_id GROUP BY s.service)
SELECT p.service, COUNT(p.patient_id) AS total_patients, su.total_staff, su.avg_weeks_present
FROM patients p JOIN staff_util su ON p.service = su.service GROUP BY p.service, su.total_staff, su.avg_weeks_present;
```


## Daily Challenge

**Question:
Create a comprehensive hospital performance dashboard using CTEs. Calculate: 1) Service-level metrics (total admissions, refusals, avg satisfaction), 2) Staff metrics per service (total staff, avg weeks present), 3) Patient demographics per service (avg age, count). Then combine all three CTEs to create a final report showing service name, all calculated metrics, and an overall performance score (weighted average of admission rate and satisfaction). Order by performance score descending.**

### My step-by-step approach: 
**Step 1 — Break the dashboard into 3 CTEs**

    I first separated the problem into three parts:
      -service metrics (admissions, refusals, avg satisfaction),
      -staff metrics (total staff + weeks_present calculated using a subquery),
      -patient demographics (count + avg age).
    Writing them separately helped avoid confusion and column errors.

**Step 2 — Fix missing columns by computing them first**

    For staff metrics, I realized weeks_present didn’t exist.
    So I created a small subquery that calculated: SUM(present) AS weeks_present per staff_id, then used AVG() on that inside the CTE.

**Step 3 — Combine the CTEs one-by-one**

    Instead of joining all 3 at once, I joined service_metrics → staff_metrics first, checked output, then added patient_demographics. 
    This ensured no service got dropped and the JOIN keys were correct.

**Step 4 — Compute the performance_score with proper casting**

    PostgreSQL gave an error because ROUND() needs numeric.
    So I cast the weighted formula to numeric: ROUND((...weighted_formula...)::numeric, 3)
    This fixed the rounding issue.

**Step 5 — Final SELECT + Order by score**

After joining all CTEs successfully, I added the performance_score and ordered the final result by it in descending order. 

```sql
WITH service_metrics AS (SELECT  service, SUM(patients_admitted) AS total_admitted, SUM(patients_refused) AS total_refused,
        ROUND(AVG(patient_satisfaction), 2) AS avg_satisfaction FROM services_weekly GROUP BY service ),
staff_metrics AS ( SELECT s.service, COUNT(s.staff_id) AS total_staff, ROUND(AVG(ss.weeks_present), 2) AS avg_weeks_present
    FROM staff s LEFT JOIN ( SELECT staff_id, SUM(present) AS weeks_present
        FROM staff_schedule GROUP BY staff_id ) ss ON s.staff_id = ss.staff_id GROUP BY s.service ),
patient_demographics AS ( SELECT service, COUNT(*) AS total_patients, ROUND(AVG(age), 2) AS avg_age FROM patients GROUP BY service)
SELECT sm.service, sm.total_admitted, sm.total_refused, sm.avg_satisfaction, stm.total_staff, stm.avg_weeks_present, pd.total_patients, pd.avg_age,
    ROUND((
            (0.6 * (sm.total_admitted::float / (sm.total_admitted + sm.total_refused)))
            +
            (0.4 * (sm.avg_satisfaction / 100))
        )::numeric, 3 ) AS performance_score
FROM service_metrics sm JOIN staff_metrics stm ON sm.service = stm.service JOIN patient_demographics pd ON sm.service = pd.service ORDER BY performance_score DESC;
```
## Output:
| **service**        | **total_admitted** | **total_refused** | **avg_satisfaction** | **total_staff** | **avg_weeks_present** | **total_patients** | **avg_age** | **performance_score** |
|--------------------|--------------------|--------------------|-----------------------|------------------|------------------------|---------------------|-------------|------------------------|
| ICU                | 648                | 141                | 81.62                 | 48               | 31.19                  | 241                 | 44.22       | 0.819                  |
| surgery            | 1686               | 555                | 79.27                 | 22               | 31.68                  | 254                 | 45.59       | 0.768                  |
| general_medicine   | 2332               | 1938               | 81.23                 | 27               | 30.67                  | 242                 | 46.19       | 0.653                  |
| emergency          | 1185               | 5008               | 77.88                 | 29               | 31.31                  | 263                 | 45.33       | 0.426                  |


##  What I Struggled With

- I got confused about weeks_present because that column doesn’t exist naturally.

- I initially wrote invalid nested queries.

- I missed that I must compute weeks_present using SUM(present) BEFORE using it.

- Joining multiple CTEs felt overwhelming at first.


##  How I Improved Today

- Understood importance of naming CTEs clearly

- Finally understood weighted scoring formulas

- Realized why CTEs are better than nested subqueries in analytics

- Improved accuracy in writing JOINs and aggregations

## Reflection

Today felt like a major step forward in writing clean, analytical SQL.
CTEs helped me think more structurally and avoid messy nested queries.
I also learned to handle computed metrics (like weeks_present) using subqueries inside CTEs.
The daily challenge pushed me to design a full analytical pipeline — and I completed it fully.

It was a productive day. 

## Day 20 — 21 Days SQL Challenge

---


### Key Concepts Learned
    ✔ 1. Aggregate Window Functions
          SUM(column) OVER (...)
          AVG(column) OVER (...)
          MIN(column) OVER (...)
          MAX(column) OVER (...)
          COUNT(*) OVER (...)

      - They work like normal aggregates but operate row by row.

    ✔ 2. Window Frame Clause (ROWS BETWEEN ... AND ...)
            UNBOUNDED PRECEDING → start from first row
            N PRECEDING → window includes N rows before
            CURRENT ROW → include the current row
            N FOLLOWING → look ahead
            UNBOUNDED FOLLOWING → go to last row

    ✔ 3. Important default behavior:

          When using ORDER BY inside OVER, SQL automatically applies:
            ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
          So: 
            SUM(x) OVER (ORDER BY week) = running total.

    ✔ 4. Partitioning Logic

        With PARTITION BY service → resets per service
        Without ORDER BY → entire partition is used (not cumulative)

---


## Practice Questions

**1. Calculate running total of patients admitted by week for each service.**
```sql
SELECT service, week, patients_admitted,
    SUM(patients_admitted) OVER ( PARTITION BY service ORDER BY week ) AS running_total FROM services_weekly;
```

**2. Find the moving average of patient satisfaction over 4-week periods.**
```sql
SELECT service, week, patient_satisfaction, ROUND( AVG(patient_satisfaction) OVER ( PARTITION BY service ORDER BY week
 ROWS BETWEEN 3 PRECEDING AND CURRENT ROW), 2) AS moving_avg_4week
FROM services_weekly;
```

**3. Show cumulative patient refusals by week across all services.**
```sql
SELECT week, patient_refusals, SUM(patient_refusals) OVER ( ORDER BY week ) AS cumulative_refusals
FROM services_weekly;
```


## Daily Challenge Question

**Question: 
Create a trend analysis showing for each service and week: week number, patients_admitted, running total of patients admitted (cumulative), 3-week moving average of patient satisfaction (current week and 2 prior weeks), and the difference between current week admissions and the service average. Filter for weeks 10-20 only.**
```sql
SELECT service, week, patients_admitted, patient_satisfaction, 
    SUM(patients_admitted) OVER ( PARTITION BY service ORDER BY week ) AS cumulative_admissions,
    ROUND( AVG(patient_satisfaction) OVER ( PARTITION BY service ORDER BY week
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW ), 2 ) AS moving_avg_3week,
    patients_admitted - AVG(patients_admitted) OVER (PARTITION BY service) AS diff_from_service_avg
FROM services_weekly WHERE week BETWEEN 10 AND 20 ORDER BY service, week;
```

---


## Learning

- Window functions don’t reduce rows—they enhance data.

- PARTITION BY acts like “grouping that doesn't collapse rows”.

- Moving averages need both ORDER BY and ROWS BETWEEN.

- Window functions can be used inside arithmetic expressions.

- The difference between:

  - A regular aggregate
    AVG(x)

  - A window aggregate
    AVG(x) OVER (...)

- Filtering normal columns ≠ filtering window functions (no subquery needed unless using window output).


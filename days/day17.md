## Day 17 — 21 Days SQL Challenge

---



##  What I Learned Today

Today was about understanding subqueries inside SELECT and subqueries inside FROM.
These two ideas sound similar, but they behave very differently:

**1. Subquery in SELECT = calculate something for each row**

This is like adding an extra column that SQL calculates on the fly.

Example thinking:

 - For each patient → show service average satisfaction

 - For each staff → show service total patients

 - For each service → show deviation from average

These subqueries must return one single value.

**2. Subquery in FROM = create a temporary table**

- This is called a derived table.

- It's useful when you need:

      - intermediate calculations

      - cleaner structure

      - things that would be messy in one big query

- Everything inside the subquery becomes a new table that you can query normally.

---


## Practice Questions

**1. Show each patient with their service’s average satisfaction**
```sql
SELECT p.patient_id, p.name, p.service,
    (
        SELECT AVG(satisfaction)
        FROM patients p2
        WHERE p2.service = p.service
    ) AS service_avg_satisfaction FROM patients p;
```

**2. Create a derived table of service statistics**
```sql
SELECT stats.service, stats.total_admitted, stats.avg_satisfaction
FROM ( SELECT service, SUM(patients_admitted) AS total_admitted, AVG(patient_satisfaction) AS avg_satisfaction FROM services_weekly
    GROUP BY service
) AS stats;
```

**3. Display staff with their service's total patient count.**
```sql
SELECT s.staff_id, s.staff_name, s.service,
    ( SELECT SUM(patients_admitted) FROM services_weekly sw
        WHERE sw.service = s.service
    ) AS service_total_admitted FROM staff s;
```

## Daily Challenge

**Question:
Create a report showing each service with: service name, total patients admitted, the difference between their total admissions and the average admissions across all services, and a rank indicator ('Above Average', 'Average', 'Below Average'). Order by total patients admitted descending.**
```sql
SELECT stats.service, stats.total_admitted, stats.total_admitted - stats.avg_total AS diff_from_avg,
    CASE
        WHEN stats.total_admitted > stats.avg_total THEN 'Above Average'
        WHEN stats.total_admitted = stats.avg_total THEN 'Average'
        ELSE 'Below Average' END AS rank_indicator
FROM (
    SELECT service, SUM(patients_admitted) AS total_admitted,
        ( SELECT ROUND(AVG(total_admitted), 2)  FROM (
                SELECT service, SUM(patients_admitted) AS total_admitted FROM services_weekly GROUP BY service
            ) AS sub
        ) AS avg_total FROM services_weekly GROUP BY service
) AS stats ORDER BY stats.total_admitted DESC;
```

## Output:

| **service**        | **total_admitted** | **diff_from_avg** | **rank_indicator** |
|--------------------|--------------------|--------------------|---------------------|
| general_medicine   | 2332               | 869.25             | Above Average       |
| surgery            | 1686               | 223.25             | Above Average       |
| emergency          | 1185               | -277.75            | Below Average       |
| ICU                | 648                | -814.75            | Below Average       |
--(more rows)

---


## Reflection

- Today was a little tricky at first — the examples looked heavy because of the nested queries.
But the moment I broke it into steps, things felt much more manageable:

- derived tables are just “intermediate results”

- subqueries in SELECT behave like mini-functions that run per row

- the key is understanding what must return one value and what returns a table

- I also noticed that when I try to jump straight into writing a big query, I get stuck.
But when I write the smaller steps first, the final query becomes obvious.

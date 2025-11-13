# Day 7 - 21 Days SQL Challenge


---

## 🧠 Practice Questions

 **1. Find services that have admitted more than 500 patients in total.**
```sql
SELECT service, SUM(patients_admitted) AS total_admitted
FROM services_weekly
GROUP BY service
HAVING SUM(patients_admitted) > 500;
```

**2. Show services where average patient satisfaction is below 75.**
```sql
SELECT service, ROUND(AVG(satisfaction), 2) AS avg_satisfaction
FROM patients
GROUP BY service
HAVING ROUND(AVG(satisfaction), 2) < 75;
```

**3. List weeks where total staff presence across all services was less than 50.**
```sql
SELECT week, COUNT(staff_id) AS total_staff_presence
FROM staff_schedule
GROUP BY week
HAVING COUNT(staff_id) < 50
ORDER BY week;
```


## ⚡ Challenge Question
**Question: Identify services that refused more than 100 patients in total and had an average patient satisfaction below 80. Show service name, total refused, and average satisfaction.**
```sql
SELECT service,
       SUM(patients_refused) AS total_refused,
       ROUND(AVG(patient_satisfaction), 2) AS avg_satisfaction
FROM services_weekly
GROUP BY service
HAVING SUM(patients_refused) > 100
   AND ROUND(AVG(patient_satisfaction), 2) < 80;
```

## 📊 Output
| **service**  | **total_refused** | **avg_satisfaction** |
|--------------|------------------|---------------------|
| surgery      | 555              | 79.27               |
| emergency    | 5008             | 77.88               |

---

## Reflection
Today’s focus on HAVING clauses and group-level filtering helped me understand the difference between filtering rows (WHERE) and filtering aggregated groups (HAVING).

I also realized the importance of choosing the right aggregation function (SUM vs COUNT) depending on the question.
This practice strengthened my confidence in analyzing data per service or week, which is exactly how real-world SQL queries are used to generate insights.



@indiandataclub
@DPDzero
#IDCWithSQL

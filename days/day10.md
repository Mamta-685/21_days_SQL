# 📅 Day 10 – SQL 21 Days Challenge  

---

## 🧩 Practice Questions

**1. Categorize patients as 'High', 'Medium', or 'Low' satisfaction based on their scores.**

```sql
SELECT name, satisfaction,
    CASE 
        WHEN satisfaction > 80 THEN 'High'
        WHEN satisfaction BETWEEN 50 AND 80 THEN 'Medium'
        ELSE 'Low'
    END AS satisfaction_level
FROM patients;
```

**2. Label staff roles as 'Medical' or 'Support' based on role type.**
```sql
SELECT staff_id, staff_name, role,
    CASE 
        WHEN role_type IN ('Doctor', 'Nurse', 'Surgeon') THEN 'Medical'
        ELSE 'Support'
    END AS role_category
FROM staff;
```

**3. Create age groups for patients (0–18, 19–40, 41–65, 65+).**
```sql
SELECT name, age,
    CASE 
        WHEN age BETWEEN 0 AND 18 THEN '0-18'
        WHEN age BETWEEN 19 AND 40 THEN '19-40'
        WHEN age BETWEEN 41 AND 65 THEN '41-65'
        ELSE '65+'
    END AS age_group
FROM patients;
```


## Daily Challenge
**Question:
Question: Create a service performance report showing service name, total patients admitted, and a performance category based on the following: 'Excellent' if avg satisfaction >= 85, 'Good' if >= 75, 'Fair' if >= 65, otherwise 'Needs Improvement'. Order by average satisfaction descending.**

```sql
SELECT service, SUM(patients_admitted) AS total_patients_admitted, ROUND(AVG(patient_satisfaction), 2) AS avg_satisfaction,
    CASE 
        WHEN AVG(patient_satisfaction) >= 85 THEN 'Excellent'
        WHEN AVG(patient_satisfaction) >= 75 THEN 'Good'
        WHEN AVG(patient_satisfaction) >= 65 THEN 'Fair'
        ELSE 'Needs Improvement'
    END AS performance_category
FROM services_weekly
GROUP BY service
ORDER BY avg_satisfaction DESC;
```

---


**Output:**

| **service**      | **total_patients_admitted** | **avg_satisfaction** | **performance_category** |
|------------------|------------------------------|----------------------|---------------------------|
| ICU              | 245                          | 89.76                | Excellent                 |
| Surgery          | 321                          | 83.42                | Good                      |
| Emergency        | 263                          | 77.21                | Good                      |
| General Ward     | 198                          | 68.54                | Fair                      |
| Recovery         | 176                          | 62.87                | Needs Improvement          |



**Reflection**

Today’s session taught me more than just SQL syntax — it reminded me to slow down and read questions carefully before jumping to write the query.

At first, I:

Forgot to use GROUP BY when calculating aggregated metrics.

Used patient_satisfaction directly instead of applying AVG().

Missed that the question specifically asked for average satisfaction per service.

Tried checking results with a quick LIMIT before verifying the logic.

Through debugging, I realized that thinking through the logic first — how data needs to be grouped, aggregated, and categorized — saves more time than quickly writing and fixing queries later.

Overall, today reinforced the idea that clarity before coding is just as important as syntax accuracy.







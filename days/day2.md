# Day 2 - 21 Days SQL Challenge

**Date:** 4th Nov 2025

---


## Practice Questions

**1. Find all patients who are older than 60 years.**  
```sql
SELECT * FROM patients
WHERE age > 60;
```

**2. Retrieve all staff members who work in the 'Emergency' service.**

```sql

SELECT * FROM staff
WHERE service = 'Emergency';
```

**3. List all weeks where more than 100 patients requested admission in any service.**

```sql
SELECT week, total_patients
FROM services_weekly
WHERE total_patients > 100;
```

## Challenge Question
**Find all patients admitted to 'Surgery' service with a satisfaction score below 70, showing their patient_id, name, age, and satisfaction score.**

```sql
SELECT patient_id AS p_id,
       name AS p_name,
       age AS p_age,
       satisfaction AS p_satis
FROM patients
WHERE service = 'Surgery'
  AND satisfaction < 70;
```

## Key Takeaways
-The WHERE clause helps filter data based on conditions.

-Comparison operators like >, <, =, and <= are used to define filters.

-The AND operator allows combining multiple conditions.

-Using AS makes query outputs more readable.

-Writing queries neatly improves clarity and debugging.

## 💡 Progress Note:
Learned how to apply conditional filters in SQL and practiced using multiple WHERE conditions effectively. Excited for what’s next! 🚀

---

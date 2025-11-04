# Day 1 - 21 Days SQL Challenge

**Date:** 3rd Nov 2025

---

## Practice Questions

**1. Retrieve all columns from the `patients` table.**
```sql
SELECT * FROM patients;
```


**2. Select only the patient_id, name, and age columns from the patients table.**
```sql
SELECT patient_id AS p_id, name AS p_name, age AS p_age FROM patients;
```

**3. Display the first 10 records from the services_weekly table.**
```sql
SELECT * FROM services_weekly LIMIT 10;
```

## Challenge Question

**List all unique hospital services available in the hospital.**
```sql
SELECT DISTINCT service FROM services_weekly;
```
```sql
Output:

 service
------------------
 general_medicine
 surgery
 ICU
 emergency


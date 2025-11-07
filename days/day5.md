# 📅 Day 5 – 21 Days SQL challenge


---

## 🧩 Practice Questions

 **1️⃣ Count the total number of patients in the hospital**
```sql
SELECT COUNT(*) AS total_patients 
FROM patients;
```


**2️⃣ Calculate the average satisfaction score of all patients**
```sql
SELECT ROUND(AVG(satisfaction), 2) AS satisfaction_score 
FROM patients;
```


**3️⃣ Find the minimum and maximum age of patients.**
```sql
SELECT MIN(age) AS youngest_patient, MAX(age) AS oldest_patient 
FROM patients;
```

---


⚡ Challenge Question
**Calculate the total number of patients admitted, total patients refused, and the average patient satisfaction across all services and weeks.
Round the average satisfaction to 2 decimal places.**
```sql
SELECT 
    SUM(patients_admitted) AS total_patients_admitted, 
    SUM(patients_refused) AS total_patients_refused, 
    ROUND(AVG(patient_satisfaction), 2) AS avg_satisfaction
FROM services_weekly;
```

**Output:**

| **total_patients_admitted** | **total_patients_refused** | **avg_satisfaction** |
|------------------------------|-----------------------------|----------------------|
| 5851                         | 7642                        | 80.00                |


**Explanation:**

-SUM(patients_admitted) adds up all admitted patients across services and weeks.

-SUM(patients_refused) gives the total refused patients.

-AVG(patient_satisfaction) computes the overall satisfaction score, rounded to two decimal places.



**✨ Learning Reflection:**

Today’s challenge deepened my understanding of aggregation functions. I learned how to combine SUM(), AVG(), and ROUND() in one query to summarize large datasets efficiently. Each small step adds up — and this one made me more confident in data summarization using SQL!

#IDCWithSQL | @DPDzero | @indiandataclub

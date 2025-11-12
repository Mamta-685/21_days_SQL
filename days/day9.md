# Day 9 – 21 Days SQL Challenge

---

## 🧠 Practice Questions

**1. Extract the year from all patient arrival dates.**
```sql
SELECT patient_id, EXTRACT(YEAR FROM arrival_date) AS arrival_year 
FROM patients;
```

**2. Calculate the length of stay for each patient (departure_date - arrival_date).**
```sql
SELECT patient_id, name, (departure_date - arrival_date) AS length_of_stay 
FROM patients;
```

**3. Find all patients who arrived in a specific month (e.g., March).**
```sql
SELECT patient_id, name, arrival_date 
FROM patients WHERE EXTRACT(MONTH FROM arrival_date) = 3;
```



## 🚀 Daily Challenge
**Question:
Calculate the average length of stay (in days) for each service, showing only services where the average stay is more than 7 days.
Also show the count of patients and order by average stay descending.**

Final Query:

```sql
SELECT 
  service,
  COUNT(*) AS total_patients,
  ROUND(AVG(departure_date - arrival_date), 2) AS avg_stay
FROM patients
GROUP BY service
HAVING AVG(departure_date - arrival_date) > 7
ORDER BY avg_stay DESC;
```

**Ouput :**

| **service**  | **total_patients** | **avg_stay** |
|--------------|--------------------|--------------|
| surgery      | 254                | 7.87         |
| ICU          | 241                | 7.61         |
| emergency    | 263                | 7.16         |


---


## Reflection

Today’s challenge focused on date functions, and I learned how powerful and clean date-based operations can be in SQL.

At first, I wasn’t sure if I could directly subtract two date columns — but realizing that departure_date - arrival_date gives the number of days was a great insight. It simplified what looked like a complex task!

The key takeaways for today:

EXTRACT() helps pick specific components (like YEAR or MONTH) from dates.

Direct subtraction between date fields can be used to calculate durations.

Using HAVING after GROUP BY lets me filter aggregated data effectively.


@indiandataclub | #SQLWithIDC | @DPDzero

## 🗓️ Day 3 — 21 Days SQL Challenge
---

## 📖 Practice Questions

**List all patients sorted by age in descending order.**
```sql
SELECT * FROM patients ORDER BY age DESC;
```

**Show all services_weekly data sorted by week number ascending and patients_request descending.**
```sql
SELECT * FROM services_weekly ORDER BY week ASC, patients_request DESC;
```

**Display staff members sorted alphabetically by their names.**
```sql
SELECT staff_name FROM staff ORDER BY staff_name ASC;
```

---


## 🏆 Daily Challenge


**Retrieve the top 5 weeks with the highest patient refusals across all services, showing week, service, patients_refused, and patients_request.
Sort by patients_refused in descending order.**
```sql
SELECT week, service, patients_refused, patients_request
FROM services_weekly
ORDER BY patients_refused DESC
LIMIT 5;
```
## Output 

| **Week** | **Service**        | **Patients Refused** | **Patients Request** |
|-----------|--------------------|----------------------|----------------------|
| 5         | Emergency          | 363                  | 388                  |
| 12        | Emergency          | 319                  | 347                  |
| 8         | Emergency          | 214                  | 240                  |
| 51        | General Medicine   | 211                  | 285                  |
| 1         | General Medicine   | 164                  | 201                  |

  ---

## 📘 Key Takeaways

-ORDER BY helps organize data for clear insights and trend discovery.

-Using LIMIT efficiently narrows results to the top-performing (or most critical) records.

-Clean and consistent SQL formatting makes analysis smoother.

-Observed that emergency services recorded the highest patient refusals in multiple weeks.

## 💬 Reflection

Today, I focused on understanding how sorting and limiting data can reveal hidden trends in datasets.
It was interesting to see how a simple combination of ORDER BY and LIMIT can provide quick insights into real-world data, like identifying which service faced the most patient refusals.



-#IDCWithSQL
-@indiandataclub
-@dpdzero

---

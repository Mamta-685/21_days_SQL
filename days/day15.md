## Day 15 — 21-Days SQL Challenge

---

## Practice Questions

**1. Join patients, staff, and staff_schedule to show patient service and staff availability.**
```sql
SELECT p.patient_id, p.name AS patient_name, p.service, s.staff_id, s.staff_name, ss.week, ss.present
  FROM patients p
  LEFT JOIN staff s ON p.service = s.service
  LEFT JOIN staff_schedule ss ON s.staff_id = ss.staff_id;
```

**2. Combine services_weekly with staff and staff_schedule for a comprehensive service view.**
```sql
SELECT sw.week, sw.service, sw.patients_admitted, sw.patients_refused, s.staff_id, s.staff_name, ss.week AS schedule_week, ss.present
  FROM services_weekly sw
  LEFT JOIN staff s ON sw.service = s.service
  LEFT JOIN staff_schedule ss ON s.staff_id = ss.staff_id;
```

**3. Create a multi-table report showing patient admissions with staff information.**
```sql
SELECT p.patient_id, p.name AS patient_name, p.service, sw.patients_admitted, s.staff_id, s.staff_name
  FROM patients p
  LEFT JOIN services_weekly sw ON p.service = sw.service
  LEFT JOIN staff s ON p.service = s.service;
```


## Daily Challenge
**Question:
Create a comprehensive service analysis report for week 20 showing: service name, total patients admitted that week, total patients refused, average patient satisfaction, count of staff assigned to service, and count of staff present that week. Order by patients admitted descending.**
```sql
SELECT sw.service, SUM(sw.patients_admitted) AS total_admitted, SUM(sw.patients_refused) AS total_refused,
  ROUND(AVG(sw.patient_satisfaction), 2) AS avg_satisfaction,
  COUNT(DISTINCT s.staff_id) AS total_staff,
  SUM(CASE WHEN ss.present = 1 THEN 1 ELSE 0 END) AS staff_present
  FROM services_weekly sw
  LEFT JOIN staff s ON sw.service = s.service
  LEFT JOIN staff_schedule ss ON s.staff_id = ss.staff_id AND ss.week = sw.week
  WHERE sw.week = 20 GROUP BY sw.service ORDER BY total_admitted DESC;
```

**Output**
| **service**        | **total_admitted** | **total_refused** | **avg_satisfaction** | **total_staff** | **staff_present** |
|--------------------|--------------------|--------------------|-----------------------|------------------|--------------------|
| general_medicine   | 1080               | 810                | 64.00                 | 27               | 24                 |
| surgery            | 682                | 176                | 99.00                 | 22               | 22                 |
| emergency          | 609                | 696                | 93.00                 | 29               | 27                 |
| ICU                | 480                | 192                | 85.00                 | 48               | 42                 |

---


**Reflection**

- I understood why SUM(CASE…) is used to count conditions like presence.

- I learned how joins must chain logically: service → staff → schedule

- I practiced looking at intermediate outputs instead of jumping to full queries.

- Multi-table joins now feel less confusing because I understand the flow.

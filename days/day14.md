## Day 14 – 21 Days SQL Challenge

---

## Practice Questions

**1. Show all staff members and their schedule information (including those with no schedule entries).**
```sql
SELECT s.staff_id, s.staff_name, s.role, s.service, ss.week
  FROM staff s
  LEFT JOIN staff_schedule ss
  ON s.staff_id = ss.staff_id;
```

**2. List all services from services_weekly and their corresponding staff (show services even if no staff assigned).**
```sql
SELECT sw.service, sw.week, sw.patients_request, sw.patients_admitted, s.staff_id, s.staff_name
  FROM services_weekly sw
  LEFT JOIN staff s
  ON sw.service = s.service;
```


**3. Display all patients and their service's weekly statistics (if available).**
```sql
SELECT p.patient_id, p.name AS patient_name, p.age, p.service, sw.week, sw.patients_request, sw.patients_admitted
  FROM patients p
  LEFT JOIN services_weekly sw
  ON p.service = sw.service;
```


## Daily Challenge 
**Question
Create a staff utilisation report showing all staff members (staff_id, staff_name, role, service) and the count of weeks they were present (from staff_schedule). Include staff members even if they have no schedule records. Order by weeks present descending.**
```sql
SELECT s.staff_id, s.staff_name, s.role, s.service,
    COUNT(ss.week) AS weeks_present
    FROM staff s
    LEFT JOIN staff_schedule ss
    ON s.staff_id = ss.staff_id
GROUP BY s.staff_id, s.staff_name, s.role, s.service
ORDER BY weeks_present DESC;
```

**Output:**

| **staff_id**   | **staff_name**    | **role**            | **service** | **weeks_present** |
|----------------|-------------------|----------------------|-------------|--------------------|
| STF-107a58e4   | Cristian Santos   | doctor               | emergency   | 52                 |
| STF-30d44186   | James Ferrell     | doctor               | surgery     | 52                 |
| STF-1ad309f8   | Matthew Foster    | nursing_assistant    | emergency   | 52                 |
| STF-a94474ff   | Lisa Archer       | nurse                | ICU         | 52                 |
| STF-e51bdcb4   | Jamie Arnold      | nurse                | emergency   | 52                 |
| …              | …                 | …                    | …           | …                  |


---


**Reflection**

Today’s confusion came from misunderstanding the JOIN condition and when to select all columns vs specific columns.

- **What I learned:**

- LEFT JOIN always keeps the left table fully intact (even if no match exists).

- Writing ON tableA.col = tableB.col — it must match the relationship in the question.

- Using * randomly makes queries unreadable and hides errors.

- For reports, selecting only relevant columns is the correct approach.

- Counting attendance (COUNT(ss.week)) works because NULL values are ignored automatically.


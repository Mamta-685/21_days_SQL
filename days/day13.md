## Day 13 - 21 Days SQL Challenge

---

## Practice Questions

**1.Join patients and staff based on their common service field (show patient and staff who work in same service).**
```sql
SELECT p.patient_id, p.name AS patient_name, p.service AS patient_service, s.staff_id, s.staff_name, s.role
  FROM patients p
  JOIN staff s
  ON p.service = s.service;
```


**2. Join services_weekly with staff to show weekly service data with staff information.**
```sql
SELECT sw.week, sw.service, sw.patients_request, sw.patients_admitted, sw.patients_refused, s.staff_id, s.staff_name, s.role
  FROM services_weekly sw
  JOIN staff s
  ON sw.service = s.service;
```

**3. Create a report showing patient information along with staff assigned to their service.**
```sql
SELECTp.patient_id, p.name AS patient_name, p.age, p.service, s.staff_id, s.staff_name, s.role
  FROM patients p
  LEFT JOIN staff s
  ON p.service = s.service;
```

## Daily Challenge
**Question:
Create a comprehensive report showing patient_id, patient name, age, service, and the total number of staff members available in their service. Only include patients from services that have more than 5 staff members. Order by number of staff descending, then by patient name ascending.**

```sql
SELECT  p.patient_id, p.name AS patient_name, p.age, p.service, sc.staff_count
  FROM patients p
  JOIN (
    SELECT service, COUNT(*) AS staff_count
    FROM staff
    GROUP BY service
) sc
  ON p.service = sc.service WHERE sc.staff_count > 5 ORDER BY sc.staff_count DESC, p.name ASC;
```

**Explanation:**

The subquery sc computes staff_count per service.

We join patients to that aggregated result to get staff_count beside each patient.

WHERE sc.staff_count > 5 filters services by staff size (group-level filter).

Finally we order by staff_count (desc) then patient name (asc).

---

**Reflection:**

- My earlier mistake

 -I used staff_id > 5 and SUM(staff_id) in a single query, trying to test the condition on individual ID values and summing identifiers. That’s wrong because staff_id is an identifier (not a numeric quantity to sum) and staff_id > 5 filters rows, not group totals.

- Why that was wrong

 -WHERE staff_id > 5 checks a column value for each row, not the number of staff in a service.

 -SUM(staff_id) attempts to add identifier values — pointless and error-prone.

 -I missed the need to aggregate staff by service first, then join that aggregated result to patients.

- What I improved

 -I learned to break the problem into two steps: (A) aggregate staff per service, (B) join aggregated results to patients.

 -I now clearly distinguish row-level filters (WHERE) and group-level filters (HAVING / filtering aggregated results after GROUP BY).

 -I practice creating subqueries that compute group metrics, then joining them for final reports — a cleaner, more reliable approach.




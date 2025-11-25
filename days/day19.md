## Day 19 – 21 Days SQL Challenge

 ---


## Practice Questions 

**1.Rank patients by satisfaction score within each service.**
```sql
SELECT patient_id, name, service, patient_satisfaction,
    RANK() OVER (PARTITION BY service ORDER BY patient_satisfaction DESC) AS satisfaction_rank
FROM patients;
```

**2. Assign row numbers to staff ordered by their name.**
```sql
SELECT staff_id, staff_name,
    ROW_NUMBER() OVER (ORDER BY staff_name) AS row_num
FROM staff;
```

**3. Rank services by total patients admitted.**
```sql
SELECT  service, total_admitted,
    RANK() OVER (ORDER BY total_admitted DESC) AS admitted_rank
FROM ( SELECT  service, SUM(patients_admitted) AS total_admitted FROM services_weekly GROUP BY service
) AS t;
```



## Daily Challenge 
**question:
For each service, rank the weeks by patient satisfaction score (highest first). Show service, week, patient_satisfaction, patients_admitted, and the rank. Include only the top 3 weeks per service.**
```sql
SELECT service, week, patient_satisfaction, patients_admitted
FROM ( SELECT  service, week, patient_satisfaction, patients_admitted,
        RANK() OVER (PARTITION BY service ORDER BY patient_satisfaction DESC) AS r
    FROM services_weekly
) AS x
WHERE r <= 3;
```
## Output:
| **service**        | **week** | **patient_satisfaction** | **patients_admitted** |
|--------------------|---------|---------------------------|------------------------|
| emergency          | 16      | 62                        | 23                     |
| emergency          | 23      | 62                        | 22                     |
| emergency          | 50      | 63                        | 24                     |
| emergency          | 47      | 63                        | 22                     |
| general_medicine   | 29      | 60                        | 30                     |
| general_medicine   | 9       | 63                        | 38                     |
| general_medicine   | 20      | 64                        | 40                     |
| general_medicine   | 35      | 64                        | 40                     |
| ICU                | 11      | 61                        | 14                     |
| ICU                | 30      | 62                        | 7                      |
| ICU                | 18      | 63                        | 9                      |
| surgery            | 9       | 61                        | 22                     |
| surgery            | 43      | 62                        | 33                     |
| surgery            | 38      | 62                        | 47                     |


## Mistakes I Made Today

- ### Mistake 1:
- Using GROUP BY with window functions

- GROUP BY reduces rows, killing the ranking logic.

- **Fix:** Removed GROUP BY completely.

- ### Mistake 2:
- Missing PARTITION BY

- Ranking was being done globally across all services.

- **Fix:**
-  Added: ARTITION BY service

- ### Mistake 3 :
- Filtering ranks incorrectly
- **Fix:** Filtered using r <= 3 in outer query.


## Reflection 

Today was a challenging but transformative day.

I made mistakes, but instead of feeling frustrated, I used each error message as a clue. I now realize that real SQL comes from understanding why things don’t work, not just writing a correct query.

I improved my understanding:

- Ranking inside groups

- Difference between window functions and aggregates

- How SQL treats row reduction vs row preservation


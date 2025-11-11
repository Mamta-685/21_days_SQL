# Day 8 – 21 Days SQL Challenge
---


## 🧠 Practice Questions

**1. Convert all patient names to uppercase.**
```sql
SELECT UPPER(name) AS name_in_uppercase FROM patients;
```

**2. Find the length of each staff member's name.**
```sql
SELECT LENGTH(staff_name) AS name_length FROM staff;
```

**3. Concatenate staff_id and staff_name with a hyphen separator.**
```sql
SELECT staff_id || '-' || staff_name AS staff_info FROM staff;
```


## 🚀 Daily Challenge
**Question:
Create a patient summary that shows patient_id, full name in uppercase, service in lowercase, age category (if age >= 65 then 'Senior', if age >= 18 then 'Adult', else 'Minor'), and name length.
Only show patients whose name length is greater than 10 characters.**

Final Query:

```sql
SELECT 
  patient_id, 
  UPPER(name) AS name_in_uppercase, 
  LOWER(service) AS service_in_lowercase, 
  CASE 
    WHEN age >= 65 THEN 'Senior' 
    WHEN age >= 18 THEN 'Adult' 
    ELSE 'Minor' 
  END AS age_category, 
  LENGTH(name) AS name_length 
FROM patients 
WHERE LENGTH(name) > 10;
```

## Reflection
Today’s challenge was all about string functions and the CASE statement — a great mix of logic and formatting.

At first, I made a small but classic SQL mistake: I wrote

```sql
ELSE 'Minor' AS age_category
```
instead of closing the CASE block with END before adding the alias.

That single missing keyword taught me an important lesson — SQL syntax precision matters. Every CASE statement must end properly before you assign it an alias.

I also realized how helpful it is to make query outputs more readable using aliases and formatting functions like UPPER(), LOWER(), and LENGTH().

Overall, today’s learning reinforced two key skills:

Writing cleaner, well-structured SQL queries

Debugging syntax errors patiently instead of rushing

Each little fix is sharpening my SQL instincts! 




---
@indiandataclub
@DPDzero
#SQLWithIDC

## Day 12 — 21 Days SQL Challenge

---

## Practice question

**1. Find all weeks in services_weekly where no special event occurred.**
```sql
select week 
from services_weekly 
where event is null and event = 'none';
```

**2. Count how many records have null or empty event values.**
```sql
select count(*) as total_rows,
  count(event) as events ,
  count(*) - count(event) as null_events
from services_weekly;
```

**3. List all services that had at least one week with a special event.**
```sql
select distinct service
from services_weekly
where event is not null;
```


## Challenge Question
**Question: 
Analyze the event impact by comparing weeks with events vs weeks without events. Show: event status ('With Event' or 'No Event'), count of weeks, average patient satisfaction, and average staff morale. Order by average patient satisfaction descending.**
```sql
select 
    case 
        when event is null or event = 'none' then 'No Event'
        else 'With Event'
    end as event_status,
    count(*) as week_counts,
    round(avg(patient_satisfaction), 2) as avg_patient_satisfaction,
    round(avg(staff_morale), 2) as avg_staff_morale
from services_weekly
group by event_status
order by avg_patient_satisfaction desc;
```

---


## Reflection (Mistakes I Made Today)

-Assumed NULL = no event, without checking the actual dataset values.

-Forgot that 'none' is a string, not a missing value.

-Tried to solve the challenge question without grouping properly.

-Wrote the CASE expression correctly but overlooked the fact that 'none' was not being filtered, so everything appeared as "With Event".


**How I improved today!**

-Learned to check data patterns before writing conditions.

-Learned the importance of handling both NULL and placeholder strings like 'none'.

-Used CASE to classify data logically.

-Wrote cleaner grouping and conditional filtering.

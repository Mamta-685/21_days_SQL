## 🗓️ Day 4 — 21 Days SQL Challenge

## 📖 Practice Questions

**Display the first 5 patients from the patients table.**
```sql
SELECT * FROM patients LIMIT 5;
```

**Show patients 11–20 using OFFSET.**
```sql
SELECT * FROM patients LIMIT 10 OFFSET 10;
```

**Get the 10 most recent patient admissions based on arrival_date.**
```sql
SELECT patient_id, name
FROM patients
ORDER BY arrival_date DESC
LIMIT 10;
```

---

## 🏆 Daily Challenge

**Question:
Find the 3rd to 7th highest patient satisfaction scores from the patients table, showing patient_id, name, service, and satisfaction. Display only these 5 records.**
```sql
SELECT patient_id, name, service, satisfaction
FROM patients
ORDER BY satisfaction DESC
OFFSET 2 LIMIT 5;
```

**🧾 Output**
| **patient_id** | **name**         | **service**  | **satisfaction** |
|-----------------|------------------|--------------|------------------|
| PAT-581a47d8    | Paul Castaneda   | Emergency    | 99               |
| PAT-84559395    | Jamie Smith      | Surgery      | 99               |
| PAT-87783e44    | Devon Flores     | Emergency    | 99               |
| PAT-a2ce687a    | Craig Morrison   | Surgery      | 99               |
| PAT-26a57bc7    | Jeffrey Mills    | Surgery      | 99               |

---

## 📘 Key Takeaways

-Learned how to extract specific ranked ranges using OFFSET and LIMIT.

-Realized the importance of ordering results (ORDER BY) to retrieve ranked data.

-Practiced pagination logic — useful for dashboards and web apps.

-Optional: Adding a secondary sort (e.g., by patient_id) avoids ties confusion.

## 💬 Reflection

This challenge helped me understand how LIMIT and OFFSET can be used together to extract middle-ranked data efficiently.
It felt great to control exactly which portion of the dataset I wanted — just like paging through a big report.



#IDCWithSQL

@Indiandataclub

@dpdzero



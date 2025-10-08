# Day 24 – Finding Countries with Above Global Average Call Duration

## Problem
A telecommunications company wants to identify **countries** where the **average call duration** is **strictly greater** than the **global average call duration**.  

The dataset includes three tables:  
- **Person** – Contains each person’s name and phone number.  
- **Country** – Contains country names and their 3-digit codes.  
- **Calls** – Contains all calls made, including the caller, callee, and duration.

Each phone number follows the format `'xxx-yyyyyyy'`, where `'xxx'` is the **country code**.  
The goal is to extract the caller’s or callee’s country, calculate its average call duration, and compare it to the global average.

---

## Final SQL Solution

```sql
SELECT
    co.name AS country
FROM
    person p
JOIN
    country co
    ON SUBSTRING(phone_number, 1, 3) = country_code
JOIN
    calls c
    ON p.id IN (c.caller_id, c.callee_id)
GROUP BY
    co.name
HAVING
    AVG(duration) > (SELECT AVG(duration) FROM calls);
```

---

## Step-by-Step Explanation

1. **Identify each person’s country**
   - We use `SUBSTRING(phone_number, 1, 3)` to extract the **3-digit country code** from the phone number.
   - Then we join this to the `country` table using `country_code` to get the **country name**.

2. **Link calls to people**
   - The `Calls` table has both a `caller_id` and a `callee_id`.
   - Using `p.id IN (c.caller_id, c.callee_id)` ensures we capture **all calls** a person is part of — whether they made or received the call.

3. **Group by country**
   - Grouping by `co.name` allows us to calculate the **average call duration** for each country.

4. **Compare with global average**
   - The `HAVING` clause compares each country’s average duration to the **global average duration**:
     ```sql
     HAVING AVG(duration) > (SELECT AVG(duration) FROM calls)
     ```
   - Only countries whose average call duration is **strictly greater** than the global average are returned.

---

## Key Takeaway
Today, I worked with **string functions**, **joins**, and **aggregate comparisons** to derive insights across multiple datasets.  

Key learnings:
- **SUBSTRING()** is useful for extracting parts of text-based fields like phone numbers.  
- **IN (c.caller_id, c.callee_id)** helps combine multiple relationships in a single join.  
- Using **HAVING** with a **subquery** allows comparing group-level aggregates (per country) to global metrics (overall average).  

This exercise reinforced how to perform **comparative analysis** in SQL, an essential skill in telecom and data analytics reporting.

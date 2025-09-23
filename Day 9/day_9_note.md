# Day 9 - LeetCode SQL Challenge

**Date:** September 23, 2025

## Problem
Today's challenge was more complex and required combining **aggregate functions** with **filtering conditions** across multiple groups.

The task was to report the **sum of all total investment values in 2016 (`tiv_2016`)** for all policyholders who meet the following criteria:

1. **Duplicate `tiv_2015` values:**  
   The policyholder must have the same `tiv_2015` as **one or more other policyholders**.

2. **Unique location:**  
   The `(lat, lon)` pair (representing a city location) must **not be shared** by any other policyholder.

Finally, the result needed to be **rounded to two decimal places**.

---

## Solution Using CTEs

Here is the final SQL solution, which uses **Common Table Expressions (CTEs)** to make the logic clear and modular:

```sql
WITH duplicate_tiv_2015 AS (
    -- Find all tiv_2015 values that appear more than once
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
),
unique_locations AS (
    -- Find all unique (lat, lon) pairs that appear exactly once
    SELECT lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
),
qualified_policyholders AS (
    -- Filter policyholders that meet both conditions
    SELECT i.*
    FROM Insurance i
    JOIN duplicate_tiv_2015 d
      ON i.tiv_2015 = d.tiv_2015
    JOIN unique_locations u
      ON i.lat = u.lat AND i.lon = u.lon
)
SELECT 
    ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM qualified_policyholders;
```

---

## Explanation of Steps

### 1. **Find duplicated `tiv_2015`**
The first CTE, `duplicate_tiv_2015`, groups by `tiv_2015` and returns only those values that occur **more than once**.

### 2. **Find unique locations**
The second CTE, `unique_locations`, groups by `(lat, lon)` and returns only locations that occur **exactly once**, ensuring no two policyholders share the same city.

### 3. **Filter qualified policyholders**
The third CTE, `qualified_policyholders`, filters the original table using joins so that only rows meeting **both criteria** remain.

### 4. **Calculate the final sum**
The main query then sums up all `tiv_2016` values and rounds the result to **two decimal places**.

---

## Key Learning
Today marked an important milestone as I tackled a **more complex real-world problem**.  
- I practiced **multiple levels of filtering**, combining them step-by-step using CTEs.  
- This problem emphasized the importance of breaking a problem down into **logical stages**.  
- Using CTEs made the query far more **readable and easier to debug**, which is essential as I progress deeper into medium and advanced SQL challenges.

---

## Final Thoughts
This challenge demonstrated how **SQL can elegantly handle complex multi-condition problems** by layering queries step-by-step.  
As I continue the challenge, I’m becoming more confident with **advanced SQL techniques**, especially **CTEs and aggregates**.

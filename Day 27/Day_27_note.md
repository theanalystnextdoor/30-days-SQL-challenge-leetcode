# Day 27: Finding Users Who Called the Same Person First and Last in a Day

## **Problem Statement**
We need to find all users whose **first and last calls on any given day** were **with the same person** — regardless of whether they were the caller or the recipient.

Each record in the `Calls` table represents a phone conversation between two users (`caller_id` and `recipient_id`) at a specific time.  
We must analyze daily call patterns to identify users who started and ended their day talking to the same contact.

---

## **Solution Query**
```sql
WITH all_calls AS (
    SELECT 
        caller_id AS user_id,
        recipient_id AS contact_id,
        call_time
    FROM Calls
    UNION ALL
    SELECT 
        recipient_id AS user_id,
        caller_id AS contact_id,
        call_time
    FROM Calls
),
ranked_calls AS (
    SELECT
        user_id,
        contact_id,
        DATE(call_time) AS call_date,
        ROW_NUMBER() OVER(PARTITION BY user_id, DATE(call_time) ORDER BY call_time ASC) AS first_call_rank,
        ROW_NUMBER() OVER(PARTITION BY user_id, DATE(call_time) ORDER BY call_time DESC) AS last_call_rank
    FROM all_calls
)
SELECT DISTINCT f.user_id
FROM ranked_calls f
JOIN ranked_calls l
  ON f.user_id = l.user_id 
  AND f.call_date = l.call_date
WHERE f.first_call_rank = 1
  AND l.last_call_rank = 1
  AND f.contact_id = l.contact_id;
```

---

## **Step-by-Step Explanation**

### 1️⃣ Combine Both Caller and Receiver Perspectives
We use a `UNION ALL` to make sure every call is counted **from both sides** —  
so whether a user made or received the call, it’s treated as their interaction.
```sql
SELECT caller_id AS user_id, recipient_id AS contact_id
UNION ALL
SELECT recipient_id AS user_id, caller_id AS contact_id
```

This gives us a unified view of all user-to-user calls.

---

### 2️⃣ Rank Calls by Time
We use two `ROW_NUMBER()` functions to track the **first** and **last** calls per user per day:
- One ranks calls in **ascending order** (earliest first).
- The other ranks them in **descending order** (latest first).

This helps us identify the **first** and **last** interactions in a single query.

---

### 3️⃣ Compare First and Last Contacts
We then join the ranked results to match users whose:
- `first_call_rank = 1`
- `last_call_rank = 1`
- and both have the **same contact_id**

This means the same person appeared in both the first and last call positions that day.

---

### 4️⃣ Get Unique Users
A user can qualify on multiple days, but we only want each `user_id` once —  
so we use `DISTINCT` in the final output.

---

## **Key Learning Today**
> *Today, I learned how to manage **bidirectional data** by unifying records with `UNION ALL` and how to use multiple window functions to analyze time-based behavior.  
> Tracking first and last interactions provides valuable insights in communication datasets — like identifying consistent communication patterns or strong relationships.*

---

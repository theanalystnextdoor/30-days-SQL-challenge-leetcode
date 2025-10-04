# Day 20 – Finding Users with 5 Consecutive Login Days

## **Problem**
Today’s challenge is about identifying people who logged in for 5 consecutive days using SQL.

We’ll work with two tables:

Logins → contains id (user id) and login_date.

Accounts → contains user details (id, name, etc.).

## **Solution Query**

```sql
SELECT A.id, A.name
FROM (
    SELECT DISTINCT b.id
    FROM Logins a, Logins b
    WHERE a.id = b.id 
      AND DATEDIFF(b.login_date, a.login_date) BETWEEN 0 AND 4
    GROUP BY b.id, b.login_date
    HAVING COUNT(DISTINCT a.login_date) = 5
) AS t1
JOIN Accounts A
  ON t1.id = A.id
ORDER BY A.id;
```
---

## **Step-by-Step Explanation**
1. Self-Join on the Logins Table
```sql
FROM Logins a, Logins b
WHERE a.id = b.id
```
We join the Logins table with itself (alias a and b).

This allows us to compare each login date of a user (a.login_date) against other login dates of the same user (b.login_date).

2. Filter Logins Within a 5-Day Window
```sql
AND DATEDIFF(b.login_date, a.login_date) BETWEEN 0 AND 4
```
DATEDIFF checks the difference in days between two login dates.

By restricting the difference between 0 and 4, we only look at windows of 5 consecutive days.

3. Group by User and Reference Date
```sql
GROUP BY b.id, b.login_date
```
For each user (b.id) and each potential "anchor day" (b.login_date), we group together login activity.

4. Require Exactly 5 Distinct Login Days
```sql
HAVING COUNT(DISTINCT a.login_date) = 5
```
This ensures that within the 5-day window, the user logged in on all 5 different days.

If a user missed even one day, the count would be less than 5.

5. Select Distinct User IDs
```sql
SELECT DISTINCT b.id
```
We only keep the unique user IDs that meet the condition of 5 consecutive login days.

6. Join with Accounts
```sql
JOIN Accounts A
  ON t1.id = A.id
```
Finally, we join with the Accounts table to retrieve the user’s name alongside their ID.

7. Order the Output
```sql
ORDER BY A.id;
```
Results are sorted by user ID for readability.

---

## **Key Takeaway**

This solution cleverly uses a self-join with DATEDIFF to capture 5-day windows and then applies a HAVING COUNT(DISTINCT …) = 5 condition to guarantee logins on all 5 consecutive days.

It’s a good example of solving consecutive-day problems without advanced window functions, just by combining grouping and date differences.
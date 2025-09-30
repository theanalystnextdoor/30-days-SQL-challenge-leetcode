# Day 16: Moving Average of Customer Payments  

## **Problem Statement**  
As a restaurant owner analyzing a possible expansion, we want to compute the **7-day moving average** of how much customers paid.  

- The moving window is **current day + 6 days before**.  
- `average_amount` should be **rounded to two decimal places**.  
- Return results ordered by `visited_on` in ascending order.  

---

## **Solution Query**
```sql
SELECT 
    a.visited_on,
    SUM(b.amount) AS amount,
    ROUND(SUM(b.amount) / 7, 2) AS average_amount
FROM (
    SELECT DISTINCT visited_on
    FROM Customer
) a
JOIN Customer b 
    ON b.visited_on BETWEEN DATE_SUB(a.visited_on, INTERVAL 6 DAY) AND a.visited_on
WHERE a.visited_on >= (
    SELECT DATE_ADD(MIN(visited_on), INTERVAL 6 DAY)
    FROM Customer
)
GROUP BY a.visited_on
ORDER BY a.visited_on;

```

---

## **Step-by-Step Explanation**

### Get All Unique Days (a)

The subquery:
```sql
SELECT DISTINCT visited_on FROM Customer
```

-- ensures we calculate the window for each distinct date.

### Join With Payments Table (b)
-- Join back to Customer to collect all payments in the 7-day window:

```sql

b.visited_on BETWEEN DATE_SUB(a.visited_on, INTERVAL 6 DAY) AND a.visited_on

```

### Skip Early Days
-- We don’t want incomplete windows before day 7:

```sql

WHERE a.visited_on >= (SELECT DATE_ADD(MIN(visited_on), INTERVAL 6 DAY) FROM Customer)

```

### Aggregate and Round

**SUM(b.amount)** gives total amount in the 7-day window.

**ROUND(SUM(b.amount) / 7, 2)** computes the average rounded to 2 decimals.

---

## **Key Learning Today**

"Today, I explored how to calculate moving averages in SQL using self-joins and date ranges. Unlike window functions, this method gives more flexibility with rolling windows while still achieving the same result."

Learned how to use **DATE_SUB** and **DATE_ADD** for date ranges.

Understood how to handle moving windows with joins.

Reinforced how to filter out incomplete windows for accuracy.
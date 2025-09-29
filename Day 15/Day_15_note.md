# Day 15: Running Total in SQL

## **Problem Statement**
Today, the goal was to calculate a **running total (cumulative sum)** of points grouped by gender and ordered by day.  
This helps in tracking the progressive growth of values over time.

---

## **Solution Query**
```sql
SELECT 
    gender,
    day,
    SUM(score_points) OVER(PARTITION BY gender ORDER BY day) AS running_total
FROM Scores
ORDER BY gender, day;
```

---

### **Step-by-Step Explanation**
#### PARTITION BY gender
-- Divides the data into separate groups based on gender.
-- Each gender has its own independent running total.

#### ORDER BY day
-- Ensures the sum is calculated in chronological order within each gender.

#### SUM() OVER()
-- **SUM()** is applied as a window function, meaning it doesn't collapse rows like **GROUP BY**.
-- It progressively adds values row-by-row, giving a rolling or running total.

---

### **Alternative Methods**
-- Although window functions are best for running totals, you can also:
-- Use a **self-join** to sum all previous rows for each gender/day.
-- Use a correlated **subquery** to achieve the same result.
-- These methods work but are less efficient than window functions.

---

### **Key Takeaway**
-- **Window functions** are the cleanest and fastest way to compute running totals in SQL.
-- They’re perfect for cumulative reports like sales, balances, or engagement trends.

End of Day 15 — mastering running totals with **window functions**!
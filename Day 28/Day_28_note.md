# Day 28: 3-Month Cumulative Salary Summary

## **Problem Statement**
We need to calculate a **3-month cumulative salary summary** for each employee from the `Employee` table.

The rule is simple:
- For each month an employee worked, compute the **sum of salaries in that month and the previous two months**.  
- If an employee did not work in earlier months, treat their salary for those months as **0**.  
- Exclude the **most recent month** for each employee from the final report.  
- Return results ordered by `id` ascending and `month` descending.

---

## **Solution Query**
```sql
SELECT 
    id,
    month,
    SUM(salary) OVER (
        PARTITION BY id 
        ORDER BY month 
        RANGE BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS Salary
FROM Employee
WHERE (id, month) NOT IN (
    SELECT id, MAX(month) AS month 
    FROM Employee 
    GROUP BY id
)
ORDER BY id, month DESC;
```

---

## **Step-by-Step Explanation**

###  1️⃣ Partition Data by Employee
```sql
PARTITION BY id
```
This ensures the salary calculation restarts for each employee — so the cumulative totals don’t mix across employees.

---

###  2️⃣ Order the Data by Month
```sql
ORDER BY month
```
This sets the sequence of months to correctly calculate rolling totals month by month.

---

###  3️⃣ Define the Rolling 3-Month Window
```sql
RANGE BETWEEN 2 PRECEDING AND CURRENT ROW
```
This clause tells SQL to **sum the salary for the current month and the previous two months**.  
If there are missing months, they are treated as **0**, effectively creating a 3-month rolling window.

---

###  4️⃣ Exclude the Most Recent Month
```sql
WHERE (id, month) NOT IN (
    SELECT id, MAX(month)
    FROM Employee
    GROUP BY id
)
```
The problem states that we should not include the **latest month** of each employee in the result.  
This subquery finds the maximum month (most recent) for each employee and filters it out.

---

###  5️⃣ Sort the Final Output
```sql
ORDER BY id, month DESC
```
Ensures that results are neatly displayed per employee, with the most recent eligible months first.


---

## ** Key Takeaway**
> *Today, I deepened my understanding of **window functions** — particularly the power of the `RANGE` clause in creating rolling calculations.  
> Combining it with a filtering subquery helped me precisely control which periods to include, demonstrating how SQL can easily perform time-based financial analysis.*

---

# Day 17 — Apply Company-Level Tax Rules to Employee Salaries

## Problem
Find each employee's salary **after applying a tax** determined by the **maximum salary in that employee's company**.

Tax rules (per company):
- If the company's max salary < 1000 → **0%** tax  
- If the company's max salary is in \[1000, 10000\] → **24%** tax  
- If the company's max salary > 10000 → **49%** tax
 
Round the resulting salary to the nearest integer.

---

## Final Query (as used)
```sql
SELECT company_id,
       employee_id,
       employee_name,
       ROUND(
         CASE 
           WHEN MAX(salary) OVER(PARTITION BY company_id) BETWEEN 1000 AND 10000 
                THEN salary * (1 - 0.24)
           WHEN MAX(salary) OVER(PARTITION BY company_id) > 10000 
                THEN salary * (1 - 0.49)
           ELSE salary
         END
       ) AS salary
FROM Salaries
```

---

## **Step-by-Step Explanation**

```SQL
FROM Salaries
```

We read the source table that contains every employee row: company_id, employee_id, employee_name, and salary.

```sql
MAX(salary) OVER(PARTITION BY company_id) (used inside CASE)
```

This is a **window function** that computes, for each row, the **maximum salary** of all employees in that row's company.

It does not collapse rows — it attaches the company's max salary to every employee row in that company.

Because it's computed per row, we can directly compare that company-level max to tax thresholds while still keeping the individual employee's salary.

```sql
CASE WHEN ... THEN ... ELSE ... END
```

Applies tax logic based on the company's maximum salary:

If MAX(...) BETWEEN 1000 AND 10000, compute salary * (1 - 0.24) (apply 24% tax).

If MAX(...) > 10000, compute salary * (1 - 0.49) (apply 49% tax).

ELSE salary covers companies where the max is < 1000 (no tax) — though those rows were mostly filtered out by the WHERE clause, keeping ELSE makes the logic explicit and defensive.

```sql
ROUND(... ) AS salary
```

Round the computed (post-tax) salary to the nearest integer.

The alias salary is the final column name returned (matches your expected format).

SELECT company_id, employee_id, employee_name, ...

Return the identifying columns plus the rounded, post-tax salary for each employee.

---

## Key Takeaway  
- Today I finally got more comfortable using **window functions** to solve real-world problems.  
- Instead of writing long CTEs or subqueries, I learned that `MAX OVER` can directly give me the company’s max salary for each row.  
- Using `CASE WHEN` made it easy to apply different tax rules depending on the company’s situation.  
- The big lesson for me is that SQL can feel like a calculator for business logic — once I break the problem down step by step.  





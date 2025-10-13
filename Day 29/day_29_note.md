##  Day 29: Comparing Department and Company Average Salaries

###  **Problem Statement**
We were tasked with finding whether each department’s **average salary** is *higher, lower,* or *the same* compared to the **company’s average salary** for each month.

This exercise demonstrates multi-level aggregation using **CTEs**, **joins**, and **conditional logic (CASE WHEN)** — an essential technique in analytics and HR benchmarking.

---

###  **Solution Query**
```sql
WITH salary_per_month AS (
    SELECT
        employee_id,
        DATE_FORMAT(pay_date, '%Y-%m') AS pay_month,
        AVG(amount) AS avg_salary
    FROM Salary
    GROUP BY employee_id, DATE_FORMAT(pay_date, '%Y-%m')
),
department_avg AS (
    SELECT
        e.department_id,
        s.pay_month,
        AVG(s.avg_salary) AS dept_avg
    FROM salary_per_month s
    JOIN Employee e
      ON s.employee_id = e.employee_id
    GROUP BY e.department_id, s.pay_month
),
company_avg AS (
    SELECT
        pay_month,
        AVG(avg_salary) AS company_avg
    FROM salary_per_month
    GROUP BY pay_month
)
SELECT
    d.pay_month,
    d.department_id,
    CASE
        WHEN d.dept_avg > c.company_avg THEN 'higher'
        WHEN d.dept_avg < c.company_avg THEN 'lower'
        ELSE 'same'
    END AS comparison
FROM department_avg d
JOIN company_avg c
  ON d.pay_month = c.pay_month
ORDER BY d.pay_month, d.department_id;
```

---

###  **Step-by-Step Explanation**

#### **1️⃣ Extract Monthly Average Salary per Employee**
We first calculate the average monthly salary for each employee using the `AVG()` function and `DATE_FORMAT()` to isolate the month.
```sql
SELECT employee_id, DATE_FORMAT(pay_date, '%Y-%m') AS pay_month, AVG(amount) AS avg_salary
FROM Salary
GROUP BY employee_id, DATE_FORMAT(pay_date, '%Y-%m');
```
This prepares a clean base table of employee-month salary averages.

---

#### **2️⃣ Compute Department-Level Averages**
Each employee belongs to a department, so we join with the `Employee` table and aggregate again.
```sql
SELECT e.department_id, s.pay_month, AVG(s.avg_salary) AS dept_avg
FROM salary_per_month s
JOIN Employee e ON s.employee_id = e.employee_id
GROUP BY e.department_id, s.pay_month;
```
Now we have each department’s monthly average salary.

---

#### **3️⃣ Compute Company-Wide Average Salary**
We take another aggregation to find the overall company average salary per month.
```sql
SELECT pay_month, AVG(avg_salary) AS company_avg
FROM salary_per_month
GROUP BY pay_month;
```
This gives us the benchmark to compare each department against.

---

#### **4️⃣ Compare Department vs Company**
Finally, we use a `CASE` statement to label whether the department’s average salary is higher, lower, or the same as the company average.
```sql
CASE
    WHEN d.dept_avg > c.company_avg THEN 'higher'
    WHEN d.dept_avg < c.company_avg THEN 'lower'
    ELSE 'same'
END AS comparison
```

---

### **Key Takeaway**
> *Today I learned how to perform multi-level aggregation in SQL using Common Table Expressions (CTEs). I also reinforced my understanding of comparing grouped results with `CASE` logic — a core analytical skill for comparing departmental or regional performance to company-wide benchmarks.*


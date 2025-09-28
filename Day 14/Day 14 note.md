# Day 14: Finding the Highest Salary in Each Department

## **Problem Statement**
We need to write a query to find **employees who have the highest salary** in their respective departments. If multiple employees share the highest salary in the same department, **all should be included**.

The result should return:
- Department name
- Employee name
- Salary

---

## **Solution Query**
```sql
WITH ranked_salary AS (
    SELECT 
        d.name AS Department,
        e.name AS Employee,
        e.salary AS Salary,
        RANK() OVER(PARTITION BY d.name ORDER BY e.salary DESC) AS ranked
    FROM Employee e
    JOIN Department d
        ON e.departmentId = d.id
)
SELECT Department, Employee, Salary
FROM ranked_salary
WHERE ranked = 1;
```

---

## **Step-by-Step Explanation**

### **1. Use `RANK()` to Identify Top Salaries per Department**
```sql
RANK() OVER(PARTITION BY d.name ORDER BY e.salary DESC) AS ranked
```
- `PARTITION BY d.name` groups the data by **department**.
- `ORDER BY e.salary DESC` ranks employees within each department by **highest salary first**.
- `RANK()` assigns `1` to the highest salary and gives the same rank to ties.

---

### **2. Filter to Get Only Top Salaries**
```sql
WHERE ranked = 1
```
- Ensures we only return employees **who have the highest salary** in their department.

---

### **Why Use `RANK()` Instead of `ROW_NUMBER()`?**
- `RANK()` is perfect here because **ties matter**.
- If two employees share the same highest salary, both will be included.
- `ROW_NUMBER()` would only return **one of them**, which isn't correct in this scenario.

---

## **Key Learning Today**
> *"Today, I learned how to use window functions like `RANK()` to solve real-world ranking problems such as finding the top salaries in each department."*

- `RANK()` is ideal when handling **ties**.
- `PARTITION BY` helps break down the data into groups for separate ranking.
- Using `WITH` (CTE) makes the query **cleaner and easier to read**.

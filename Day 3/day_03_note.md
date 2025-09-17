
# Day 3 - LeetCode SQL Challenge

## Problem Statement
Today's challenge involved a single `Employee` table where each record represents an employee and includes a reference to their **manager**.

The goal was to:
- Find the names of employees **whose salary is greater than their manager's salary**.

This problem is a classic example of a **self-join**, where a table is joined to itself.

---

## My Thought Process
Self-joins can be tricky at first because you are essentially comparing rows **within the same table**.

In this case, each employee has a `managerId` that links back to another `id` in the same `Employee` table.  
This means we needed to **join the table to itself**, matching each employee with their manager.

The tricky part was deciding what to join on:
- `e.managerId = m.id` → This matches an employee's `managerId` with their manager's `id`.

Once I understood this relationship, the rest of the query became straightforward.

---

## Initial SQL Solution

Here’s the first query without using a CTE:

```sql
SELECT 
    e.name AS Employee
FROM Employee e
JOIN Employee m
    ON e.managerId = m.id
WHERE e.salary > m.salary;
```

### **Explanation:**
1. The table `Employee` is aliased twice:
   - `e` → represents the employee.
   - `m` → represents the manager.
2. The `ON` condition links each employee to their manager.
3. The `WHERE` clause filters for employees who earn **more than their manager**.

---

## Refactored Solution Using a CTE

To make the query cleaner and easier to extend, I refactored it using a **Common Table Expression (CTE):**

```sql
WITH EmployeeManagerComparison AS (
    SELECT 
        e.name AS Employee,
        e.salary AS EmployeeSalary,
        m.name AS Manager,
        m.salary AS ManagerSalary
    FROM Employee e
    JOIN Employee m
        ON e.managerId = m.id
)
SELECT Employee
FROM EmployeeManagerComparison
WHERE EmployeeSalary > ManagerSalary;
```

### **Why This is Better:**
- The CTE separates the **joining logic** from the **filtering logic**, making the query easier to read and maintain.
- If more calculations or columns are needed later, they can easily be added inside the CTE without cluttering the main query.

---

## Key Takeaways
- Self-joins are useful when comparing rows **within the same table**.
- Always carefully define the join condition to avoid incorrect matches.
- CTEs make queries **cleaner, easier to read, and more modular**.
- In this challenge, understanding the relationship between `managerId` and `id` was key.

---

**Summary:**  
This problem was a great exercise in **self-joins** and **query refactoring**.  
While the initial query worked, the CTE approach made it easier to manage and set up for more complex extensions.

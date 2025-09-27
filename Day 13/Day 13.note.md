# Day 13: Tracing Employee Hierarchy 

## **Problem Statement**
We need to find all employees who **directly or indirectly** report to the **head of the company**. The reporting chain will **not exceed three levels**.

Return all `employee_id` values that meet this condition.

---

## **Solution Query**
```sql
SELECT DISTINCT e.employee_id
FROM Employees e
JOIN Employees m1 ON e.manager_id = m1.employee_id      -- Level 1: Employee → Manager
JOIN Employees m2 ON m1.manager_id = m2.employee_id     -- Level 2: Manager → Manager's Manager
WHERE m2.manager_id = 1                                 -- Level 3: Head of company (ID = 1)
AND e.employee_id <> e.manager_id;                      -- Prevent an employee from being their own manager
```

---

## **Step-by-Step Explanation**

### **1. Start With Employees Table (`e`)**
- Think of `e` as **all the employees at the bottom** of the hierarchy.
- These are the people whose IDs we want to return.

---

### **2. Join to Their Direct Managers (`m1`)
```sql
JOIN Employees m1 ON e.manager_id = m1.employee_id
```
- Matches each employee to **their immediate manager**.

---

### **3. Join to the Manager's Boss (`m2`)
```sql
JOIN Employees m2 ON m1.manager_id = m2.employee_id
```
- Finds **who the manager reports to**.

---

### **4. Ensure the Top-Level Boss Reports to Head**
```sql
WHERE m2.manager_id = 1
```
- Filters so that the top boss in the chain **directly reports to the head (ID = 1)**

---

### **5. Prevent Invalid Self-Reporting**
```sql
AND e.employee_id <> e.manager_id
```
- Ensures no one is **their own manager**, which would cause invalid data loops.

---

### **6. Use `DISTINCT` to Avoid Duplicates**
- Employees may appear in **multiple paths**, so `DISTINCT` ensures each employee is listed **once**.

---

## **Key Learning Today**
> *"Today, I learned how to trace a multi-level employee reporting structure using **self-joins**. This method is straightforward, easy to debug, and perfect when the hierarchy depth is fixed."*

- Self-joins allow you to **follow a chain step-by-step**.
- They are more **readable and predictable** for small datasets.
- Always include filters to prevent **loops and invalid relationships**.



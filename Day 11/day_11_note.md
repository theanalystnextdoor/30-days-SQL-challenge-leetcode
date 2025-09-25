
# Day 11 of 30 Days SQL Challenge

## **Focus:** Finding Most Experienced Employees per Project Without Window Functions

Today, I worked on a problem where I needed to report the **most experienced employees in each project**.  
If there was a tie (multiple employees with the same max experience), **all tied employees** had to be included in the result.

---

### **Problem Statement**
We are given two tables:

1. **Project Table**  
   - `project_id`  
   - `employee_id`

2. **Employee Table**  
   - `employee_id`  
   - `experience_years`

The goal:
- For each project, find the employee(s) with the **maximum number of experience years**.
- If multiple employees share the maximum experience, **return all of them**.

---

### **My Solution Using CTEs Only**
```sql
WITH max_experience_per_project AS (
    SELECT 
        p.project_id,
        MAX(e.experience_years) AS max_experience
    FROM Project p
    JOIN Employee e 
        ON p.project_id = e.project_id
    GROUP BY p.project_id
)
SELECT 
    p.project_id,
    e.employee_id,
    e.experience_years
FROM Project p
JOIN Employee e 
    ON p.project_id = e.project_id
JOIN max_experience_per_project m
    ON p.project_id = m.project_id
    AND e.experience_years = m.max_experience
ORDER BY p.project_id, e.employee_id;
```

---

### **Step-by-Step Explanation**

#### **Step 1: CTE to Identify Max Experience**
```sql
WITH max_experience_per_project AS (
    SELECT 
        p.project_id,
        MAX(e.experience_years) AS max_experience
    FROM Project p
    JOIN Employee e 
        ON p.project_id = e.project_id
    GROUP BY p.project_id
)
```
---

#### **Step 2: Join to Filter Employees with Max Experience**
```sql
JOIN max_experience_per_project m
    ON p.project_id = m.project_id
    AND e.experience_years = m.max_experience
```
- This filters down to **only those employees whose experience matches the maximum** for that project.

---

#### **Step 3: Final Selection**
Finally, I selected the relevant columns and sorted the output:

```sql
SELECT p.project_id, e.employee_id, e.experience_years
ORDER BY p.project_id, e.employee_id;
---

### **Why This Approach Works**
- **Simple logic**: Uses just `MAX()` and joins, no window functions required.
- **Handles ties naturally**: If multiple employees have the same max experience, they are all included.
- **Readable and clean**: Easy to maintain and debug.

---

### **Key Learnings**
- Practiced solving problems **without window functions**.
- Strengthened my understanding of using **aggregate functions (`MAX`) with CTEs**.
- Learned how to **filter results using joins** to match the max values.

---



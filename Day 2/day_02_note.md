
# Day 2 - LeetCode SQL Challenge

## Problem Statement
Today, I worked on a problem involving two tables: `student` and `department`.  
The goal was to report the **department name** and **number of students majoring in each department**, including departments with **no current students**.

The result needed to be **sorted by student count (descending)** and, in case of a tie, by **department name alphabetically**.

---

## My Thought Process
Today, I expanded my SQL skills by working with **aggregate functions** and more advanced query techniques.

When I read the question, I realized that I needed to return **all departments**, even if they have no students.  
This immediately pointed me toward using a **LEFT JOIN**, as it takes everything from the `department` table and matches it with the `student` table.

Once the join was done, I used the `COUNT` function to count students in each department.  
For departments with no students, the count naturally returned **0**.

Finally, I used `GROUP BY` to group the data by department and applied `ORDER BY` to sort the results:
- **Descending order by student count**
- Alphabetical order by department name in case of ties

---

## SQL Solution

```sql
SELECT 
    d.dept_name,
    COUNT(s.student_id) AS student_number
FROM department AS d
LEFT JOIN student AS s
    ON d.dept_id = s.dept_id
GROUP BY d.dept_id, d.dept_name
ORDER BY student_number DESC, d.dept_name ASC;
```

---

## Key Takeaways
- Learned to use `COUNT()` for data aggregation.  
- Understood when and why to use a `LEFT JOIN`.  
- Practiced grouping data using `GROUP BY`.  
- Learned how to sort results with multiple conditions using `ORDER BY`.

---

**Summary:**  
This challenge helped me strengthen my understanding of aggregate functions and query structuring.  
It was a good exercise in **thinking about data relationships** and ensuring no records were lost when combining tables.

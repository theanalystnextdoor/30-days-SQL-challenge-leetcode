
## Day 8: Finding the Highest Grade for Each Student Using Dense_Rank

## Problem
As I progressed into the second week of my 30-day SQL challenge, I encountered a **medium-level question**.  
The task was to find the **highest grade** with its corresponding course for each student.  
In case of a tie, we should return the **course with the smallest course_id**.

The results should be ordered by `student_id` in ascending order.

---

## Solution

Here is the query I wrote using the `DENSE_RANK()` window function:

```sql
SELECT student_id, course_id, grade
FROM (
    SELECT 
        student_id,
        course_id,
        grade,
        DENSE_RANK() OVER (
            PARTITION BY student_id 
            ORDER BY grade DESC, course_id ASC
        ) AS rnk
    FROM Enrollment
) ranked
WHERE rnk = 1
ORDER BY student_id;

```

---

## Explanation

I used the DENSE_RANK() window function for the first time in this challenge.

PARTITION BY student_id allows ranking within each student group, ensuring each student's courses are handled independently.

ORDER BY grade DESC, course_id ASC:

Highest grades come first.

In case of a tie, the course with the smallest course_id is chosen.

Finally, I filtered for rnk = 1 to return only the top-ranked course per student.

This marks my transition into more advanced SQL concepts as I begin the second week of the challenge, tackling medium-level problems and deepening my understanding of window functions.

Key Takeaway

Window functions are incredibly powerful for ranking and ordering data within groups, and they provide elegant solutions to problems that would otherwise require complex subqueries or joins.
"""

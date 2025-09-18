
# Day 4 - LeetCode SQL Challenge

## Problem Statement
Today's challenge was focused on **aggregating data** using the `SUM` function.  
We were given a table called `Sales` that contains records of sales transactions, including the `product_id` and `quantity` sold for each record.

The goal was to:
- **Calculate the total quantity sold** for each product.  
- Group the results by `product_id` so each product appears only once in the final output.

---

## My Thought Process
When I saw the problem, I immediately knew that this would require an **aggregate function** and a **GROUP BY clause**.

The key steps were:
1. **Summing the quantity sold** for each product using the `SUM()` function.
2. **Grouping by product_id** to ensure the total is calculated correctly for each unique product.

This was straightforward compared to previous days, but it reinforced the importance of understanding aggregation fundamentals in SQL.

---

## SQL Solution

Here’s the query I wrote to solve the problem:

```sql
SELECT 
    product_id,
    SUM(quantity) AS total_quantity
FROM Sales
GROUP BY product_id;
```

### **Explanation:**
- `SUM(quantity)` → adds up the quantity for each product.
- `GROUP BY product_id` → ensures that the sum is calculated **per product**, not for the entire table.
- `AS total_quantity` → renames the output column for clarity.

---

## Key Takeaways
- Learned how to **aggregate data using SUM()**.
- Reinforced understanding of the **GROUP BY** clause.
- Practiced writing simple yet powerful SQL queries for reporting.
- Highlighted how SQL aggregation is essential for summarizing data effectively.

---

**Summary:**  
This challenge was a great practice in applying the basics of SQL aggregation.  
While it seemed simple, mastering these foundational concepts is critical for more advanced analytics work.

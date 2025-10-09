# Day 25 – Finding the Most Frequently Ordered Product by Each Customer

## Problem
Today’s task was to determine the **most frequently ordered product** for each customer.  

The dataset consists of three main tables:
- **Orders** – Contains customer orders and product IDs.  
- **Products** – Contains product names and their corresponding IDs.  
- **Customers** – Contains customer IDs and details.

The goal is to identify which product each customer buys most often, and in case of ties, include all products with the same highest order frequency.

---

## Solution Query

```sql
WITH order_count AS 
(
    SELECT 
        o.product_id,
        p.product_name,
        c.customer_id,
        COUNT(*) AS total_order,
        RANK() OVER(PARTITION BY c.customer_id ORDER BY COUNT(*) DESC) AS rnk
    FROM Orders o
    JOIN Products p
      ON o.product_id = p.product_id
    JOIN Customers c
      ON o.customer_id = c.customer_id
    GROUP BY c.customer_id, product_id
)
SELECT 
    customer_id,
    product_id,
    product_name
FROM order_count
WHERE rnk = 1;
```

---

## Step-by-Step Explanation

1. **Count total orders per product per customer**
   - Using `COUNT(*)`, we count how many times each product was ordered by each customer.
   - We group by both `customer_id` and `product_id` to get totals per combination.

2. **Rank products by order frequency**
   - The `RANK()` window function assigns a rank to each product per customer based on the count of orders.
   - `PARTITION BY c.customer_id` ensures ranking happens **within each customer’s data**, not across all customers.
   - `ORDER BY COUNT(*) DESC` ranks the most ordered products first (rank = 1).

3. **Select top-ranked products**
   - After ranking, we filter the result to include only `rnk = 1`, i.e., the most frequently ordered product(s) for each customer.
   - This allows for multiple products per customer in case of ties.

---

## Key Takeaway
Today’s challenge introduced how to combine **aggregate functions** with **window functions** to analyze customer behavior.  

Key learnings:
- **COUNT()** helps measure frequency of occurrence within grouped data.  
- **RANK() OVER(PARTITION BY … ORDER BY …)** allows comparison within a specific category (like per customer).  
- Combining both lets you quickly find **the top or most frequent item** per group — a powerful pattern for customer and sales analysis.


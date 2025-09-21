
# Day 7: Finding Seller(s) with the Highest Total Sales Using CTE

## **Problem Statement**
Write a query to find the `seller_id` or `seller_ids` whose **total sales amount** (`price`) is the highest among all sellers.

This solution must return **all sellers**, including ties, if multiple sellers share the highest total.

---

## **Solution Using CTE**

```sql
WITH total_sales AS (
    SELECT seller_id, SUM(price) AS total_price
    FROM Sales
    GROUP BY seller_id
),
max_sales AS (
    SELECT MAX(total_price) AS highest_total
    FROM total_sales
)
SELECT t.seller_id
FROM total_sales t
JOIN max_sales m
ON t.total_price = m.highest_total;
```

---

## **Explanation**

### **1. Calculate Total Sales per Seller**
The first CTE, `total_sales`, groups the data by `seller_id` and calculates the total sales (`SUM(price)`) for each seller.

### **2. Identify the Maximum Total Sales**
The second CTE, `max_sales`, finds the highest total sales value from the `total_sales` result.

### **3. Return Sellers with the Maximum Total**
Finally, we join `total_sales` with `max_sales` to return only sellers whose total sales equal the maximum value.  
This ensures that if there is a tie, **all top sellers are included**.

---

## **Key Learning**
Today, I explored **Common Table Expressions (CTEs)** to write cleaner and more organized SQL queries.  
Using multiple CTEs helped break down the problem into logical steps, making the query easier to read and maintain.

I also learned how to handle **ties** when ranking or filtering by aggregate values, ensuring my solution is robust and accurate.


# Day 5 – SQL Learning Journal

## **Topic:** Finding the Customer with the Most Orders

Today, I worked on a SQL challenge where I needed to find the **customer who has placed the largest number of orders**.  
The table `Orders` contains two columns:
- `order_number` (Primary Key)
- `customer_number` (ID of the customer who placed the order)

---

## **Challenge**
The goal was to:
- Return only the `customer_number` of the customer with the **highest order count**.
- Ensure the query works even when there are many customers with different numbers of orders.

---

## **Solution Query**
Here’s the query I wrote:

```sql
SELECT customer_number
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(order_number) DESC
LIMIT 1;
```

---

## **Step-by-Step Breakdown**

1. **`GROUP BY customer_number`**
   - Groups the table by each unique customer.
   - This allows us to perform aggregate functions like `COUNT()` on each customer.

2. **`COUNT(order_number)`**
   - Counts how many orders each customer has placed.

3. **`ORDER BY COUNT(order_number) DESC`**
   - This was the most interesting part for me.
   - I have **never used an aggregation function directly in the `ORDER BY` clause** before.
   - It sorts the grouped results by the total number of orders **in descending order**.
   - The customer with the highest count appears first.

4. **`LIMIT 1`**
   - Ensures that only **one record** is returned — the customer with the most orders.

---

## **Why This Was a Learning Moment**
Before today, I usually used `COUNT()` or other aggregate functions only in the `SELECT` clause, not in `ORDER BY`.  
Using an **aggregate function directly inside `ORDER BY`** felt new and powerful because it allowed me to sort groups **without writing a subquery** or creating a separate alias.

---

## **Key Takeaway**
Today, I learned:
- How to **combine `GROUP BY` with `ORDER BY` using aggregate functions** directly.
- That I don’t need a subquery to sort by an aggregate value.
- This makes my SQL code **simpler and more efficient**.

This new approach will definitely help me write more concise and powerful queries in the future!

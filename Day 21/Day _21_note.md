# Day 21 - SQL Challenge  

## **Problem** 
We need to find the **most recent order(s)** for each product.  

The result must be ordered by:  
1. `product_name` (ascending)  
2. `product_id` (ascending, if tie)  
3. `order_id` (ascending, if tie)  

---

## **Solution Query** 

```sql
WITH recent_product_order AS
(
    SELECT 
        p.product_name,
        p.product_id,
        o.order_id,
        o.order_date,
        RANK() OVER(
            PARTITION BY p.product_id 
            ORDER BY o.order_date DESC
        ) rnk
    FROM Products p
    JOIN Orders o
      ON p.product_id = o.product_id
)
SELECT 
    product_name,
    product_id,
    order_id,
    order_date
FROM recent_product_order
WHERE rnk = 1
ORDER BY product_name, product_id, order_id;

```

---

## **Step-by-Step Explanation**

1. CTE Creation (recent_product_order)

Joins Products with Orders.
Prepares to assign ranks for each product’s orders.

2. Ranking Orders Per Product
```sql
RANK() OVER(
    PARTITION BY p.product_id 
    ORDER BY o.order_date DESC
)
```
Resets ranking for each product (PARTITION BY).
Newest orders get RANK = 1.
If multiple orders share the same latest date, they all get RANK = 1.

3. Filtering for Most Recent Orders
```sql
WHERE rnk = 1
```
Keeps only the latest order(s) for each product.

4. Final Ordering
```sql
ORDER BY product_name, product_id, order_id;
```
Ensures results are sorted properly.

---

## **Key Takeaway**

When solving “most recent per group” problems, **RANK()** (or **ROW_NUMBER()**) is powerful because:

It works within groups **(PARTITION BY)**.

It allows us to capture ties, unlike **MAX()**.

It makes the query easier to extend (e.g., get top-N per product).

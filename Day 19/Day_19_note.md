# Day 19: Finding Product Prices on a Specific Date  

## **Problem Statement**
We need to determine the prices of all products on the date **2019-08-16**.  

- Each product initially has a **default price = 10**.  
- The `Products` table records changes in price with the product ID, the new price, and the date of change.  
- If a product has no price change before that date, its price remains **10**.  
- If there are multiple changes, we take the **most recent one before or on 2019-08-16**.  

---

## **Solution Query**

```sql
SELECT p.product_id,
       COALESCE((
           SELECT pr.new_price
           FROM Products pr
           WHERE pr.product_id = p.product_id
             AND pr.change_date <= '2019-08-16'
           ORDER BY pr.change_date DESC
           LIMIT 1
       ), 10) AS price
FROM (SELECT DISTINCT product_id FROM Products) p;
```

---

## **Step-by-Step Explanation**
1. Get all unique products
```sql
(SELECT DISTINCT product_id FROM Products)
```

This ensures we include every product in the result, even if it has no price change record.

2. Find the latest price change before the target date
```sql
SELECT pr.new_price
FROM Products pr
WHERE pr.product_id = p.product_id
  AND pr.change_date <= '2019-08-16'
ORDER BY pr.change_date DESC
LIMIT 1
```

Only consider changes made before or on 2019-08-16.
Sort by date descending so the most recent change comes first.
Use LIMIT 1 to pick that single most relevant change.

3. Handle missing price changes with COALESCE
```sql
COALESCE(subquery_result, 10)
```

COALESCE returns the first non-null value in its arguments.
If the subquery finds a price, we use it.
If not (meaning no price change before the date), we fall back to the default value 10.

4. Final Output
The result lists:
product_id
price on 2019-08-16

---

## **Key Learning Today**

Today I learned how to track the price of products at a specific point in time by combining subqueries and COALESCE.

COALESCE is powerful for providing default values when a query might return NULL.

In real-life data problems, defaults like this are common — e.g., missing values, fallback pricing, or optional attributes.

This query taught me that careful filtering + ordering + fallback handling is essential when working with historical data.

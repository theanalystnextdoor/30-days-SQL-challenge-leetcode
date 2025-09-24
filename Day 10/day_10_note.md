
# Day 10 of 30 Days SQL Challenge

## **Focus:** Finding First Year Sales per Product Using CTE

Today, I continued to strengthen my understanding and usage of **Common Table Expressions (CTEs)** by solving a problem that required identifying the **first year each product was sold**, along with its **quantity** and **price** for that year.

---

### **Problem**
We have a `Sales` table with details about product sales, including:
- `product_id`
- `year`
- `quantity`
- `price`

The goal is to:
1. Find the **first year** each product appeared in the sales table.
2. Return the sales data (quantity and price) for only that first year.
3. Present the results ordered by `product_id` and `quantity`.

---

### **Solution Using CTE**
```sql
WITH product_first_years AS (
    SELECT 
        product_id,
        MIN(year) AS first_year
    FROM Sales
    GROUP BY product_id
),
first_year_sales AS (
    SELECT 
        s.product_id,
        pfy.first_year,
        s.quantity,
        s.price
    FROM Sales s
    JOIN product_first_years pfy 
    ON s.product_id = pfy.product_id 
    AND s.year = pfy.first_year
)
SELECT 
    product_id,
    first_year,
    quantity,
    price
FROM first_year_sales
ORDER BY product_id, quantity;
```

---

### **Step-by-Step Explanation**

#### **1. First CTE - `product_first_years`**
This part finds the **earliest year** each product was sold.
```sql
SELECT 
    product_id,
    MIN(year) AS first_year
FROM Sales
GROUP BY product_id;
```
- `MIN(year)` ensures we capture the first year of sales for every product.

---

#### **2. Second CTE - `first_year_sales`**
This joins the first CTE back to the main table to filter only sales records from the **first year**.
```sql
SELECT 
    s.product_id,
    pfy.first_year,
    s.quantity,
    s.price
FROM Sales s
JOIN product_first_years pfy 
ON s.product_id = pfy.product_id 
AND s.year = pfy.first_year;
```

---

#### **3. Final Output**
Finally, we select the relevant columns and order them by `product_id` and `quantity` for neat presentation.

---

### **Why Use CTE Here?**
Using CTEs made the solution **cleaner and modular**, allowing me to break down the problem into steps:
- First, isolate the earliest year for each product.
- Then, filter and join the data to get the corresponding sales records.

This improves readability and simplifies debugging compared to writing one long nested query.

---

### **Key Learnings**
- Gained confidence in chaining multiple CTEs to solve real-world problems.
- Understood the power of breaking queries into logical parts for better maintainability.
- Strengthened my understanding of **JOIN operations** combined with aggregated values.

---




# Day 12 - SQL Challenge

## **Problem Statement**
Find the customers who have **purchased all the available products**.  
We are given:
- **Customer table:** Contains customer purchase records, including `customer_id` and `product_key`.
- **Product table:** Contains all available products (`product_key`).

The task is to return only those customers who have bought **every product** in the `Product` table.

---

## **Solution**

```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (
    SELECT COUNT(DISTINCT product_key)
    FROM Product
);
```

---

## **Understanding the Solution**

### **Step 1: Counting all available products**
```sql
SELECT COUNT(DISTINCT product_key) FROM Product;
```
- This counts the **total number of unique products** available in the database.

---

### **Step 2: Counting products each customer purchased**
```sql
SELECT customer_id, COUNT(DISTINCT product_key)
FROM Customer
GROUP BY customer_id;
```
- Groups data by `customer_id` and counts how many **distinct products** each customer has bought.

---

### **Step 3: Using HAVING to filter only those who bought all products**
```sql
HAVING COUNT(DISTINCT product_key) = (total product count)
```
- Filters the grouped results to only include customers whose count matches the total product count.

---

## **Why HAVING and not WHERE?**

| **WHERE** | **HAVING** |
|------------|------------|
| Filters **rows before grouping**. | Filters **groups after aggregation**. |
| Cannot use aggregate functions like `COUNT`, `SUM`, etc. | **Can** use aggregate functions. |

Example:  
- `WHERE` → "Show me purchases made in Lagos." *(row-level filter)*  
- `HAVING` → "Show me customers who bought **at least 5 products**." *(group-level filter)*

---

## **Key Learning**
- **`WHERE`** filters individual rows **before grouping**.  
- **`HAVING`** filters aggregated groups **after grouping**.  
- Combining `GROUP BY` with `HAVING` is essential when solving problems that involve conditions on aggregates like `COUNT` or `SUM`.

---

## **Final Thought**
Today's challenge deepened my understanding of how and when to use **HAVING** vs **WHERE**, and how to approach problems that require checking for completeness across multiple entities.

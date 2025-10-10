# Day 26: Weekly Sales Report by Category

## **Problem Statement**
You are the business owner and want to obtain a **sales report** showing how many units in each **item category** were ordered on each **day of the week**.

The challenge is to ensure **all categories** (even those with no sales) appear in the final report.

---

## **Solution Query**
```sql
SELECT 
    i.item_category AS category,
    SUM(CASE WHEN DAYNAME(o.order_date) = 'Monday' THEN o.quantity ELSE 0 END) AS Monday,
    SUM(CASE WHEN DAYNAME(o.order_date) = 'Tuesday' THEN o.quantity ELSE 0 END) AS Tuesday,
    SUM(CASE WHEN DAYNAME(o.order_date) = 'Wednesday' THEN o.quantity ELSE 0 END) AS Wednesday,
    SUM(CASE WHEN DAYNAME(o.order_date) = 'Thursday' THEN o.quantity ELSE 0 END) AS Thursday,
    SUM(CASE WHEN DAYNAME(o.order_date) = 'Friday' THEN o.quantity ELSE 0 END) AS Friday,
    SUM(CASE WHEN DAYNAME(o.order_date) = 'Saturday' THEN o.quantity ELSE 0 END) AS Saturday,
    SUM(CASE WHEN DAYNAME(o.order_date) = 'Sunday' THEN o.quantity ELSE 0 END) AS Sunday
FROM Orders o
RIGHT JOIN Items i
  ON o.item_id = i.item_id
GROUP BY i.item_category
ORDER BY i.item_category;
```

---

## **Step-by-Step Explanation**

### 🧩 1. Join Tables
We used a **RIGHT JOIN** so that all categories from the `Items` table appear in the output — even if they don’t have corresponding orders.  
This ensures that a category like *T-shirt* (with zero orders) is not left out.

---

### 📅 2. Extract Day Names
We used the SQL function:
```sql
DAYNAME(o.order_date)
```
to get the day of the week from each order date — e.g., “Monday”, “Tuesday”, etc.

---

### 📊 3. Conditional Aggregation
We used `CASE WHEN` inside `SUM()` to count quantities for each day:
```sql
SUM(CASE WHEN DAYNAME(o.order_date) = 'Monday' THEN o.quantity ELSE 0 END)
```
This pattern repeats for all seven days of the week.

---

### 🧮 4. Group by Category
We grouped by `i.item_category` to ensure daily totals are calculated per category.

---

### 📈 5. Order by Category
Finally, we sorted alphabetically by category name for better readability.

---

## **Key Learning Today**
> “Today, I learned how to create detailed weekday sales reports using conditional aggregation and how the type of join (LEFT, RIGHT, or INNER) impacts the completeness of a report.  
> Using `RIGHT JOIN` ensured that even products without sales were represented, which is crucial for accurate business reporting.”



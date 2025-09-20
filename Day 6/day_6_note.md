
# Day 6 – SQL Learning Journal

## **Topic:** Filtering Customers Based on Referrals and Null Values

Today, I solved a SQL problem where I had to **find customer names** that are either:

1. **Referred by a customer whose `id` is NOT 2**, or  
2. **Not referred by anyone at all (no referee)**.

This was an interesting challenge because it required careful handling of **NULL values** in SQL.

---

## **Problem Breakdown**
We have a table `Customer` with the following columns:
- `id` → Unique ID for each customer.
- `name` → Customer's name.
- `referee_id` → The `id` of the customer who referred them (can be `NULL` if they were not referred).

The goal was to **return the names of customers** who either:
- Have a referee **not equal to `2`**, or
- **Do not have a referee** (`referee_id` is `NULL`).

The result can be returned in **any order**.

---

## **Solution Query**
Here is the query I wrote to solve the problem:

```sql
SELECT name
FROM Customer
WHERE referee_id <> 2
   OR referee_id IS NULL;
```

---

## **Key Insights**
### 1. Handling `NULL` Values
Initially, I thought just using:

```sql
WHERE referee_id <> 2
```

would be enough.  
But I quickly realized that **`NULL` values are not returned** with normal comparisons because:

- In SQL, `NULL` means **unknown**, so `referee_id <> 2` will not include rows where `referee_id` is `NULL`.

That's why I added:
```sql
OR referee_id IS NULL
```
This explicitly captures customers with **no referee**.

---

### 2. Why Use `<>` Instead of `!=`
Both `!=` and `<>` can be used for "not equal to" in MySQL.  
However, `<>` is **ANSI SQL standard**, making it **more portable** across different database systems.

Example with `!=`:
```sql
SELECT name
FROM Customer
WHERE referee_id != 2
   OR referee_id IS NULL;
```

Example with `<>` (Preferred):
```sql
SELECT name
FROM Customer
WHERE referee_id <> 2
   OR referee_id IS NULL;
```

---

## **Learning Reflection**
Today, I learned:
- The importance of explicitly handling `NULL` values using `IS NULL` in SQL queries.
- The difference between `!=` and `<>` and why `<>` is more standard.
- How to combine multiple conditions using `OR` to solve complex filtering problems.

This was a solid exercise that sharpened my understanding of **NULL handling** and **filtering techniques** in SQL.

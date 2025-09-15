
# Day 1 – Choosing the Right Join

Today’s problem was about **returning all records from the `Person` table**, and for each person, **showing their `City` and `State`** if that information exists in the `Address` table.  

At first glance, the challenge seemed simple, but I quickly realized the **crucial decision** here was **choosing the right type of SQL join**.  

---

### 🧠 My Thought Process

When I saw the requirement:
> *"Return `NULL` for `City` and `State` if a person's ID is not found in the `Address` table."*

Immediately, a light bulb went off 💡 — **this calls for a `LEFT JOIN`**.

Why?  
- The goal was to **include everyone from the `Person` table**, even if they don't have an address.  
- A `LEFT JOIN` returns **all rows from the first table** (`Person`), and only matches rows from the second table (`Address`) where they exist.  
- If there’s **no matching address**, the query automatically fills in `NULL` for those columns.

In short, the `LEFT JOIN` gives me exactly what the question asked for — full list of people, with address details where available, and `NULL` where not.

---

### 💻 Solution
```sql
SELECT p.PersonId, p.LastName, p.FirstName, a.City, a.State
FROM Person p
LEFT JOIN Address a
ON p.PersonId = a.PersonId;
```

---

### ✍🏾 Reflection
What stood out to me today was how **powerful the right join choice can be**.  
At first, it seemed like a simple query, but choosing the correct join was **the key to solving the problem correctly**.  

This problem reinforced the following lessons:
- Understand the **relationship between tables** before writing the query.  
- `LEFT JOIN` is perfect when you want **everything from the first table**, plus matches from the second.  
- Pay attention to details like `NULL` outputs, as they guide you to the right solution.

It felt good to start this challenge by solving a problem that emphasized **fundamental SQL concepts**. One day down, 29 more to go! 🚀

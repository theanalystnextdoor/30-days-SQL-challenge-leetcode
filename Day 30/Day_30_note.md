# Day 30: Ranking Scores with DENSE_RANK()

## **Problem Statement**
We need to find the rank of scores from the **Scores** table.  
The ranking should follow these rules:

1. Scores are ranked from **highest to lowest**.  
2. If two scores are equal, they share the **same rank**.  
3. After a tie, the **next rank number increases by 1** (no skipped ranks).

---

## **Solution Query**
```sql
SELECT 
    score,
    DENSE_RANK() OVER(ORDER BY score DESC) AS 'rank'
FROM Scores
ORDER BY score DESC;
```
---

## **Step-by-Step Explanation**
1. Selecting the Score Column

We start by selecting the score column, which holds the numerical values we want to rank.
```sql
SELECT score
FROM Scores
```

2. Using the DENSE_RANK() Window Function
```sql
DENSE_RANK() OVER(ORDER BY score DESC)
```

**DENSE_RANK()** assigns ranks without gaps between them.

**ORDER BY score DESC** means the highest score gets rank 1.

Equal scores share the same rank (ties handled automatically).

3. Naming the Rank Column

We use 'rank' (quoted to avoid SQL keyword conflicts) to label the ranking column.

4. Ordering the Output
```sql
ORDER BY score DESC
```

Ensures that results are displayed from the highest to lowest score.

---

## **Key Learning Today**

Today marks the final day of my 30-day SQL challenge, and I explored one of the most powerful window functions — DENSE_RANK().

It’s perfect for generating leaderboards, ranking scores, or assigning order without gaps.

It’s more accurate and elegant than manual ranking methods or subqueries.

I also learned that quoting column names (like 'rank') avoids syntax issues when using reserved words.

---

## **Reflection**

Finishing this challenge taught me that SQL is more than just querying — it’s about thinking analytically, building logic layer by layer, and writing clean, expressive code.
Each day built upon the last, and now I can confidently handle analytical SQL problems with clarity and structure.
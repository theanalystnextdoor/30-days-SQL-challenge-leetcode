# Day 18 – Finding the Candidate with the Highest Votes  

## Problem
We want to return the candidate with the highest number of votes from an election database.  

The database has two tables:  
- **Candidate**: contains candidate information (`id`, `name`)  
- **Vote**: contains votes for each candidate (`id`, `candidateId`)  

---

## Solution Query  
```sql
WITH vote_per_candidate AS (
    SELECT c.name,
           COUNT(v.id) AS total_vote
    FROM Candidate c
    JOIN Vote v
      ON c.id = v.candidateId
    GROUP BY c.name
)
SELECT name AS Name
FROM vote_per_candidate
WHERE total_vote = (
    SELECT MAX(total_vote)
    FROM vote_per_candidate
)

```

---

## Step-by-Step Explanation
1. Build a CTE (vote_per_candidate)
```sql
WITH vote_per_candidate AS (
    SELECT c.name,
           COUNT(v.id) AS total_vote
    ...
    GROUP BY c.name
)
```
For each candidate, we count the number of votes they received.
**GROUP BY c.name** ensures the counting is per candidate.
The result: a table of candidate name → total votes.

2. Select the Candidate(s) with Maximum Votes
```sql
WHERE total_vote = (
    SELECT MAX(total_vote)
    FROM vote_per_candidate
)
```
We compare each candidate’s total votes with the highest vote count.
If multiple candidates are tied for the top, they will all be returned.

3. Final Output

The query returns the name(s) of the candidate(s) who received the highest number of votes.

---

## Key Takeaway

Today I learned how to:

Use a CTE to first compute aggregate results (votes per candidate).

Apply a subquery to filter only the highest values.

Combine both to solve a “top-N” type problem cleanly.
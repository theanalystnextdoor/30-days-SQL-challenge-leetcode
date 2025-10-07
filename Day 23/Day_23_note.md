# Day 23 – Finding the Top Reviewer and Top Rated Movie

## **Problem**

We were given three tables — Movies, Users, and MovieRating — and asked to:

Find the name of the user who rated the most movies.

In case of a tie, return the lexicographically smaller (alphabetically first) user name.

Find the movie name with the highest average rating in February 2020.

In case of a tie, return the lexicographically smaller movie name.

The result must show both answers under a single column.

---

## **Solution Query**
```sql
(
    SELECT u.name results
    FROM Users u
    JOIN MovieRating mr
    ON u.user_id = mr.user_id
    GROUP BY u.user_id
    ORDER BY COUNT(mr.movie_id) DESC, u.name ASC
    LIMIT 1
)
UNION ALL
(
    SELECT m.title results
    FROM Movies m
    JOIN MovieRating mr
    ON m.movie_id = mr.movie_id
    WHERE mr.created_at LIKE '2020-02-%'
    GROUP BY mr.movie_id
    ORDER BY AVG(mr.rating) DESC, m.title ASC
    LIMIT 1
);
```

---

## **Step by Step Explanation

1. Finding the Most Active User

We joined the **Users** and **MovieRating** tables using **user_id**.

**Group by** each user to **count** how many movies they rated.

**Order by COUNT(mr.movie_id)** in descending order to find who rated the most movies.

Used **u.name ASC** as a tiebreaker to ensure alphabetically smaller names come first.

Limited the result to only one row using **LIMIT 1**.


2. Finding the Top Rated Movie in February 2020

We joined the **Movies** and **MovieRating** tables using **movie_id**.

Filtered only reviews made in February 2020 using **WHERE mr.created_at LIKE '2020-02-%'**.

**Group by** movie_id to compute the average rating for each movie.

**Order by AVG(mr.rating)** in descending order to get the highest rating first.

Used **m.title ASC** as a tiebreaker and LIMIT 1 to get the top movie.


3. Combining Both Results

Used **UNION ALL** to merge both results vertically into a single column called results.

The first query returns the top reviewer, and the second returns the top-rated movie.

---

## **Key Takeaway**

Today’s challenge taught me how to handle multiple ranking queries and merge their results cleanly using **UNION ALL**.
I also reinforced how to use aggregate functions **(COUNT, AVG)**, tie-breaking logic **(ORDER BY ASC)**, and pattern matching **(LIKE)** together.
The concept of lexicographical order simply means sorting alphabetically — a small but essential detail in SQL ranking challenges.

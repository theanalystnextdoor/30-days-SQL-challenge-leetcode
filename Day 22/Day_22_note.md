# Day 22 – Combining Transactions and Chargebacks

## Problem
For each month and country, report:
- number of approved transactions and their total amount,
- number of chargebacks and their total amount.

Important detail: **approved transactions use `Transactions.trans_date`**, while **chargebacks use `Chargebacks.trans_date`** (they are separate events). Ignore rows that would show all zeros.

---

## Final SQL Solution

```sql
SELECT month,
       country,
       SUM(approved_count)    AS approved_count,
       SUM(approved_amount)   AS approved_amount,
       SUM(chargeback_count)  AS chargeback_count,
       SUM(chargeback_amount) AS chargeback_amount
FROM (
    -- Part A: approved transactions (grouped by transaction date)
    SELECT 
        DATE_FORMAT(t.trans_date, '%Y-%m') AS month,
        t.country,
        COUNT(*) AS approved_count,
        SUM(t.amount) AS approved_amount,
        0 AS chargeback_count,
        0 AS chargeback_amount
    FROM Transactions t
    WHERE t.state = 'approved'
    GROUP BY DATE_FORMAT(t.trans_date, '%Y-%m'), t.country

    UNION ALL

    -- Part B: chargebacks (grouped by chargeback date; join to get country & amount)
    SELECT 
        DATE_FORMAT(cb.trans_date, '%Y-%m') AS month,
        t.country,
        0 AS approved_count,
        0 AS approved_amount,
        COUNT(*) AS chargeback_count,
        SUM(t.amount) AS chargeback_amount
    FROM Chargebacks cb
    JOIN Transactions t
      ON cb.trans_id = t.id
    GROUP BY DATE_FORMAT(cb.trans_date, '%Y-%m'), t.country
) AS combined
GROUP BY month, country;
```

---

## **Step-by-Step Explanation**

1. Part A (approved transactions)

Use Transactions.trans_date to get the month (DATE_FORMAT(..., '%Y-%m')).

Count approved rows and sum amount. Put zeros into the chargeback columns so shapes match for the union.

2. Part B (chargebacks)

Use Chargebacks.trans_date (the date the chargeback occurred) to get the month.

Join Chargebacks → Transactions on trans_id to know the country and amount of the original transaction.

Count chargebacks and sum the original transaction amount. Put zeros into the approved columns.

UNION ALL

Stack the two result sets; each row contains either approved values or chargeback values (the other columns are zero).

3. Final aggregation

GROUP BY month, country and SUM(...) aggregate the stacked rows into a single row per (month, country), summing approved and chargeback counts/amounts together.

Why this returns months with only chargebacks

Because chargeback rows use Chargebacks.trans_date, a month may have chargebacks but no approved transactions; the UNION approach preserves those months.

---

## **Key Takeaway**

When I first read this I almost tried to force everything into one query — but the moment I realized chargebacks have their own date, the right pattern became obvious: aggregate each event source by its own date, then combine. 
That pattern (aggregate-by-source → union → final aggregate) is simple, robust, and easy to validate — and I now use it whenever different event tables use different date fields.

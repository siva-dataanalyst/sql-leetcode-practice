# 182. Duplicate Emails

**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/duplicate-emails/  
**Topics:** GROUP BY, HAVING

## Problem
Given the `Person` table with columns `id` and `email`, write a query 
to report all the duplicate emails. It's guaranteed that the email 
field is not `NULL`.

## My Approach
To find duplicates, I need to group all rows by `email` and then 
check which groups have **more than one** row — meaning that email 
appears more than once in the table.

`GROUP BY` collapses all rows with the same email into a single group. 
`HAVING COUNT(email) > 1` then filters those groups down to only the 
ones that occurred more than once — i.e., the actual duplicates.

I used `HAVING` instead of `WHERE` because `WHERE` filters rows 
*before* grouping happens, but I need to filter based on the 
**count of a group**, which only exists *after* grouping — that's 
exactly what `HAVING` is for.

## My Solution
```sql
SELECT email AS Email
FROM Person
GROUP BY email
HAVING COUNT(email) > 1;
```

## Result
✅ Accepted — 15/15 test cases passed  
⏱ Runtime: 368 ms (faster than 89.90% of submissions)

## Notes / Learnings
- `WHERE` filters individual rows **before** grouping; `HAVING` 
  filters **groups** after `GROUP BY` has been applied. Any condition 
  involving an aggregate function (like `COUNT`, `SUM`, `MAX`) must go 
  in `HAVING`, not `WHERE`.
- `COUNT(email)` and `COUNT(*)` behave the same here since `email` is 
  guaranteed non-null, but using `COUNT(email)` is a good habit when 
  a column could contain `NULL`s (since `COUNT(column)` ignores 
  `NULL`s while `COUNT(*)` counts all rows).

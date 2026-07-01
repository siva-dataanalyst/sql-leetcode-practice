# 176. Second Highest Salary

**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/second-highest-salary/  
**Topics:** Subquery, Aggregation (MAX)

## Problem
Write a query to find the second highest distinct salary from the 
`Employee` table. If there is no second highest salary, the query 
should report `null`.

- `Employee` table has: id, salary

## My Approach
Instead of ranking every salary, I only need the **highest value that 
is strictly less than the overall maximum salary** — that value is, by 
definition, the second highest.

I first find the maximum salary using a subquery, then filter the 
table to only rows with a salary less than that maximum, and take the 
`MAX()` of what's left. This automatically handles duplicates (e.g. 
two employees tied for highest salary won't cause issues) and also 
handles the case where no second highest exists — `MAX()` on an empty 
result set returns `NULL` automatically, which is exactly what 
LeetCode expects.

## My Solution
```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

## Result
✅ Accepted — 10/10 test cases passed  
⏱ Runtime: 286 ms (faster than 54.28% of submissions)

## Notes / Learnings
- `MAX()` naturally returns `NULL` when there are no matching rows — 
  no need for extra `NULL` handling or `IFNULL()`.
- This approach avoids window functions entirely and is often faster 
  for small/medium tables since it only scans the data twice (once for 
  the inner subquery, once for the outer filter).
- I also tried a `RANK()` window function version of this problem — 
  worth comparing both approaches to see tradeoffs between 
  readability and performance.

# 181. Employees Earning More Than Their Managers

**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/employees-earning-more-than-their-managers/  
**Topics:** Self Join

## Problem
Given the `Employee` table with columns `id`, `name`, `salary`, and 
`managerId`, write a query to find employees who earn more than their 
managers.

## My Approach
Since manager info is stored in the *same* table as employee info 
(via `managerId` referencing another row's `id`), this needs a 
**self join** — joining the `Employee` table to itself using two 
aliases: one representing the employee (`e`), and one representing 
their manager (`m`).

My first attempt tried joining directly on `e.salary > m.salary`, 
which was wrong — that compares *every* employee to *every other* 
employee with a lower salary, not specifically their own manager. 
The fix was to join on the actual relationship first 
(`e.managerId = m.id`), and only then filter by salary.

## My Solution
```sql
SELECT e.name AS Employee
FROM Employee e
JOIN Employee m
ON e.managerId = m.id
WHERE e.salary > m.salary;
```

## Result
✅ Accepted — 14/14 test cases passed  
⏱ Runtime: 427 ms (faster than 26.39% of submissions)

## Notes / Learnings
- A **self join** just means joining a table to itself — always give 
  each "copy" its own alias so SQL knows which role each row is 
  playing (here: `e` = employee, `m` = manager).
- Always join on the actual relationship (foreign key) first — 
  filtering conditions like salary comparisons come *after* the join, 
  in the `WHERE` clause, not as the join condition itself.
- `INNER JOIN` (the default `JOIN`) works here since every employee in 
  the test data has a manager. If some employees had `NULL` 
  managerId, a `LEFT JOIN` would be needed to still include them 
  (though they'd just be excluded from the final result if they have 
  no manager to compare against).

# 175. Combine Two Tables

**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/combine-two-tables/  
**Topics:** Joins (LEFT JOIN)

## Problem
Given two tables, `Person` and `Address`, write a query to report the 
first name, last name, city, and state of each person. If a person's 
address is not present, report `null` instead.

- `Person` table has: personId, firstName, lastName
- `Address` table has: addressId, personId, city, state

## My Approach
Since not every person has an address, a regular (`INNER`) `JOIN` would 
drop people with no matching address row. To keep **all** people 
regardless of whether they have an address, I used a `LEFT JOIN` from 
`Person` to `Address`, matching on `personId`. Rows with no address 
match automatically return `NULL` for `city` and `state`.

## My Solution
```sql
SELECT firstName, lastName, city, state
FROM Person P
LEFT JOIN Address A
ON P.personId = A.personId;
```

## Result
✅ Accepted — 8/8 test cases passed  
⏱ Runtime: 403 ms (faster than 86.14% of submissions)

## Notes / Learnings
- `LEFT JOIN` keeps every row from the left table (`Person`) even when 
  there's no match in the right table (`Address`).
- Using table aliases (`P`, `A`) makes the query more readable when 
  columns need to be qualified.

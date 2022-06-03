# SQL-Leetcode
A repository for solutions to SQL leetcode problems.

## Hard

## Medium

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
```

Write a SQL query to report the second highest salary from the `Employee` table.  If there is no second highest salary, the query should report `null`.

### MY SOLUTION
```SQL
SELECT MAX(salary) AS SecondHighestSalary
FROM employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

## Easy

### QUESTION 175

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| personId    | int     |
| lastName    | varchar |
| firstName   | varchar |
+-------------+---------+
```
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| addressId   | int     |
| personId    | int     |
| city        | varchar |
| state       | varchar |
+-------------+---------+
```

Write a SQL query to report the first name, last name, city, and state of each person in the Person table.

### MY SOLUTION

```SQL
SELECT
    firstName,
    lastName,
    city,
    state
FROM person
LEFT JOIN address
ON person.personid = address.personid;
```

## QUESTION

### MY SOLUTION

```SQL
```

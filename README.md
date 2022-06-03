# SQL-Leetcode
A repository for solutions to SQL leetcode problems.

## Hard

## Medium

### QUESTION 176

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

### QUESTION 184

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
```
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
```

Write n SQL query to find employees who have the highest salary in each of the departments.

### MY SOLUTION

```SQL
SELECT
    d.name AS Department,
    e.name AS Employee,
    e.salary
FROM Employee AS e
JOIN Department AS d
ON e.departmentId = d.id
JOIN (
    SELECT MAX(salary) AS Salary, Departmentid
    FROM Employee
    GROUP BY Departmentid
    ) AS mx
ON e.Salary = mx.Salary AND e.Departmentid = mx.Departmentid;

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

### QUESTION 181

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |
+-------------+---------+
```

Write a SQL query to find the employees who earn more than their manager.

### MY SOLUTION

```SQL
SELECT e.name AS Employee
FROM Employee e
JOIN Employee AS m
ON e.managerId = m.id
WHERE e.salary > m.salary;
```

### QUESTION 182

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
```

Write a SQL query to report all the duplicate emails.

### MY SOLUTION

```SQL
SELECT email
FROM Person
GROUP BY email
HAVING COUNT(email) > 1;
```

### QUESTION 511

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
```

Write a SQL query to report the first login date for each player.

### MY SOLUTION

```SQL
SELECT
    player_id,
    min(event_date) AS first_login
FROM
    Activity
GROUP BY player_id;
```

### QUESTION

### MY SOLUTION

```SQL
```

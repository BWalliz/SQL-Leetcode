# SQL-Leetcode
A repository for solutions to SQL leetcode problems.

## Hard

### QUESTION 185

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

A company's executives are interested in seeing who earns the most money in each of the company's departments. A high earner in a department is an employee who has a salary in the top three unique salaries for that department.

Write a SQL query to find the employees who are high earners in each of the departments.

### MY SOLUTION

```SQL
SELECT
    d.name AS Department,
    e1.name AS Employee,
    e1.salary
FROM Employee e1
JOIN Department d
ON e1.departmentid = d.id
WHERE 3 > (SELECT
          COUNT(DISTINCT e2.salary)
          FROM Employee e2
           WHERE e2.salary > e1.salary AND e1.departmentid = e2.departmentid
            );
```

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

Write a SQL query to find employees who have the highest salary in each of the departments.

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


### QUESTION 607

```
+-----------------+---------+
| Column Name     | Type    |
+-----------------+---------+
| sales_id        | int     |
| name            | varchar |
| salary          | int     |
| commission_rate | int     |
| hire_date       | date    |
+-----------------+---------+
```
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| com_id      | int     |
| name        | varchar |
| city        | varchar |
+-------------+---------+
```
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| order_date  | date |
| com_id      | int  |
| sales_id    | int  |
| amount      | int  |
+-------------+------+
```

Write a SQL query to report the names of all the salespersons who did not have any orders related to the company with the name "RED".

### MY SOLUTION

```SQL
SELECT name
FROM Salesperson
WHERE sales_id NOT IN (
  SELECT o.sales_id
  FROM Orders o
  JOIN Company c ON o.com_id = c.com_id
  WHERE c.name = 'RED'
);
```

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

### QUESTION 196

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
```

Write a SQL query to delete all the duplicate emails, keeping only one unique email with the smallest `id`.

### MY SOLUTION

```SQL
DELETE p2
FROM Person p1
JOIN Person p2
ON p1.email = p2.email
WHERE p1.id < p2.id;
```

### QUESTION 586

```
+-----------------+----------+
| Column Name     | Type     |
+-----------------+----------+
| order_number    | int      |
| customer_number | int      |
+-----------------+----------+
```

Write an SQL query to find the `customer_number` for the customer who has placed the largest number of orders.

### MY SOLUTION

```SQL
SELECT customer_number
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(*) DESC
LIMIT 1;
```

### QUESTION

### MY SOLUTION

```SQL
```

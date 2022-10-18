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
### QUESTION 177

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
```

Write an SQL query to report the nth highest salary from the Employee table. If there is no nth highest salary, the query should report null.

### MY SOLUTION

```SQL
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    SET N = N - 1;
  RETURN (
    SELECT DISTINCT salary
    FROM Employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET N
);
END
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

### QUESTION 180

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
```

Write a SQL query to find all numbers that appear at least three times consecutively.

### MY SOLUTION

```SQL
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM
  Logs l1,
  Logs l2,
  Logs l3
WHERE
  l1.id = l2.id - 1
  AND l2.id = l2.id - 1
  AND l1.num = l2.num
  AND l2.num = l3.num
;
```

### QUESTION 1158

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| join_date      | date    |
| favorite_brand | varchar |
+----------------+---------+
```
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| item_id       | int     |
| buyer_id      | int     |
| seller_id     | int     |
+---------------+---------+
```
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| item_id       | int     |
| item_brand    | varchar |
+---------------+---------+
```

Write a SQL query to find for each user, the join date and the number of orders they made as a buyer in `2019`.

### MY SOLUTION

```SQL
SELECT
    u.user_id AS buyer_id,
    u.join_date,
    SUM(CASE
        WHEN o.order_date >= '2019-01-01' AND o.order_date <= '2019-12-31'
        THEN 1
        ELSE 0
    END
    ) AS orders_in_2019
FROM Users u
LEFT JOIN Orders o
ON o.buyer_id = u.user_id
GROUP BY u.user_id;
```

### QUESTION 1393

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| stock_name    | varchar |
| operation     | enum    |
| operation_day | int     |
| price         | int     |
+---------------+---------+
```

Write a SQL query to report the Capital gain/loss for each stock.

### MY SOLUTION

```SQL
SELECT
    s.stock_name,
    SUM(IF(s.operation = 'BUY', -s.price, s.price)) AS capital_gain_loss
FROM Stocks s
GROUP BY s.stock_name
```

## Easy

### QUESTION 1965

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| name        | varchar |
+-------------+---------+
```
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| salary      | int     |
+-------------+---------+
```

Write an SQL query to report the IDs of all the employees with missing information. The information of an employee is missing if:

* The employee's name is missing, or
* The employee's salary is missing.

### MY SOLUTION

```SQL
SELECT employee_id
FROM Employees
WHERE employee_id NOT IN (SELECT employee_id FROM Salaries)
UNION
SELECT employee_id
FROM Salaries
WHERE employee_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;
```

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
### QUESTION 183

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
```
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| customerId  | int  |
+-------------+------+
```
Write an SQL query to report all customers who never order anything.

### MY SOLUTION

```SQL
SELECT c.name as Customers
FROM Customers c
WHERE c.id NOT IN
(
    SELECT CustomerId FROM Orders
);
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

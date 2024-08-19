# 🎓 SQL Interview Questions

Welcome to the SQL Interview Questions repository! This collection of SQL queries is designed to help you prepare for interviews by providing a variety of practical examples. Each question is accompanied by table data and a detailed answer.

## Database Setup

SQL scripts to create the `departments` and `employees` tables, as well as to insert sample data into them.

## SQL Scripts

### Create the `departments` table 
<details><summary>
:arrow_forward: View Table SQL Query
</summary>

  ```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);
INSERT INTO departments (department_id, department_name) VALUES
(1, 'HR'),
(2, 'Finance'),
(3, 'IT'),
(4, 'Marketing')
(5, 'Civil');
```
</details>
<br> 

### Create the `employees` table 
<details><summary>
:arrow_forward: View Table SQL Query
</summary>
  
```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department_id INT,
    salary DECIMAL(10, 2),
    hire_date DATE,
    manager_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);

INSERT INTO employees (employee_id, first_name, last_name, department_id, salary, hire_date, manager_id) VALUES 
(1, 'John', 'Doe', 1, 50000.00, '2020-01-15', NULL),
(2, 'Jane', 'Smith', 2, 60000.00, '2019-03-10', 1),
(3, 'Jim', 'Brown', 3, 55000.00, '2018-07-23', 1),
(4, 'Jake', 'White', 4, 45000.00, '2021-06-12', 3),
(5, 'Jill', 'Green', 1, 70000.00, '2017-11-19', NULL),
(6, 'Joe', 'Black', 2, 80000.00, '2015-04-29', 2),
(7, 'Jerry', 'Red', 3, 75000.00, '2016-09-14', 3),
(8, 'Jenny', 'Blue', 4, 65000.00, '2019-12-25', 4),
(9, 'Jordan', 'Purple', 1, 48000.00, '2020-08-05', 5),
(10, 'Jamie', 'Orange', 2, 67000.00, '2018-05-21', 6),
(11, 'John', 'Doe', 1, 50000.00, '2020-01-15', NULL)
(12, 'Jamie', 'Orange', 2, 67000.00, '2018-05-21', 6);
```
</details>
<br>

## 1: Finding the MAX and MIN salary from the employees table

<br>

### a. Finding the top 2 maximum salaries of each department❓

<details><summary>
:arrow_forward: View Answer
</summary>

```sql
  SELECT 
  employee_id, 
  first_name, 
  last_name, 
  department_id, 
  salary, 
  hire_date, 
  manager_id 
FROM 
  (
    SELECT 
      employee_id, 
      first_name, 
      last_name, 
      department_id, 
      salary, 
      hire_date, 
      manager_id, 
      DENSE_RANK() OVER (
        PARTITION BY department_id 
        ORDER BY 
          salary DESC
      ) dr 
    FROM 
      employees
  ) temp 
WHERE 
  dr <= 2;
```
</details>
<br>

### b. Finding the top 2 minimum salaries of each department❓

<details><summary>
:arrow_forward: View Answer
</summary>

```sql
  SELECT 
  employee_id, 
  first_name, 
  last_name, 
  department_id, 
  salary, 
  hire_date, 
  manager_id 
FROM 
  (
    SELECT 
      employee_id, 
      first_name, 
      last_name, 
      department_id, 
      salary, 
      hire_date, 
      manager_id, 
      DENSE_RANK() OVER (
        PARTITION BY department_id 
        ORDER BY 
          salary ASC
      ) dr 
    FROM 
      employees
  ) temp 
WHERE 
  dr <= 2;
```
</details>
<br>

### c. Finding the top 2 maximum and minimum salaries of each department❓

<details><summary>
:arrow_forward: View Answer
</summary>
  
```sql
  WITH max_salary AS (
  SELECT 
    employee_id, 
    first_name, 
    last_name, 
    department_id, 
    salary, 
    hire_date, 
    manager_id, 
    DENSE_RANK() OVER (
      PARTITION BY department_id 
      ORDER BY 
        salary DESC
    ) dr 
  FROM 
    employees
), 
min_salary AS (
  SELECT 
    employee_id, 
    first_name, 
    last_name, 
    department_id, 
    salary, 
    hire_date, 
    manager_id, 
    DENSE_RANK() OVER (
      PARTITION BY department_id 
      ORDER BY 
        salary ASC
    ) dr 
  FROM 
    employees
) 
SELECT 
  employee_id, 
  first_name, 
  last_name, 
  department_id, 
  salary, 
  hire_date, 
  manager_id, 
  'MAX' AS salary_type 
FROM 
  max_salary 
WHERE 
  dr <= 2 
UNION 
SELECT 
  employee_id, 
  first_name, 
  last_name, 
  department_id, 
  salary, 
  hire_date, 
  manager_id, 
  'MIN' AS salary_type 
FROM 
  min_salary 
WHERE 
  dr <= 2
```
</details>
<br>

## 2: Interview Question: Using LEAD and LAG Functions 

<br>

### a. Identify Sequences of Available Seats in a Cinema Seating Arrangement❓

#### Create the `cinema_seats` table 
<details><summary>
:arrow_forward: View Table SQL Query
</summary>
  
```sql
CREATE TABLE cinema_seats (
    seat_id INT PRIMARY KEY,
    is_free INT
);

INSERT INTO cinema_seats (seat_id, is_free) VALUES 
(1, 1), (2, 0), (3, 1), (4, 0), 
(5, 1), (6, 1), (7, 1), (8, 0), 
(9, 1), (10, 1);
```
</details>

<details><summary>
:arrow_forward: View Answer
</summary>

```sql
WITH seat_sequence AS (
    SELECT 
        seat_id, 
        is_free,
        LAG(is_free) OVER (ORDER BY seat_id) AS prev_seat_free,
        LEAD(is_free) OVER (ORDER BY seat_id) AS next_seat_free
    FROM cinema_seats
)
SELECT seat_id 
FROM seat_sequence 
WHERE is_free = 1 
  AND (prev_seat_free = 1 OR next_seat_free = 1);
```
</details>
<br>

### b. Identify the Top Performers Who Consistently Outperformed Their Peers❓

#### Create the `employee_performance` table 
<details><summary>
:arrow_forward: View Table SQL Query
</summary>
  
```sql
CREATE TABLE employee_performance (
    employee_id INT,
    month DATE,
    performance_score INT
);

INSERT INTO employee_performance (employee_id, month, performance_score) VALUES
(1, '2024-01-01', 85),
(1, '2024-02-01', 90),
(1, '2024-03-01', 95),
(1, '2024-04-01', 92),
(2, '2024-01-01', 75),
(2, '2024-02-01', 78),
(2, '2024-03-01', 80),
(2, '2024-04-01', 82);
```
</details>

<details><summary>
:arrow_forward: View Answer
</summary>

```sql
WITH performance_ranking AS (
    SELECT 
        employee_id, 
        month, 
        performance_score,
        LAG(performance_score) OVER (PARTITION BY employee_id ORDER BY month) AS prev_month_score,
        LEAD(performance_score) OVER (PARTITION BY employee_id ORDER BY month) AS next_month_score
    FROM employee_performance
)
SELECT 
    employee_id, 
    month, 
    performance_score
FROM 
    performance_ranking
WHERE 
    performance_score > prev_month_score 
    AND performance_score > next_month_score
ORDER BY 
    employee_id;
```
</details>
<br>

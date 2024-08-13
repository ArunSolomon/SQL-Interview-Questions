# 🎓 SQL Interview Questions

Welcome to the SQL Interview Questions repository! This collection of SQL queries is designed to help you prepare for interviews by providing a variety of practical examples. Each question is accompanied by table data and a detailed answer.

# Database Setup

SQL scripts to create the `departments` and `employees` tables, as well as to insert sample data into them.

## SQL Scripts

### Create the `departments` table :question:
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

### Create the `employees` table :question:
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










## 💡 1: Example Question

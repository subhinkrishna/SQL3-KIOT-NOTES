# SQL QUERIES USED IN 3 DAYS CLASS

## DATE FUNCTION NOTES
use testdata;

CREATE TABLE EmployeeSalary (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    salary DECIMAL(10,2)
);

INSERT INTO EmployeeSalary VALUES
(1, 'Arun', 45678.75),
(2, 'Priya', 38999.40),
(3, 'Kiran', 52000.90),
(4, 'Neha', 27550.25),
(5, 'Rahul', -1500.75);

SELECT * FROM EmployeeSalary;

CREATE TABLE EmployeeJoining (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    joining_date DATE
);
INSERT INTO EmployeeJoining VALUES
(1, 'Arun', '2021-05-15'),
(2, 'Priya', '2022-08-20'),
(3, 'Kiran', '2020-12-10'),
(4, 'Neha', '2023-01-05'),
(5, 'Rahul', '2024-06-18');

SELECT * FROM EmployeeJoining;
SELECT CURRENT_DATE();
SELECT joining_date, YEAR(joining_date) AS JoinedYear FROM EmployeeJoining;
SELECT joining_date, MONTH(joining_date) AS JoinedMonth FROM EmployeeJoining;
SELECT joining_date, DAY(joining_date) AS JoinedDay FROM EmployeeJoining;
SELECT joining_date, DATEDIFF(CURRENT_DATE(), joining_date) AS JoinedDayS FROM EmployeeJoining;

SELECT salary, ROUND(salary) AS RoundedSalary FROM EmployeeSalary;
SELECT salary, CEIL(salary) AS CeildedSalary FROM EmployeeSalary;
SELECT salary, FLOOR(salary) AS FlooredSalary FROM EmployeeSalary;
SELECT salary, ABS(salary) AS AbsoluteSalary FROM EmployeeSalary;
SELECT salary, MOD(salary, 10000) AS AbsoluteSalary FROM EmployeeSalary;




----------------------------------------------





## JOIN FUNCTIONS IN SQL
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(50)
);

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    product VARCHAR(50)
);

INSERT INTO Customers VALUES
(1,'Rahul'),
(2,'Priya'),
(3,'Kiran'),
(4,'Neha');

INSERT INTO Orders VALUES
(101,1,'Laptop'),
(102,2,'Mobile'),
(103,1,'Mouse'),
(104,3,'Keyboard');

SELECT * FROM Customers;
SELECT * FROM Orders;
-- SELECT table1.col1, table1.col2 FROM table1 INNER JOIN table2 ON table1.ID=table2.ID; 



-- SELECT Customers.customer_id, Customers.customer_name  FROM Customers INNER JOIN Orders ON Customers.customer_id = Orders.customer_id;

-- INNER JOIN
SELECT c.customer_id, c.customer_name, o.product  FROM Customers c INNER JOIN Orders o
ON c.customer_id = o.customer_id;

-- LEFT JOIN
SELECT c.customer_id, c.customer_name, o.product  FROM Customers c LEFT JOIN Orders o
ON c.customer_id = o.customer_id;

-- RIGHT JOIN
SELECT c.customer_id, c.customer_name, o.product  FROM Customers c RIGHT JOIN Orders o
ON c.customer_id = o.customer_id;

-- FULL JOIN
-- SELECT c.customer_id, c.customer_name, o.product  FROM Customers c FULL JOIN Orders o
-- ON c.customer_id = o.customer_id;




-------------------------------------------



## Conditionals Function in SQL
CREATE TABLE Employees1 (
    emp_id INT,
    emp_name VARCHAR(50),
    salary INT,
    phone VARCHAR(15)
);

INSERT INTO Employees1 VALUES
(101, 'Subhin', 50000, '9876543210'),
(102, 'Rahul', 25000, NULL),
(103, 'Arun', 40000, '9123456789'),
(104, 'Akash', 18000, NULL),
(105, 'John', 60000, '9988776655');

SELECT * FROM Employees1;

SELECT emp_name,
IFNULL(phone, 'Not Available') AS phone FROM Employees1;

SELECT emp_name,
COALESCE(phone, 'Not Available') AS phone FROM Employees1;

SELECT emp_name, salary,
CASE
  WHEN salary >= 30000 then 'High Salary'
ELSE 'Low Salary'
END AS Salary_Status FROM Employees1;

SELECT IFNULL('9876543210', 'No Phone');
SELECT COALESCE(NULL, NULL, 'SQL', 'MongoDB');




------------------------------------



## SUB QUERY
CREATE TABLE Employees2 (
    emp_id INT,
    emp_name VARCHAR(50),
    department VARCHAR(20),
    salary INT
);

INSERT INTO Employees2 VALUES
(101,'Priya','IT',50000),
(102,'Rahul','HR',30000),
(103,'Arun','IT',45000),
(104,'Akash','Sales',25000),
(105,'John','HR',35000);

SELECT * FROM Employees2;

### SINGLE ROW SUB QUERY
SELECT AVG(salary)
    FROM Employees2;

Single Row Sub Query
SELECT *
FROM Employees2
WHERE salary >
(
    SELECT AVG(salary)
    FROM Employees2
);

### NESTED SUB QUERY
SELECT *
FROM Employees2
WHERE salary >
(
    SELECT AVG(salary)
    FROM Employees2
    WHERE department =
    (
        SELECT department
        FROM Employees2
        WHERE emp_name='Rahul'
    )
);

### Multiple Row SUB QUERY
SELECT department
    FROM Employees2
    GROUP BY department;

SELECT *
FROM Employees2
WHERE department IN
(
    SELECT department
    FROM Employees2
    GROUP BY department
    HAVING AVG(salary) > 30000
);

### Correlated SUB QUERY
SELECT emp_name, department, salary
FROM Employees2 e1
WHERE salary >
(
    SELECT AVG(salary)
    FROM Employees2 e2
    WHERE e2.department = e1.department
);


-----------------------------------
## CTE - Common Table Expressions
CREATE TABLE Employees2 (
    emp_id INT,
    emp_name VARCHAR(50),
    department VARCHAR(20),
    salary INT
);

INSERT INTO Employees2 VALUES
(101,'Priya','IT',50000),
(102,'Rahul','HR',30000),
(103,'Arun','IT',45000),
(104,'Akash','Sales',25000),
(105,'John','HR',35000);

SELECT * FROM Employees2;

### Non Recursive
SELECT *
FROM
(
    SELECT emp_name, salary
    FROM Employees2
    WHERE salary > 30000
) AS HighSalaryEmployees;

WITH HighSalary AS
(
    SELECT *
    FROM Employees2
    WHERE salary > 30000
)

SELECT *
FROM HighSalary;

### Recursive
WITH RECURSIVE Numbers AS
(
    SELECT 1 AS num

    UNION ALL

    SELECT num + 1
    FROM Numbers
    WHERE num < 5
)

SELECT *
FROM Numbers;

----------------------------
## VIEW
CREATE TABLE Employees2 (
    emp_id INT,
    emp_name VARCHAR(50),
    department VARCHAR(20),
    salary INT
);
INSERT INTO Employees2 VALUES (101,'Priya','IT',50000),
(102,'Rahul','HR',30000), (103,'Arun','IT',45000),
(104,'Akash','Sales',25000), (105,'John','HR',35000);
SELECT * FROM Employees2;

### CREATE VIEW
CREATE VIEW ITEmployees AS 
SELECT *
FROM Employees2
WHERE department = 'IT';
SELECT * FROM ITEmployees;

### ALTER VIEW
ALTER VIEW ITEmployees AS
SELECT *
FROM Employees2
WHERE salary > 45000;
SELECT * FROM ITEmployees;

### DROP VIEW
DROP VIEW ITEmployees;

SELECT * FROM ITEmployees;

----------------------------------------
## Introduction to Window Function
CREATE TABLE Sales (
    sale_id INT,
    sales_person VARCHAR(50),
    department VARCHAR(20),
    sale_month VARCHAR(10),
    sales_amount INT
);

INSERT INTO Sales VALUES
(1, 'Rahul', 'Electronics', 'Jan', 10000),
(2, 'Priya', 'Electronics', 'Feb', 15000),
(3, 'Arun', 'Furniture', 'Jan', 8000),
(4, 'John', 'Furniture', 'Feb', 12000),
(5, 'Akash', 'Electronics', 'Mar', 20000),
(6, 'Anu', 'Furniture', 'Mar', 10000);

SELECT * FROM Sales;

### OVER()
SELECT
sales_person,
sales_amount,
AVG(sales_amount) OVER() AS AvgSales
FROM Sales;

### PARTITION BY()
SELECT
sales_person,
department,
sales_amount,
AVG(sales_amount) OVER(PARTITION BY department) AS DeptAvg
FROM Sales;

### ORDER BY
CREATE TABLE ProductSales (
    product_id INT,
    product_name VARCHAR(50),
    total_sales INT
);

INSERT INTO ProductSales VALUES
(1, 'Laptop', 50000),
(2, 'Mouse', 10000),
(3, 'Keyboard', 15000),
(4, 'Monitor', 30000),
(5, 'Printer', 20000);

SELECT
product_id,
    product_name,
    total_sales,
    SUM(total_sales) OVER(ORDER BY product_id) AS Running_Total
FROM ProductSales;

------------------------------------------
## Ranking Functions
CREATE TABLE Students (
    student_id INT,
    student_name VARCHAR(50),
    marks INT
);

INSERT INTO Students VALUES
(101, 'Rahul', 95),
(102, 'Priya', 90),
(103, 'Arun', 90),
(104, 'John', 85),
(105, 'Akash', 80);

SELECT *
FROM Students;

### ROW_NUMBER()
SELECT
    student_name,
    marks,
    ROW_NUMBER() OVER (ORDER BY marks DESC) AS Row_No
FROM Students;

### RANK()
SELECT
    student_name,
    marks,
     OVER (ORDER BY marks DESC) AS Rank_No
FROM Students;

### DENSE_RANK()
SELECT
    student_name,
    marks,
    DENSE_RANK() OVER (ORDER BY marks DESC) AS Dense_Rank
FROM Students;

-----------------------------------------
## Analytical Functions
CREATE TABLE Sales (
    sale_id INT,
    sales_person VARCHAR(50),
    sale_month VARCHAR(10),
    sales_amount INT
);

INSERT INTO Sales VALUES
(1,'Rahul','Jan',10000),
(2,'Priya','Feb',15000),
(3,'Arun','Mar',12000),
(4,'John','Apr',18000),
(5,'Akash','May',16000);

### LEAD()
SELECT
sale_id,
sales_person,
sale_month,
sales_amount,
LEAD(sales_amount) OVER(ORDER BY sale_id) AS Next_Mon_Sales
FROM Sales;

### LAG()
SELECT
sale_id,
sales_person,
sale_month,
sales_amount,
LAG(sales_amount) OVER(ORDER BY sale_id) AS Prev_Mon_Sales
FROM Sales;

### FIRST_VALUE()
SELECT
sales_person,
sale_month,
sales_amount,
FIRST_VALUE(sales_amount)
OVER(ORDER BY sale_id) AS First_Sales
FROM Sales;

### LAST_VALUE()
SELECT
sale_id,
sales_person,
sale_month,
sales_amount,
LAST_VALUE(sales_amount)
OVER(ORDER BY sale_id
ROWS BETWEEN UNBOUNDED PRECEDING
AND UNBOUNDED FOLLOWING
)
AS Last_Sales
FROM Sales;

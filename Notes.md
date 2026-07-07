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
### SINGLE ROW SUB QUERY
CREATE TABLE Employees (
    emp_id INT,
    emp_name VARCHAR(50),
    department VARCHAR(20),
    salary INT
);
INSERT INTO Employees VALUES
(101,'Subhin','IT',50000),
(102,'Rahul','HR',30000),
(103,'Arun','IT',45000),
(104,'Akash','Sales',25000),
(105,'John','HR',35000);
SELECT * FROM Employees;
SELECT * FROM Employees WHERE salary > 30000;
SELECT AVG(salary) FROM Employees;
SELECT * FROM Employees WHERE salary > (SELECT AVG(salary) FROM Employees);

### NESTED SUB QUERY
SELECT AVG(salary)
    FROM Employees
    WHERE department =
    (
        SELECT department
        FROM Employees
        WHERE emp_name = 'Arun'
    );

SELECT *
FROM Employees
WHERE salary >
(
    SELECT AVG(salary)
    FROM Employees
    WHERE department =
    (
        SELECT department
        FROM Employees
        WHERE emp_name = 'Arun'
    )
);
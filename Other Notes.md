
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

SELECT
AVG(salary) AS AvgSalary
FROM Employees2;

SELECT
emp_name,
salary,
AVG(salary)  OVER() AS AvgSalary
FROM Employees2;

SELECT
emp_name,
salary,
AVG(salary)  OVER(PARTITION BY department) AS DeptAvgSalary
FROM Employees2;

SELECT
emp_name,
salary,
SUM(salary) OVER(ORDER BY salary) AS RunningTotal
FROM Employees2;


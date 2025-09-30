# Subqueries-and-Nested-Queries
In this database i have created a table Employees having relevant data in which i have performed sql subqueries with 'IN','Exists','=' as mentioned in task

create database company;
use company;
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    DepartmentID INT,
    Salary DECIMAL(10, 2),
    ManagerID INT
);

INSERT INTO Employees(EmployeeID,FirstName,LastName,DepartmentID,Salary,ManagerID) VALUES
(100,'Steven','King',90,24000.00,0),(101,'Neena' ,'Kochhar' ,90,17000.00 ,100),(102,'Lex','De-Haan' ,90,17000.00,100 ),(103,'Alexander','Hunold',60,9000.00,102 ),
(104,'Bruce' ,'Ernst',60,6000.00,103 ),(105,'David' ,'Austin',60,4800.00,103),(106,'Valli','Pataballa',60,4800.00,103),(107,'Diana' ,'Lorentz' ,60,4200.00,103),
(108,'Nancy' ,'Greenberg',100,12000.00 ,101),(109,'Daniel' ,'Faviet',100,9000.00,108),(110,'John','Chen',100,8200.00,108),(111,'Ismael','Sciarra' ,100,7700.00,108),
(112,'Jose Manuel','Urman',100,7800.00,108),(113 ,'Luis','Popp',100,6900.00 ,108),(114,'Den' ,'Raphaely',30,11000.00,100 ),(115,'Alexander','Khoo' ,30,3100.00,114);

-- Retrieve the names of employees who earn more than the average salary of all employees.
SELECT FirstName, LastName, Salary
    FROM Employees
    WHERE Salary > (SELECT AVG(Salary) FROM Employees);

-- Find employees who earn more than the average salary in their respective departments.     
SELECT E1.FirstName, E1.LastName, E1.Salary, E1.DepartmentID
    FROM Employees E1
    WHERE E1.Salary > (SELECT AVG(E2.Salary) FROM Employees E2 WHERE E2.DepartmentID = E1.DepartmentID); 
  
  -- Identify employees who are managers
SELECT E1.FirstName, E1.LastName
    FROM Employees E1
    WHERE EXISTS (SELECT 1 FROM Employees E2 WHERE E2.ManagerID = E1.EmployeeID);  
 
 -- Find the names of employees who work in departments that have more than 5 employees.
SELECT FirstName, LastName
    FROM Employees
    WHERE DepartmentID IN (SELECT DepartmentID FROM Employees GROUP BY DepartmentID HAVING COUNT(*) > 5);    
    
-- Find the employee(s) who earn the same salary as the employee with employee_id = 101.
 SELECT FirstName, LastName, Salary
    FROM Employees
    WHERE Salary = (SELECT Salary FROM Employees WHERE EmployeeID= 101); 

-- Retrieve all employees who work in the same department as 'Steven King'.
SELECT FirstName, LastName, DepartmentID
    FROM Employees
    WHERE DepartmentID = (SELECT DepartmentID FROM Employees WHERE FirstName = 'Steven' AND LastName = 'King'); 
    
-- List employees whose department_id matches the department_id of the employee with employee_id = 105
SELECT e.FirstName,e.LastName, e.DepartmentID
    FROM Employees e
    WHERE e.DepartmentID = (SELECT DepartmentID FROM Employees WHERE EmployeeID = 105);    

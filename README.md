# 📊 SQL Practice Queries – Employee, Department & Project Management

This repository contains a set of 36 SQL queries covering a variety of real-world scenarios related to employee management, departmental structure, and project assignments. These queries demonstrate the use of JOINs, GROUP BY, HAVING, subqueries, aggregations, and filtering techniques in SQL.

### 🗃️ Database Tables Involved


<ul data-start="516" data-end="728">
<li data-start="516" data-end="597" class="" style="">
<p data-start="518" data-end="597" class=""><strong data-start="518" data-end="531">Employees</strong> (<code data-start="533" data-end="540">EmpID</code>, <code data-start="542" data-end="551">EmpName</code>, <code data-start="553" data-end="561">Gender</code>, <code data-start="563" data-end="571">Salary</code>, <code data-start="573" data-end="581">DeptID</code>, <code data-start="583" data-end="596">JoiningDate</code>)</p>
</li>
<li data-start="598" data-end="638" class="" style="">
<p data-start="600" data-end="638" class=""><strong data-start="600" data-end="615">Departments</strong> (<code data-start="617" data-end="625">DeptID</code>, <code data-start="627" data-end="637">DeptName</code>)</p>
</li>
<li data-start="639" data-end="682" class="" style="">
<p data-start="641" data-end="682" class=""><strong data-start="641" data-end="653">Projects</strong> (<code data-start="655" data-end="666">ProjectID</code>, <code data-start="668" data-end="681">ProjectName</code>)</p>
</li>
<li data-start="683" data-end="728" class="" style="">
<p data-start="685" data-end="728" class=""><strong data-start="685" data-end="705">EmployeeProjects</strong> (<code data-start="707" data-end="714">EmpID</code>, <code data-start="716" data-end="727">ProjectID</code>)</p>
</li>
</ul>

### ✅ Table Scripts

``` SQL
-- Department table
CREATE TABLE Departments (
    DeptID INT PRIMARY KEY,
    DeptName VARCHAR(50)
);

-- Employee table
CREATE TABLE Employees (
    EmpID INT PRIMARY KEY,
    EmpName VARCHAR(100),
    Gender VARCHAR(10),
    Salary DECIMAL(10, 2),
    DeptID INT,
    JoiningDate DATE,
    FOREIGN KEY (DeptID) REFERENCES Departments(DeptID)
);

-- Project table
CREATE TABLE Projects (
    ProjectID INT PRIMARY KEY,
    ProjectName VARCHAR(100),
    StartDate DATE,
    EndDate DATE
);

-- EmployeeProject table (Many-to-Many)
CREATE TABLE EmployeeProjects (
    EmpID INT,
    ProjectID INT,
    AllocationDate DATE,
    PRIMARY KEY (EmpID, ProjectID),
    FOREIGN KEY (EmpID) REFERENCES Employees(EmpID),
    FOREIGN KEY (ProjectID) REFERENCES Projects(ProjectID)
);
```

### 🧪 Sample Data (You can insert in your DB)
``` SQL
-- Departments
INSERT INTO Departments VALUES (1, 'HR'), (2, 'IT'), (3, 'Finance');

-- Employees
INSERT INTO Employees VALUES 
(101, 'Amit', 'Male', 60000, 2, '2020-01-15'),
(102, 'Priya', 'Female', 75000, 2, '2019-03-10'),
(103, 'Ravi', 'Male', 50000, 1, '2021-06-01'),
(104, 'Sneha', 'Female', 80000, 3, '2018-09-23'),
(105, 'John', 'Male', 55000, 1, '2022-07-05');

-- Projects
INSERT INTO Projects VALUES 
(201, 'Project A', '2021-01-01', '2021-12-31'),
(202, 'Project B', '2020-06-01', '2021-06-30'),
(203, 'Project C', '2022-01-15', '2022-12-15');

-- EmployeeProjects
INSERT INTO EmployeeProjects VALUES 
(101, 201, '2021-01-10'),
(102, 202, '2020-06-15'),
(103, 201, '2021-02-01'),
(104, 203, '2022-01-20'),
(105, 203, '2022-02-01'),
(101, 203, '2022-01-25');
```
### ✅ List of SQL Queries

1.List all employees with their names, gender, salary, and department name.
```
select e.EmpName,e.Gender,e.Salary,d.DeptName from Employees e inner join Departments d on e.DeptID=d.DeptID;
```
2.List the names of all employees who joined after January 1, 2020.
```
Select e.EmpName from Employees e where e.JoiningDate>'2020-01-01';
```
3.Show the total number of employees in each department. Display department name and employee count.
```
select d.DeptName,Count(e.EmpID) EmployeeCount from Employees e inner join Departments d on e.DeptID =d.DeptID group by d.DeptName
```
4.Find the highest salary in each department. Show department name and highest salary.
```
select d.DeptName,Max(e.Salary) HighestSalary from Employees e inner join Departments d on e.DeptID=d.DeptID group by d.DeptName
```
5.List the names of employees who are working on more than one project.
```
select e.EmpName from Employees e inner join EmployeeProjects ep on e.EmpID=ep.EmpID group by e.EmpName having Count(ep.ProjectID)>1
```
6.Display project name and number of employees working on each project.
```
select p.ProjectName,Count(ep.EmpID) EmployeeCount from Projects p inner join EmployeeProjects ep on p.ProjectID=ep.ProjectID group by p.ProjectName
```
7.List all employees (names) who are not assigned to any project.
```
select e.EmpName from Employees e left join EmployeeProjects ep on e.EmpID=ep.EmpID where ep.EmpID is  null
```
8.Show the average salary of employees in each department. Show department name and average salary.
```
select d.DeptName,Avg(e.Salary) AvgSalary from Employees e inner join Departments d on e.DeptID=d.DeptID group by d.DeptName
```
9.Display the names of employees who work on the project named 'Project C'.
```
select e.EmpName from Employees e inner join EmployeeProjects ep on e.EmpID=ep.EmpID inner join Projects p on ep.ProjectID=p.ProjectID where p.ProjectName='Project C';
```
10.List the names of employees along with their department name and number of projects they are assigned to.
```
select e.EmpName,d.DeptName,Count(ep.EmpID) ProjectCount from Employees e inner join Departments d on e.DeptID=d.DeptID inner join EmployeeProjects ep on e.EmpID=ep.EmpID group by e.EmpName,d.DeptName
```
11.Find the name(s) of the department(s) that have more than one employee.
```
select d.DeptName from Employees e inner join Departments d on e.DeptID=d.DeptID group by d.DeptName having Count(e.DeptID)>1
```
12.Find the names of employees who joined in the year 2022.
```
select e.EmpName from Employees e where Year(JoiningDate)=2022
```
13.List the names of employees who have worked on at least 2 projects.
```
select e.EmpName from Employees e inner join EmployeeProjects ep on e.EmpID=ep.EmpID group by e.EmpName having Count(ep.EmpID) >2;
```
14.Show the names of employees who earn more than the average salary of all employees.
```
select e.EmpName from Employees e where Salary>(select Avg(salary) from Employees);
```
15.Find the name of the department with the highest average salary.
```
select top 1 d.DeptName,Avg(e.Salary) AvgSalary from Employees e inner join Departments d on e.DeptID=d.DeptID group by d.DeptName order by Avg(e.Salary) desc
```
16.List the departments with no employees.
```
select d.DeptName from Departments d left join Employees e on d.DeptID=e.DeptID where e.EmpID is null
```
17.Find the names of employees who have the highest salary in each department.
```
select  e.EmpName, e.Salary,d.DeptName from Employees e inner join Departments d on e.DeptID=d.DeptID where e.Salary= (select Max(salary) from Employees a where e.DeptID=a.DeptID)
```
18.List the projects that no employee is assigned to.
```
select p.ProjectName from Projects p left join EmployeeProjects ep on p.ProjectID=ep.ProjectID where ep.EmpID is null
```
19.Display each department’s name along with the number of employees and their total combined salary.
```
select d.DeptName,Count(e.EmpID) EmployeeCount,Sum(e.Salary) CombinedSalary from Employees e inner join Departments d on e.DeptID=d.DeptID  group by d.DeptName
```
20.Find the employees who are assigned to all the available projects.
```
select e.EmpName from Employees e inner join EmployeeProjects ep on e.EmpID=ep.EmpID group by e.EmpID,e.EmpName having Count( distinct ep.ProjectID) =(select Count(p.ProjectID) from Projects p)
```
21.Find the second highest salary from the Employees table.
```
select Max(Salary) SecondHighestSalary from Employees e where e.Salary<(select Max(Salary) from Employees);
select e.salary from Employees e order by e.Salary desc Offset 1 rows fetch next 1 rows only;
```
22.Find the employees who have worked on more than one project but less than three projects.
```
SELECT e.EmpName FROM Employees e INNER JOIN EmployeeProjects ep ON e.EmpID = ep.EmpID GROUP BY e.EmpID, e.EmpName HAVING COUNT(DISTINCT ep.ProjectID) > 1 AND COUNT(DISTINCT ep.ProjectID) < 3;
```
23.Find the departments that have the highest number of employees.
```
select top 1 d.DeptName,Count(e.EmpID) EmployeeCount from Employees e inner join Departments d on e.DeptID=d.DeptID group by d.DeptName order by Count(e.EmpID) desc
```
24.Find employees who joined in the same year as any other employee (i.e., year of joining is not unique).
```
select e.EmpName from Employees e where Year(e.JoiningDate) in (select Year(JoiningDate) from Employees group by YEAR(JoiningDate) having COUNT(EmpID)>1)
```
25.List all employees along with the number of projects they are working on. Also include employees who are not assigned to any project.
```
select e.EmpName,Count(ep.ProjectID) ProjectCount from Employees e left join EmployeeProjects ep on e.EmpID=ep.EmpID group by e.EmpName
```
26.Find the department which has the lowest average salary.
```
select top 1 d.DeptName,Avg(e.Salary) AvgSalary from Employees e inner join Departments d on e.DeptID=d.DeptID group by d.DeptName order by Avg(e.Salary) asc
```
27.List the names of employees who are not assigned to any department.
```
select * from Employees e where e.DeptID is null
```
28.List all projects that have at least 2 employees working on them.
```
select p.ProjectName from Projects p inner join EmployeeProjects ep on p.ProjectID=ep.ProjectID group by p.ProjectName having Count(ep.EmpID) >1;
```
29.Show the names of employees who are working on both Project A and Project B.
```
SELECT e.EmpName FROM Employees e INNER JOIN EmployeeProjects ep ON e.EmpID = ep.EmpID INNER JOIN Projects p ON ep.ProjectID = p.ProjectID WHERE p.ProjectName IN ('Project A', 'Project B') GROUP BY e.EmpID, e.EmpName HAVING COUNT(DISTINCT p.ProjectName) = 2;
```
30.List the departments which have no employees assigned to them.
```
select d.DeptName from Departments d left join Employees e on d.DeptID=e.DeptID where e.DeptID is null;
```
31.Find the employee(s) with the highest salary in the company. (Just name(s), not amount.)
```
select top 1 e.EmpName from Employees e order by Salary desc
```
32.Find the departments where employees have more than one project assigned.
```
select d.DeptName from Employees e inner join EmployeeProjects ep on e.EmpID=ep.EmpID inner join Departments d on e.DeptID=d.DeptID group by d.DeptName having Count(ep.ProjectID) > 1
```
33.Show the names of employees who joined in the same month (any month) across any year.
```
select e.EmpName,MONTH(JoiningDate) JoiningDate from Employees e where MONTH(JoiningDate) in (select MONTH(JoiningDate) from Employees a group by MONTH(a.JoiningDate) having Count(*) > 1)
```
34.Find the employees who are working on exactly 2 projects.
```
select e.EmpName from Employees e inner join EmployeeProjects ep on e.EmpID=ep.EmpID group by e.EmpName having Count(ep.ProjectID) =2
```
35.List the names of employees along with the number of departments they are associated with.
```
select e.EmpName,Count(d.DeptID) CountOfDept from Employees e inner join Departments d on e.DeptID=d.DeptID group by e.EmpName 
```
36.List all employees who are not assigned to any project, but do belong to a department.
```
select e.EmpName from Employees e left join EmployeeProjects ep on e.EmpID=ep.EmpID where ep.EmpID is null and e.DeptID is null
```


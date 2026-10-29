🚀 SELECT, WHERE, ORDER BY
📊 Table: Employees
| id | name   | salary | department | age |
| -- | ------ | ------ | ---------- | --- |
| 1  | Aalia  | 50000  | IT         | 24  |
| 2  | Rahul  | 60000  | HR         | 28  |
| 3  | Simran | 55000  | IT         | 26  |
| 4  | Aman   | 70000  | Sales      | 30  |
| 5  | Neha   | 48000  | HR         | 25  |

# Q1 Retrieve all columns from the Employees table
- SELECT * FROM Employees;

# Q2 Retrieve only name and salary
- SELECT name, salary FROM Employees;

# Q3 Find employees with salary greater than 50000
- SELECT * FROM Employees WHERE salary > 50000;

# Q4 Find employees who belong to the IT department
- SELECT * FROM Employees WHERE department = "IT";

# Q5 Find employees whose age is less than 27
- SELECT * FROM Employees WHERE age < 27;

# Q6 Find employees with salary between 50000 and 70000
- SELECT * FROM Employees WHERE salary BETWEEN 50000 AND 70000;

# Q7 Find employees who are in HR or IT department
- SELECT * FROM Employees 
WHERE department IN ("HR", "IT");

# Q8 Sort employees by salary in ascending order
- SELECT * FROM Employees ORDER BY salary ASC;

# Q9 Sort employees by age in descending order
- SELECT * FROM Employees ORDER BY age DESC;

# Q10 Find top 3 employees with highest salary
- SELECT * FROM Employees ORDER BY salary DESC LIMIT 3;
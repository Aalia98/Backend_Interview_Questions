GROUP BY + HAVING
📊 Same Table: Employees
| id | name   | salary | department | age |
| -- | ------ | ------ | ---------- | --- |
| 1  | Aalia  | 50000  | IT         | 24  |
| 2  | Rahul  | 60000  | HR         | 28  |
| 3  | Simran | 55000  | IT         | 26  |
| 4  | Aman   | 70000  | Sales      | 30  |
| 5  | Neha   | 48000  | HR         | 25  |

# Q1 Find total salary of each department
- SELECT department, SUM(salary) AS total
FROM Employees
GROUP BY department;

# Q2 Find average salary of each department
- SELECT department, AVG(salary) AS avg_salary
FROM Employees
GROUP BY department;

# Q3 Count number of employees in each department
- SELECT department, COUNT(name)
FROM Employees
GROUP BY department;

# Q4 Find maximum salary in each department
- SELECT department, MAX(salary) AS maxi
FROM Employees
GROUP BY department;

# Q5 Find minimum age in each department
- SELECT department, MIN(age) AS mini_age
FROM Employees
GROUP BY department;

# Q6 Find departments where total salary is greater than 100000
- SELECT department
FROM Employees
GROUP BY department
HAVING SUM(salary) > 100000;

# Q7 Find departments having more than 1 employee
- SELECT department, COUNT(*) 
FROM Employees G
GROUP BY department 
HAVING COUNT(*) > 1;

# Q8 Find average salary of departments where average salary > 55000
- SELECT department, AVG(salary) as avg_salary 
FROM Employees 
GROUP BY department 
HAVING AVG(salary) > 55000;

# Q9 Find departments where maximum salary is greater than 60000
- SELECT department, MAX(salary) as maxi 
FROM Employees 
GROUP BY department 
HAVING MAX(salary) > 60000;

# Q10 Find departments where minimum age is less than 25
- SELECT department, MIN(age) as min_age 
FROM Employees 
GROUP BY department 
HAVING MIN(age) < 25;
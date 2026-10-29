🚀 JOINS
📊 Tables
Employees
| id | name   | dept_id | salary |
| -- | ------ | ------- | ------ |
| 1  | Aalia  | 101     | 50000  |
| 2  | Rahul  | 102     | 60000  |
| 3  | Simran | 101     | 55000  |
| 4  | Aman   | 103     | 70000  |
| 5  | Neha   | NULL    | 48000  |

Departments
| dept_id | dept_name |
| ------- | --------- |
| 101     | IT        |
| 102     | HR        |
| 103     | Sales     |
| 104     | Marketing |

# Q1 Get employee name and department name
- SELECT e.name, d.dept_name
FROM Employees e
INNER JOIN departments d
ON e.dept_id = d.dept_id;

# Q2 Get all employees with department names (even if no department)
- SELECT e.name, d.dept_name
FROM Employees e
LEFT JOIN departments d
ON e.dept_id = d.dept_id;

# Q3 Get all departments with employees (even if no employees)
- SELECT d.dept_name, e.name
FROM departments d
LEFT JOIN Employees e
ON d.dept_id = e.dept_id;

# Q4 Get employees who do NOT belong to any department
- SELECT e.name
FROM Employees e
LEFT JOIN departments d
ON e.dept_id = d.dept_id
WHERE d.dept_id IS NULL;

# Q5 Get employee name, salary, and department name only for IT department
- SELECT e.name, e.salary, d.dept_name
FROM Employees e
JOIN departments d
ON e.dept_id = d.dept_id
WHERE d.dept_name = "IT";

# Q6 Count number of employees in each department
- SELECT d.dept_name, COUNT(e.id) AS total_employees
FROM departments d
LEFT JOIN Employees e
ON e.dept_id = d.dept_id
GROUP BY d.dept_name;

# Q7 Find departments with no employees
- SELECT d.dept_name
FROM Departments d
LEFT JOIN Employees e
ON d.dept_id = e.dept_id
WHERE e.dept_id IS NULL;

# Q8 Get highest salary in each department
- SELECT d.dept_name, MAX(e.salary) AS highest_salary
FROM Departments d
LEFT JOIN Employees e
ON d.dept_id = e.dept_id
GROUP BY d.dept_name;

# Q9 Show all employees and if no department, show "No Department" instead of NULL
- SELECT e.name, COALESCE(d.dept_name, 'No Department') AS dept
FROM Employees e
LEFT JOIN Departments d
ON d.dept_id = e.dept_id;
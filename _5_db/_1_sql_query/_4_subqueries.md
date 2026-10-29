🚀 SUBQUERIES
📊 Table: Employees
| id | name   | salary | department |
| -- | ------ | ------ | ---------- |
| 1  | Aalia  | 50000  | IT         |
| 2  | Rahul  | 60000  | HR         |
| 3  | Simran | 55000  | IT         |
| 4  | Aman   | 70000  | Sales      |
| 5  | Neha   | 48000  | HR         |

# Q1 Find employees who have salary greater than average salary
- SELECT *
FROM Employees
WHERE salary > (
    SELECT AVG(salary) FROM Employees
);

# Q2 Find employee(s) with highest salary
- SELECT *
FROM Employees
WHERE salary = (
    SELECT MAX(salary) FROM Employees
);

# Q3 Find employee(s) with lowest salary
- SELECT *
FROM Employees
WHERE salary = (
    SELECT MIN(salary) FROM Employees
);

# Q4 Find employees who work in the same department as Aalia
- SELECT *
FROM Employees
WHERE department = (
    SELECT department
    FROM Employees
    WHERE name = 'Aalia'
);

# Q5 Find employees whose salary is greater than Rahul’s salary
- SELECT *
FROM Employees
WHERE salary > (
    SELECT salary
    FROM Employees
    WHERE name = 'Rahul'
);

# Q6 Find departments whose average salary is greater than overall average salary
- SELECT department, AVG(salary)
FROM Employees
GROUP BY department
HAVING AVG(salary) > (
    SELECT AVG(salary)
    FROM Employees
);

# Q7 Find second highest salary
- SELECT MAX(salary)
FROM Employees
WHERE salary < (
    SELECT MAX(salary)
    FROM Employees
);

# Q8 Find employees who earn more than average salary of their own department
- SELECT *
FROM Employees e1
WHERE salary > (
    SELECT AVG(salary)
    FROM Employees e2
    WHERE e1.department = e2.department
);

# Q9 Find departments having more employees than HR department
- SELECT department, COUNT(*)
FROM Employees
GROUP BY department
HAVING COUNT(*) > (
    SELECT COUNT(*)
    FROM Employees
    WHERE department = 'HR'
);

# Q10 Find employees who are NOT in departments where average salary is below 50000
- SELECT *
FROM Employees
WHERE department NOT IN (
    SELECT department
    FROM Employees
    GROUP BY department
    HAVING AVG(salary) < 50000
);

# Q11 Find employees who have the highest salary in their department
- SELECT *
FROM Employees e1
WHERE salary = (
    SELECT MAX(salary)
    FROM Employees e2
    WHERE e1.department = e2.department
);

# Q12 Find departments where all employees earn more than 50000
- SELECT department
FROM Employees
GROUP BY department
HAVING MIN(salary) > 50000;

# Q13 Find employees whose salary is equal to any employee in HR department
- SELECT *
FROM Employees
WHERE salary IN (
    SELECT salary
    FROM Employees
    WHERE department = 'HR'
);

# Q14 Find employees whose salary is greater than all employees in HR department
- SELECT *
FROM Employees
WHERE salary > (
    SELECT MAX(salary)
    FROM Employees
    WHERE department = 'HR'
);

# Q15 Find duplicate salaries
- SELECT salary, COUNT(*)
FROM Employees
GROUP BY salary
HAVING COUNT(*) > 1;
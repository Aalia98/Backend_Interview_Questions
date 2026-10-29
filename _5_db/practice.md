## 🟡 Easy Level

# Q1. Get all users whose age is greater than 25
SELECT * FROM users WHERE age > 25;

# Q2. Get all users from city = 'Delhi'
SELECT * FROM users WHERE city = 'Delhi';

# Q3. Count total number of users
SELECT COUNT(*) FROM users;

# Q4. Find maximum salary
SELECT MAX(salary) FROM employees;

# Q5. Get distinct cities
SELECT DISTINCT city FROM users;

## 🟡 Medium Level

# Q6. Find number of users in each city
SELECT city, COUNT(*) 
FROM users 
GROUP BY city;

# Q7. Find users who have placed at least one order
SELECT DISTINCT u.*
FROM users u
JOIN orders o ON u.id = o.user_id;

# Q8. Find total order amount for each user
SELECT user_id, SUM(amount) AS total_amount
FROM orders
GROUP BY user_id;

# Q9. Find users who have never placed an order
SELECT u.*
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.id IS NULL;

# Q10. Find departments having more than 3 employees
SELECT department, COUNT(*) 
FROM employees
GROUP BY department
HAVING COUNT(*) > 3;

## 🟡 Hard Level

# Q11. Find the second highest salary
SELECT MAX(salary)
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

# Q12. Find users who placed orders worth more than 10,000 in total
SELECT user_id, SUM(amount) AS total
FROM orders
GROUP BY user_id
HAVING SUM(amount) > 10000;

# Q13. Find duplicate emails
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

# Q14. Get latest order of each user
SELECT o.*
FROM orders o
JOIN (
  SELECT user_id, MAX(order_date) AS last_date
  FROM orders
  GROUP BY user_id
) t ON o.user_id = t.user_id AND o.order_date = t.last_date;

# Q15. Rank employees by salary (highest first)
SELECT *,
       RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;

## 🔥 Bonus Interview Challenges

# Q16. Delete duplicate users keeping only one
DELETE FROM users
WHERE id NOT IN (
  SELECT MIN(id)
  FROM users
  GROUP BY email
);

# Q17. Increase salary by 10% for IT department
UPDATE employees
SET salary = salary * 1.10
WHERE department = 'IT';

# Q18. Find employees earning above department average
SELECT *
FROM employees e
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
  WHERE department = e.department
);
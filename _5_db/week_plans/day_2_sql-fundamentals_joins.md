## Part 1 — SD1 SQL Interview Questions

# 1. What is SQL?

SQL stands for Structured Query Language. It is used to interact with relational databases to create, retrieve, update, and delete data, as well as define database structures and constraints.

Examples: SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER, DROP

# 2. SQL vs MySQL?

SQL is a language used to interact with relational databases, while MySQL is a relational database management system that implements SQL.

# 3. What is SELECT?

SELECT is used to retrieve data from one or more tables.

Example: SELECT name, email FROM users;

# 4. WHERE vs HAVING?

- WHERE is used to filter rows before grouping, while HAVING is used to filter data after aggregation.
- WHERE works before GROUP BY, while HAVING works after GROUP BY.

For example, if I want employees with salary > 50,000, I use WHERE.
But if I want departments where average salary > 50,000, I use HAVING.

# 5. What is GROUP BY?

GROUP BY groups rows with the same values in one or more columns so that aggregate functions can be applied to each group.

# 6. What are aggregate functions?

Aggregate functions calculate a value from multiple rows.

Common functions:

COUNT()
SUM()
AVG()
MIN()
MAX()

# 7. What is a JOIN?

A JOIN combines rows from two or more tables based on a related column or condition.

Example:

SELECT u.name, o.amount
FROM users u
JOIN orders o
    ON u.id = o.user_id;

# 8. INNER JOIN vs LEFT JOIN?

INNER JOIN returns only matching records from both tables, while LEFT JOIN returns all records from the left table and matching records from the right table. If there is no match, right-side columns are NULL.

# 9. What happens when LEFT JOIN has no match?

The columns from the right table become NULL.

# 10. Why does a JOIN sometimes return duplicate rows?

Because JOINs operate on matching rows, not necessarily unique entities.

A JOIN can produce multiple rows for one entity when there are multiple matching rows on the other side of the relationship, especially in one-to-many relationships.

# 11. What is DISTINCT?

- DISTINCT removes duplicate combinations from the selected result.
- DISTINCT operates on the entire selected row, not just one column conceptually.

# 12. What is a self join?

A self join is when a table is joined with itself.

Example:

Employees

id | name  | manager_id
---|-------|-----------
1  | Aalia | NULL
2  | Rahul | 1
3  | Neha  | 1

Find employee + manager:

SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.id;

Here:

employees e → employee
employees m → manager

Same table, two aliases.

# 13. What is a subquery?

A subquery is a query nested inside another SQL query.

Example:

SELECT *
FROM products
WHERE price > (
    SELECT AVG(price)
    FROM products
);

The inner query calculates average price.
The outer query finds products above that average.

# 14. JOIN vs Subquery?

JOIN - Useful when you need to combine data from multiple tables.

SELECT u.name, o.amount
FROM users u
JOIN orders o
ON u.id = o.user_id;

Subquery - Useful when one query depends on the result of another query.

SELECT *
FROM products
WHERE price > (
    SELECT AVG(price)
    FROM products
);

# 15. What happens if we forget the JOIN condition?

Without a join condition, we can get a Cartesian product where every row from one table is combined with every row from the other table, potentially producing a huge and incorrect result set.

# 16. How do you find records with no matching record?

SELECT u.*
FROM users u
LEFT JOIN orders o
    ON u.id = o.user_id
WHERE o.id IS NULL;

Alternative

SELECT *
FROM users u
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.user_id = u.id
);

# 17. How would you find the second-highest value?

SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);

Another common approach:

SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
Interview tip

If asked this question, first clarify:
"Do you mean the second-highest distinct salary?"

# 18. How would you find the top 5 products by sales?

Assume:

order_items
------------
product_id
quantity

SELECT
    product_id,
    SUM(quantity) AS total_sold
FROM order_items
GROUP BY product_id
ORDER BY total_sold DESC
LIMIT 5;

# 19. How would you find users who haven't placed an order?

SELECT u.*
FROM users u
LEFT JOIN orders o
    ON u.id = o.user_id
WHERE o.id IS NULL;

Alternative

SELECT *
FROM users u
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.user_id = u.id
);

# 20. How would you find the highest-spending(amount) customer?

How we think about it -> Users + Orders

SELECT
    u.id,
    u.name,
    SUM(o.amount) AS total_spent
FROM users u
JOIN orders o
    ON u.id = o.user_id
GROUP BY u.id, u.name
ORDER BY total_spent DESC
LIMIT 1;

## Part 2 - SQL Practice

Use:

    users
    -----
    id
    name
    email

    orders
    ------
    id
    user_id
    amount
    status
    created_at
    Practice 1

# 1. Find all users.

- SELECT * FROM users;

# 2. Find users whose name is "Aalia".

SELECT * 
FROM users 
WHERE name = 'Aalia';

# 3. Find orders with amount greater than 1000.

SELECT *
FROM orders
WHERE amount > 1000;

# 4. Find the 5 most expensive orders.

SELECT *
FROM orders
ORDER BY amount DESC
LIMIT 5;

# 5. Find the total number of orders.

- SELECT COUNT(*) FROM orders;

# 6. Find total amount spent by each user.

SELECT user_id, SUM(amount) AS total_spent
FROM orders
GROUP BY user_id;

# 7. Find users who have placed at least one order.

SELECT DISTINCT u.*
FROM users u
INNER JOIN orders o
ON u.id = o.user_id;

# 8. Find all users, including users who have never placed an order.

SELECT u.*
FROM users u
LEFT JOIN orders o
ON u.id = o.user_id;

# 9. Find users who have never placed an order.

SELECT u.*
FROM users u
LEFT JOIN orders o
ON u.id = o.user_id
WHERE o.id IS NULL;

# 10. Find users whose total order amount is greater than 10,000.

SELECT u.id, u.name, SUM(o.amount) AS total
FROM users u
JOIN orders o
    ON u.id = o.user_id
GROUP BY u.id, u.name
HAVING SUM(o.amount) > 10000;

## Part 3 - Design Exercise

# Suppose your e-commerce admin dashboard needs:

Customer name, number of orders, and total amount spent for every customer — including customers who have never ordered anything.

Expected result:

name  | order_count | total_spent
------|-------------|------------
Aalia | 5           | 12000
Rahul | 2           | 5000
Neha  | 0           | 0

# Write the SQL query.

SELECT 
    u.id, 
    u.name, 
    COUNT(o.id) AS order_count, 
    COALESCE(SUM(o.amount), 0) AS total_spent
FROM users u
LEFT JOIN orders o
    ON u.id = o.user_id
GROUP BY u.id, u.name;


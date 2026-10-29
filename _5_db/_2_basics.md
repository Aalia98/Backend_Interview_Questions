## DATABASES
# What is database and its types?

- A database is an organized collection of data that allows applications to store, retrieve, update, and manage data efficiently.
- Types of Database :
  - Relational : Data stores in tables. for ex - MySQL, PostgreSQL, ORACLE, Microsoft SQL Server.
  - Non-Relational : Data not stored in tables. for ex - mongoDB

# DBMS?

- A software application used to manage our DB is called database management system (DBMS).

# RDBMS?

- An RDBMS is a database management system that stores data in related tables and uses keys and constraints to maintain relationships and data integrity.

## 1. SQL Basics
# Q1. What is SQL?

- SQL is a programming language used to interact with relational databases.
- It is used to perform CRUD operations : Create, Read, Update, Delete

# Q2. What is a table, row, and column?

- Table → Collection of data (like a spreadsheet)
- Row → One record
- Column → One attribute/field

# Schema?

- A database schema defines the structure of the database, including tables, columns, data types, constraints, and relationships.

# Constraints and types of constraints?

- A constraint is a rule that controls what data can be stored.

- NOT NULL : columns cannot have a null value
- UNIQUE : all values in column are different
- PRIMARY KEY : makes a column unique & not null but used only for one
- FOREIGN KEY : prevent actions that would destroy links between tables
- DEFAULT : sets the default value of a column
- CHECK : it can limit the values allowed in a column
age INT CHECK (age >= 18)

# Q3. What is a Primary Key?

- A column that uniquely identifies each row in a table.
- Cannot be NULL
- Must be unique
CREATE TABLE users (
  id INT PRIMARY KEY,
  name VARCHAR(50)
);

# Q4. What is a Foreign Key?

- A foreign key is a column (or set of columns) in a table that refers to the primary key in another table and helps maintain referential integrity between the tables.
- FKs can have duplicate & null values.
- There can be multiple FKs.
CREATE TABLE orders (
  id INT PRIMARY KEY,
  user_id INT,
  FOREIGN KEY (user_id) REFERENCES users(id)
);

# Q. What is a Unique Key?

- A Unique Key is a constraint that ensures all values in a column are unique across the table.
CREATE TABLE users (
  id INT PRIMARY KEY,
  email VARCHAR(100) UNIQUE
);

# Q. What is a Candidate Key?

- A candidate key is a minimal set of attributes that can uniquely identify each row. One candidate key is selected as the primary key, while the others are alternate keys.
CREATE TABLE users (
    id INT,
    email VARCHAR(100),
    phone VARCHAR(15)
);

# Super Key?

-  super key is any set of columns that can uniquely identify a row.

# Q. What is the difference between:
Primary Key
Unique Key

- Primary key is used to uniquely identify each row in a table and it cannot contain NULL values. Also, a table can have only one primary key.

- Unique key also ensures uniqueness of values, but it can allow NULL values, and a table can have multiple unique keys.

# Q. Types of SQL Commands

- DDL (Data Definition Language) : create, alter, rename, truncate & drop
- DML (Data Manipulation Language) : select, insert, update & delete
- DCL (Data Control Language) : grant & revoke permission to users
- DQL (Data Query Language) : select
- TCL (Transaction Control Language) : start transaction, commit, rollback etc.

## 2. SELECT Queries
# Q5. How do you fetch all records from a table?

SELECT * FROM users;

# Q6. How do you fetch only specific columns?

SELECT name, email FROM users;

# Q7. What is WHERE clause?

Used to filter records.
SELECT * FROM users WHERE age > 25;

# Q8. Difference between WHERE and HAVING?

- WHERE is used to filter rows before grouping, while HAVING is used to filter data after aggregation.
- WHERE works before GROUP BY, and HAVING works after GROUP BY.

For example, if I want employees with salary > 50,000, I use WHERE.
But if I want departments where average salary > 50,000, I use HAVING.

## 3. Aggregate Functions
# Q9. What are aggregate functions?

Functions that perform calculations on multiple rows:
COUNT()
SUM()
AVG()
MIN()
MAX()
SELECT COUNT(*) FROM users;

# Q10. Find the total salary of all employees

SELECT SUM(salary) FROM employees;

## 4. GROUP BY
# Q11. Find number of employees in each department

SELECT department, COUNT(*) 
FROM employees 
GROUP BY department;

## 5. Joins
# Q12. What is JOIN?

- JOIN is used to combine rows from two or more tables based on a related column.

# Q13. Types of JOIN

INNER JOIN : Returns records that have matching values in both tables.
LEFT JOIN	: Returns all records from the left table, and the matched records from the right table. If there is no match, NULL values are returned for the right table.
RIGHT JOIN : Returns all records from the right table, and the matched records from the left table.
FULL JOIN	: Returns all records when there is a match in either left or right table.

# Q2. Difference between INNER JOIN and LEFT JOIN?

INNER JOIN	
Only matching rows	
Excludes unmatched	

LEFT JOIN
All left + matching right
Includes unmatched (NULL)

# Q3. How do you find records present in one table but not in another?

SELECT u.*
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.id IS NULL;

# Q4. Can INNER JOIN return duplicate rows?

Yes, if one record matches multiple rows in other table.

# Q5. What happens if join condition is missing?

It creates Cartesian product (very large result).

# Q6. Difference between JOIN and SUBQUERY?

JOIN	
Faster	
Better for large data	

SUBQUERY
Slower
Good for simple logic

# Q7. Can we join more than 2 tables?

Yes
SELECT u.name, o.id, p.name
FROM users u
JOIN orders o ON u.id = o.user_id
JOIN products p ON o.product_id = p.id;

# Q8. Which JOIN is most used in real projects?

INNER JOIN and LEFT JOIN

# Q9. How to find customers who placed orders?

SELECT DISTINCT u.*
FROM users u
JOIN orders o ON u.id = o.user_id;

# Q10. How to find customers who didn’t place orders?

SELECT u.*
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.id IS NULL;

# Q14. Example of INNER JOIN

SELECT users.name, orders.id
FROM users
INNER JOIN orders ON users.id = orders.user_id;

## 10. Common Practical Questions
# Q20. Find duplicate records

SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

# Q21. Find the second highest salary

SELECT MAX(salary) 
FROM employees 
WHERE salary < (SELECT MAX(salary) FROM employees);

# Q22. Difference between DELETE and TRUNCATE

DELETE	                     TRUNCATE
Removes specific rows	   Removes all rows
Can rollback	           Cannot rollback
Slower	                 Faster

# Q23. What is NULL?

Represents missing or unknown value.

# Q24. What is a View?

A virtual table based on a SELECT query.
CREATE VIEW active_users AS
SELECT * FROM users WHERE active = 1;

# Q25. Difference between UNION and UNION ALL

UNION	                  UNION ALL
Removes duplicates	  Keeps duplicates

# Q. Table related Queries
Alter (to change the schema) :

Add column
- ALTER TABLE table_name
ADD COLUMN column_name datatype constraint;

Delete column
- ALTER TABLE table_name
DROP COLUMN column_name;

Change column (rename)
- ALTER TABLE table_name
CHANGE COLUMN old_name new_name new_datatype new_constraint;

MODIFY Column (modify datatype/ constraint)
- ALTER TABLE table_name
MODIFY col_name new_datatype new_constraint;

Rename table
- ALTER TABLE table_name
RENAME TO new_table_name;

# Q. Truncate 

- to delete table's data
- TRUNCATE TABLE table_name ;

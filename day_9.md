# Question 1

Imagine you have a users table:
users
-------------------------
id
name
email
age

# What is a Primary Key? And tell me why we use it.

- The id column is the primary key because it uniquely identifies each row and cannot contain NULL values.

# Question 2 - What is the difference between: Primary Key and Unique Key

- Primary key is used to uniquely identify each row in a table and it cannot contain NULL values. Also, a table can have only one primary key.

- Unique key also ensures uniqueness of values, but it can allow NULL values, and a table can have multiple unique keys.

# Question 3 — Indexing

Suppose this table has 10 million users:
users
----------------
id
name
email
age

And we frequently run:
SELECT * FROM users
WHERE email = 'abc@gmail.com';

# Why might we create an index on email? And what is one disadvantage of having indexes?

- We create an index on email to search query fast instead of scanning the whole table. And one disadvantage of having indexes is it slow down the write, update and delete operations because the indexes may also need to be maintained.

# What is the difference between WHERE and HAVING in SQL?
# Give me a simple example if you can.

- WHERE is used to filter rows before grouping, while HAVING is used to filter data after aggregation.
- WHERE works before GROUP BY, and HAVING works after GROUP BY.

For example, if I want employees with salary > 50,000, I use WHERE.
But if I want departments where average salary > 50,000, I use HAVING.

# Question 5 — JOIN

You have two tables:
users
----------------
id
name

orders
----------------
id
user_id
amount

# What is a JOIN? And explain the difference between: INNER JOIN and LEFT JOIN

- JOIN is used to combine data from two or more related tables. 
- INNER JOIN returns only matching records from both tables, while LEFT JOIN returns all records from the left table and matching records from the right table. If there is no match, the right-side columns contain NULL.

# Question 6 — Index Selection
Suppose you have:

users
----------------
id
name
email
age

And these queries are common:
SELECT * FROM users WHERE email = 'a@gmail.com';
SELECT * FROM users WHERE age = 25;
You can create indexes on both email and age.

# Would you blindly create indexes on every column? Why or why not?

- No, I wouldn't create indexes on every column. Indexes improve search and read performance, but they also add storage overhead and can slow down INSERT, UPDATE, and DELETE operations because the indexes need to be maintained. So I would create indexes based on the queries and access patterns.

# Question 7 — Transactions

Imagine you're transferring ₹1,000 from Aalia's bank account to Rahul's account.

The operation has two steps:
1. Deduct ₹1,000 from Aalia
2. Add ₹1,000 to Rahul
Suppose step 1 succeeds, but the application crashes before step 2.

# What database concept would you use to make sure this transfer doesn't leave the database in an inconsistent state? And why?

- I would use a database transaction. The debit and credit operations should be performed within the same transaction. If both succeed, we commit; if any operation fails, we rollback. This is supported by ACID properties, especially Atomicity, which ensures that the operations are treated as one unit.

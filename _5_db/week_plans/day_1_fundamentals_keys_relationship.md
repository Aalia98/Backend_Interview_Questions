## Part 1 — Interview Questions

# 1. What is a database?

A database is an organized collection of data that allows applications to store, retrieve, update, and manage data efficiently.

# 2. What is DBMS?

DBMS stands for Database Management System. It is software that allows applications to create, store, retrieve, update, and manage data in a database.

# 3. What is RDBMS?

RDBMS stands for Relational Database Management System. It stores data in tables and allows relationships between those tables using keys and constraints.

# 4. What is a table?

A table is a structured collection of related data organized into rows and columns.

Table → Users
Row → one user
Column → attribute such as name

# 5. What is a schema?

A schema defines the structure of a database, including its tables, columns, data types, constraints, and relationships.

# 6. What are database constraints?

Constraints are rules enforced by the database to maintain data integrity and prevent invalid data.

Common constraints:

PRIMARY KEY
FOREIGN KEY
UNIQUE
NOT NULL
CHECK
DEFAULT

# 7. What is a primary key?

A primary key is a column or set of columns that uniquely identifies each row in a table. It cannot contain NULL values and a table can have only one primary key.

# 8. Why can't a primary key contain NULL?

Because the purpose of a primary key is to uniquely identify a row.
If the identifier were NULL, we couldn't reliably identify that row.
So a primary key must be: Unique + Not NULL

# 9. What is a unique key?

A unique key is a constraint that ensures that values in a column, or combination of columns, are not duplicated.

A UNIQUE constraint can allow NULL values, and its exact behavior with multiple NULLs depends on the database's rules.

# 10. Primary Key vs Unique Key?

- Primary key is used to uniquely identify each row in a table and it cannot contain NULL values. Also, a table can have only one primary key.

- Unique key also ensures uniqueness of values, but it can allow NULL values, and a table can have multiple unique keys.

# 11. What is a candidate key?

A candidate key is a minimal set of columns that can uniquely identify every row in a table.

Example:

Users

id
email
phone

If all three are unique:

id       → candidate key
email    → candidate key
phone    → candidate key

One of them can be selected as the primary key and others called as alternate key.

# 12. What is a super key?

A super key is any set of one or more columns that can uniquely identify a row.

Suppose id is unique:

{id}
{id, name}
{id, email}
{id, email, name}

All can uniquely identify a row, so they are super keys.
But some contain unnecessary columns.

# 13. Candidate key vs Super key?

A candidate key is a minimal super key.

Example:

{id}          → Super Key + Candidate Key
{id, name}    → Super Key
{id, email}   → Super Key

{id, name} isn't a candidate key because id alone is enough.

Mental model
Super Key
    │
    │ remove unnecessary attributes
    ▼
Candidate Key

# 14. Can a table have multiple candidate keys?

Yes.

Example:

Users

id
email
phone

If all three are unique:

id       → Candidate Key
email    → Candidate Key
phone    → Candidate Key

One is chosen as the primary key.
The remaining candidate keys are often called alternate keys.

# 15. Can a table have multiple unique keys?

Yes.

# 16. What is a composite key?

A composite key is a key made up of two or more columns that together uniquely identify a row.

for example:

Table Student_Course

student_id
course_id

A student can enroll in many courses, and a course can have many students.

Together:

(student_id, course_id)

can uniquely identify an enrollment.

Example:

PRIMARY KEY (student_id, course_id)

# 17. What is a foreign key?

A foreign key is a column or set of columns that references a key in another table and is used to maintain referential integrity.

# 18. Why do we need foreign keys?

Foreign keys ensure that relationships between tables remain valid and prevent invalid references.

# 19. What is referential integrity?

Referential integrity means that a foreign-key value must reference a valid key in the related table, unless NULL is allowed by the schema.

# 20. What is a 1:1 relationship?

A one-to-one relationship means one record in one table is associated with at most one record in another table, and vice versa.

Example:

User ───── Profile
 1           1

Implementation:

Profiles (
    id INT PRIMARY KEY,
    user_id INT UNIQUE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

The UNIQUE constraint prevents multiple profiles for the same user.

# 21. What is a 1:N relationship?

A one-to-many relationship means one record in one table can be associated with multiple records in another table.

Example:

User
 │
 ├── Order 1
 ├── Order 2
 └── Order 3

So:

Users 1 ─────── N Orders

Implementation:

Users
-----
id PK

Orders
------
id PK
user_id FK

# 22. What is an M:N relationship?

A many-to-many relationship means multiple records in one table can be associated with multiple records in another table.

Example:

Students ↔ Courses

One student can take multiple courses, and one course can have multiple students.

# 23. Where do you place a foreign key in 1:N?

- Let's take a example of Users table and  Orders table, I would put the foreign key in the Orders table because Orders is the many side. Multiple orders can reference the same user.

# 24. How do you implement M:N?

We implement a many-to-many relationship using a junction table containing foreign keys to both entities.

Example:

Students
   │
   │ 1:N
   ▼
Student_Courses
   ▲
   │ N:1
   │
Courses

Schema:

Students (
    id PRIMARY KEY
);

Courses (
    id PRIMARY KEY
);

Student_Courses (
    student_id,
    course_id,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Students(id),
    FOREIGN KEY (course_id) REFERENCES Courses(id)
);

# 25. What is a junction table?

A junction table, also called a bridge or associative table, is a table used to implement a many-to-many relationship between two entities.

Example:

Students
Courses
   ↓
Student_Courses

It contains the relationship:

student_id
course_id

It can also contain relationship-specific information:

student_id
course_id
enrolled_at
status
grade

# 26. How do you enforce a 1:1 relationship?

Put a foreign key in one table and add a UNIQUE constraint to that foreign key.

Example:

CREATE TABLE profiles (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT UNIQUE NOT NULL,
    bio VARCHAR(255),

    FOREIGN KEY (user_id)
        REFERENCES users(id)
);

## Part 2 — SQL / Technical Practice

# Write SQL to create users.

Requirements:

id → primary key
name → required
email → unique and required

- CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
  );

# Create orders.

Requirements:

id → primary key
user_id → foreign key referencing users
amount
status

- CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    amount INT,
    status VARCHAR(30),

    FOREIGN KEY (user_id) REFERENCES users(id)
  );

# Write a query to find all orders belonging to user 1.

- SELECT * FROM orders WHERE user_id = 1;

# Write a query to find all users who have placed an order.

- SELECT DISTINCT u.*
  FROM users u
  INNER JOIN orders o
      ON u.id = o.user_id;

# Write a query to find users who have not placed any order.

- SELECT u.*
  FROM users u
  LEFT JOIN orders o
      ON u.id = o.user_id
  WHERE o.id IS NULL;

# 6. Suppose:

users = 10 million
orders = 100 million

What column would you consider indexing for:

SELECT *
FROM orders
WHERE user_id = 123;

And why?

- I would create an index on orders.user_id because the query filters orders by user_id. The index allows MySQL to locate the relevant rows efficiently instead of scanning the entire orders table.

## Part 3 — Design Exercise

User

A user has:

    ID
    Name
    Email
    Phone
    Order

An order has:

    ID
    User
    Amount
    Status
    Created time

Requirements
Every user must have a unique ID.
Email must be unique.
A user can place many orders.
Every order belongs to one user.
We should prevent an order from referencing a non-existing user.

# Q1 What is the primary key of Users?

- id is the primary key of Users.

# Q2 What should be UNIQUE?

- email and phone should be unique.

# Q3 What is the relationship between Users and Orders?

- One-to-many.

# Q4 Which table contains the foreign key?

- Orders table contains the foreign key.

# Q5 What is the foreign key?

- A foreign key is a column or set of columns that references a key in another table and helps maintain referential integrity between the tables.

# Q6 Write the two tables.

- CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    email VARCHAR(50) UNIQUE NOT NULL,
    phone VARCHAR(20) UNIQUE
  );

- CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    status VARCHAR(20) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (user_id) REFERENCES users(id)
  );

# Q7 Why did you put the FK there?

- I would put the foreign key in the Orders table because Orders is the many side. Multiple orders can reference the same user.

# Q8. Why can't we put order_id inside the Users table?

- Because one user may have multiple orders.

# Q9. Why is user_id enough to establish the relationship?

- user_id in Orders stores the ID of the user who owns that order, and the foreign-key constraint establishes that it must reference a valid users.id.

# Q10. Can Orders.user_id contain duplicate values?

- Yes

# Q11. Can Users.email and Users.phone both be UNIQUE?

- Yes

# Q12. What happens if I try to insert:
INSERT INTO orders (user_id, amount)
VALUES (999, 5000);
when user 999 doesn't exist?

- if users.id = 999 doesn't exist, the foreign-key constraint rejects the insert.
- This is referential integrity.

# Q13. What happens if I delete a user who has orders?

- We can use ON DELETE CASCADE if deleting the parent should delete all child records, but for important historical data such as orders, I'd generally avoid physically deleting the orders and may use RESTRICT or soft deletion depending on the business requirements.

## Part 4 - Revision 

DATABASE DAY 1
────────────────────────

1. Database
→ Organized collection of data.

2. RDBMS
→ Data stored in related tables.

3. Primary Key
→ Uniquely identifies each row.
→ Unique + NOT NULL.

4. Unique Key
→ Prevents duplicate values.
→ Multiple unique constraints possible.

5. Candidate Key
→ Minimal set of columns that can uniquely identify a row.

6. Super Key
→ Any set of columns that uniquely identifies a row.
→ Candidate Key = minimal Super Key.

7. Foreign Key
→ References a key in another table.
→ Maintains referential integrity.

8. Relationships

1:1
→ One A ↔ One B

1:N
→ One A ↔ Many B
→ FK usually on N side

M:N
→ Many A ↔ Many B
→ Junction table
→ Two 1:N relationships

9. Key Mental Model

PK → Who am I?
UK → Must this value be unique?
FK → Who am I related to?
Candidate → Could this identify the row?
Super → Can this identify the row, even with extras?
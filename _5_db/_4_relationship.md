# 1. What is Relationship in DB?

- A relationship defines how tables are connected to each other using keys (usually Primary Key ↔ Foreign Key).

# 2. Types of Relationships (Most Important)
- 1. One-to-One (1:1) : One record in Table A is related to exactly one record in Table B.
Example:
User ↔ Profile
One user has one profile

📊 Table Design:
Users(user_id PK, name)
Profiles(profile_id PK, user_id FK UNIQUE)

👉 user_id in Profiles is:
Foreign Key
UNIQUE → ensures 1:1

🎯 Interview Tip : Use 1:1 when data is optional or needs separation (e.g., security info)

- 2. One-to-Many (1:N) : One record in Table A can have multiple records in Table B.
Example:
User → Orders
One user can have many orders

📊 Table Design:
Users(user_id PK)
Orders(order_id PK, user_id FK)
user_id in Orders = Foreign Key

🎯 Interview Tip: Always implement using Foreign Key in “many” side

- 3. Many-to-Many (M:N) : Multiple records in Table A relate to multiple records in Table B.
Example:
Students ↔ Courses
One student can enroll in many courses
One course has many students

📊 Table Design 
Students(student_id PK)
Courses(course_id PK)

Student_Course(
    student_id FK,
    course_id FK
)
This is called: Junction Table / Bridge Table

🎯 Interview Tip: Many-to-Many is ALWAYS broken into two 1:N relationships

# Q: What is difference between 1:1, 1:N, M:N?

1:1 → One to one mapping
1:N → One record maps to many
M:N → Many records map to many (requires junction table)

# Q: How do you implement Many-to-Many?

Using a junction table with foreign keys from both tables.

# Q: Where do you place Foreign Key?

In the “many” side of the relationship.

# Q: Can 1:1 be merged into one table?

Yes, if both entities are always used together.
Otherwise, keep separate.

# Q: How to enforce 1:1?

Add UNIQUE constraint on foreign key.

# Q: What is Junction Table?

A table used to break many-to-many relationship into two one-to-many relationships.

# Q: Design Instagram Follow System

- Relationship:
User ↔ User (Many-to-Many)
Table:
Follows(
    follower_id,
    following_id
)

# Q: E-commerce System

- Relationships:
User → Orders (1:N)
Order → Products (M:N)

# Q: Blogging Platform

- Relationships:
User → Posts (1:N)
Post → Tags (M:N)

# Q: Can Many-to-Many exist directly?

No, it must be converted into two 1:N relationships using a junction table.

# Q: What happens if no Foreign Key?

Data inconsistency may occur (no referential integrity).

# Q: Difference between logical and physical relationship?

Logical → conceptual design
Physical → implemented using keys

# Q. Practice Questions

Try these:

Design:

Library system
Online course platform
Food delivery app
Identify relationships:
Customer ↔ Address
Order ↔ Product
Teacher ↔ Student
Daily structure — ~3 hours
    30–45 min  → Theory
    45–60 min  → SQL / technical practice
    30–45 min  → Interview questions
    30–45 min  → Design exercise
    15 min     → Revision / notes

# 🟢 DAY 1 — Database Fundamentals + Keys + Relationships

Database fundamentals

    DB vs DBMS vs RDBMS
    Table, row, column
    Schema
    Constraints
    NULL
    Data types

Keys

    Primary Key
    Foreign Key
    Unique Key
    Candidate Key
    Super Key

Relationships

    1:1
    1:N
    M:N
    Junction/bridge table

Practice questions

    What is a database?
    DBMS vs RDBMS?
    What is a primary key?
    Why can't a primary key contain NULL?
    Primary key vs unique key?
    What is a foreign key?
    Why do we need foreign keys?
    What is a candidate key?
    What is a composite key?
    Explain 1:1, 1:N and M:N.
    How do you implement M:N?
    Where should the foreign key be placed in 1:N?
    How do you enforce a 1:1 relationship?

🛠️ Design Exercise #1

    Design Users + Addresses for an e-commerce system.

    Decide:

    Users
    Addresses

Questions to answer:

    Can one user have multiple addresses?
    Which relationship?
    Where does FK go?
    What should be the primary key?
    Should email be UNIQUE?
    Should address be part of Users or separate?

Expected thinking:

Users
-----
id PK
name
email UNIQUE

Addresses
---------
id PK
user_id FK
street
city
state
pincode

Likely relationship:
User 1 ───────── N Addresses

# 🟢 DAY 2 — SQL Fundamentals + JOINs

This is a very important day.

Learn
SELECT
FROM
WHERE
ORDER BY
GROUP BY
HAVING
DISTINCT
LIMIT

JOIN
 ├── INNER JOIN
 ├── LEFT JOIN
 └── SELF JOIN

INSERT
UPDATE
DELETE

Then understand:

WHERE vs HAVING
WHERE vs ON
INNER JOIN vs LEFT JOIN
Practice SQL

Use an e-commerce schema:

users
products
orders
order_items
categories

Write queries for:

Find all users.
Find users from Delhi.
Find products costing more than ₹1000.
Find the 10 most expensive products.
Find all orders of a particular user.
Find users who have placed at least one order.
Find users who have never placed an order.
Count orders per user.
Find categories having more than 10 products.
Find total amount of each order.
Find the most expensive product.
Find the number of products in each category.
Interview questions
What is JOIN?
INNER JOIN vs LEFT JOIN?
What happens when there is no matching row in LEFT JOIN?
WHERE vs HAVING?
WHERE vs ON?
What is GROUP BY?
Why do we use aggregate functions?
What is a subquery?
What is a correlated subquery?
🛠️ Design Exercise #2

Design an e-commerce Order database.

Start with:

Users
Products
Orders
Order_Items

Determine:

User → Orders
Order → Order_Items
Product → Order_Items

Then explain all relationships.

Your final relationship diagram should look roughly like:

Users
  │
  │ 1:N
  ▼
Orders
  │
  │ 1:N
  ▼
Order_Items
  ▲
  │ N:1
  │
Products

# 🟢 DAY 3 — Normalization + Database Design

You've already started this topic, so Day 3 should be practice-heavy.

Learn
Data Redundancy
      ↓
Anomalies
 ├── Insert
 ├── Update
 └── Delete

Normalization
 ├── 1NF
 ├── 2NF
 └── 3NF

Denormalization
Must understand
1NF

Atomic values.

2NF

No partial dependency on a composite key.

3NF

No transitive dependency.

Practice questions
What is normalization?
Why do we normalize?
What is data redundancy?
What is insertion anomaly?
What is update anomaly?
What is deletion anomaly?
What is 1NF?
What is 2NF?
What is 3NF?
What is partial dependency?
What is transitive dependency?
Why is 2NF mainly relevant with composite keys?
What is denormalization?
When would you denormalize?
Is normalization always good?
🛠️ Design Exercise #3

You're given:

Order(
    order_id,
    customer_name,
    customer_phone,
    product_name,
    product_price,
    quantity
)

Suppose one order can contain multiple products.

Your job:

Step 1

Identify redundancy.

Step 2

Identify possible anomalies.

Step 3

Convert it to a better design.

You should arrive at something similar to:

Customers
---------
customer_id PK
name
phone

Orders
------
order_id PK
customer_id FK

Products
--------
product_id PK
name
price

Order_Items
-----------
order_id FK
product_id FK
quantity

Then explain why each table exists.

# 🔴 DAY 4 — Indexing + Query Performance

This is one of the highest-value SD1 backend topics.

Learn
Index
 │
 ├── Why indexes exist
 ├── Full table scan
 ├── B+ Tree
 ├── Unique index
 ├── Composite index
 ├── Covering index
 └── EXPLAIN

Also understand:

Index advantages
Index disadvantages
When index isn't useful
Selectivity
Leftmost-prefix idea for composite indexes
Index and write performance
Practice questions
What is an index?
Why does an index make queries faster?
What data structure does MySQL commonly use for indexes?
What is a B+ Tree?
Why not create indexes on every column?
What is a composite index?
What is a covering index?
What happens when we insert into an indexed table?
Can an index slow down writes?
When might MySQL not use an index?
What is a full table scan?
What is EXPLAIN?
How would you investigate a slow query?
Should we index a low-cardinality column?
How would you choose the order of columns in a composite index?
🛠️ Design Exercise #4

E-commerce product search:

SELECT *
FROM products
WHERE category_id = 10
AND price BETWEEN 1000 AND 5000
ORDER BY created_at DESC;

Suppose:

products = 20 million rows

Design indexes for this query.

Then answer:

What index would you consider?
Why?
Would one index be enough?
How would you verify whether MySQL uses it?
What would you check using EXPLAIN?

# 🔴 DAY 5 — Transactions + ACID + Isolation + Locks

This is another must-master backend topic.

Learn
Transaction
    ↓
ACID
 ├── Atomicity
 ├── Consistency
 ├── Isolation
 └── Durability

Isolation Levels
 ├── Read Uncommitted
 ├── Read Committed
 ├── Repeatable Read
 └── Serializable

Locks
 ├── Shared
 └── Exclusive

Deadlocks

Also know these problems:

Dirty Read
Non-repeatable Read
Phantom Read
Practice questions
What is a transaction?
Why do we need transactions?
Explain ACID.
What is atomicity?
What is consistency?
What is isolation?
What is durability?
What is a dirty read?
What is a non-repeatable read?
What is a phantom read?
What are isolation levels?
What is a lock?
Shared vs exclusive lock?
What is a deadlock?
How can deadlocks happen?
How can you reduce deadlocks?
🛠️ Design Exercise #5

E-commerce order placement

Suppose a user buys the last item of a product.

You need to:

1. Create order
2. Create order item
3. Decrease inventory
4. Commit

Ask yourself:

What happens if inventory update succeeds but order creation fails?

Design the transaction boundary.

Example:

BEGIN TRANSACTION

Create Order
      ↓
Create Order Item
      ↓
Update Inventory
      ↓
COMMIT

If anything fails
      ↓
ROLLBACK

Then explain why ACID matters.

# 🟠 DAY 6 — Pagination + Replication + Sharding + Partitioning

This day connects Database → HLD.

Learn
Pagination
LIMIT + OFFSET
        ↓
Cursor-based pagination

Understand why:

OFFSET 1000000

can become expensive.

Replication
             Primary
             /     \
            /       \
       Replica 1   Replica 2

Understand:

Primary DB
Read replica
Read/write separation
Replication lag
Scaling
Vertical Scaling
Horizontal Scaling
Sharding
Database
   │
   ├── Shard 1 → Users 1–1M
   ├── Shard 2 → Users 1M–2M
   └── Shard 3 → Users 2M–3M
Partitioning

Understand the difference between:

Partitioning vs Sharding

This is a common interview question.

Practice questions
What is pagination?
OFFSET vs cursor pagination?
Why can large OFFSET become slow?
What is replication?
What is a read replica?
Why use read replicas?
What is replication lag?
What is vertical scaling?
What is horizontal scaling?
What is sharding?
Why do we shard?
What is a shard key?
What is partitioning?
Partitioning vs sharding?
What problems can sharding introduce?
🛠️ Design Exercise #6

Imagine:

E-commerce
100 million users
500 million orders

Design a database architecture.

Start with:

                  Backend
                     │
             ┌───────┴───────┐
             │               │
          Writes            Reads
             │               │
             ▼               ▼
          Primary       Read Replicas

Then discuss:

When would you need replicas?
Would you shard?
What could be the shard key?
Would you partition orders?
What problems could occur?

You don't need to implement this. Explain the trade-offs.

# 🔴 DAY 7 — Complete Database Design + Revision + Mock Interview

This is your integration day.

Don't learn many new concepts.

Instead, connect everything you've learned.

🛒 Design Exercise #7 — Complete E-commerce Database

Design a database for:

Amazon-like basic e-commerce system

Requirements:

Users
Register/login
Multiple addresses
Products
Product name
Price
Category
Inventory
Orders
User places an order
Order contains multiple products
Order status
Payments
One payment per order
Reviews
User can review products

Your schema might eventually become:

                         Users
                           │
                ┌──────────┼──────────┐
                │          │          │
                ▼          ▼          ▼
            Addresses    Orders     Reviews
                           │           │
                           │           │
                           ▼           ▼
                      Order_Items   Products
                           ▲           ▲
                           │           │
                           └───────────┘
                           
Orders ───────── Payments

Products ─────── Categories

Now explain:

1. Relationships
User → Address       1:N
User → Order         1:N
Order → Product      M:N
User → Review        1:N
Product → Review     1:N
Order → Payment      1:1
Product → Category   N:1
2. Keys

Identify:

Primary keys
Foreign keys
Unique keys
3. Normalization

Explain:

Why did you separate Order_Items from Orders?

4. Indexes

Choose indexes for:

users.email
orders.user_id
order_items.order_id
order_items.product_id
products.category_id
reviews.product_id

Then explain why.

5. Transaction

Explain the transaction during:

Place Order
6. Scaling

Finally explain:

Primary
   ↓
Read Replicas
   ↓
Redis
   ↓
Potential Sharding

This one exercise connects almost your entire Database syllabus.

🔁 Day 7 Revision Test

At the end of the week, you should be able to answer these without notes:

Fundamentals
What is a database?
RDBMS vs NoSQL?
What is a schema?
What is a constraint?
Keys
Primary vs Unique?
Primary vs Candidate?
Foreign key?
Composite key?
Relationships
1:1?
1:N?
M:N?
Junction table?
Normalization
1NF?
2NF?
3NF?
Partial dependency?
Transitive dependency?
Denormalization?
SQL
JOIN?
GROUP BY?
HAVING?
Subquery?
CTE?
Window function?
Indexing
Why index?
B+ Tree?
Composite index?
Covering index?
EXPLAIN?
Why indexes slow writes?
Transactions
Transaction?
ACID?
Isolation levels?
Dirty read?
Locks?
Deadlock?
Scaling
Replication?
Read replica?
Sharding?
Partitioning?
Pagination?
Cursor pagination?
📊 Your 7-Day Priority

If you get overwhelmed, don't give equal time to everything.

                    DATABASE SD1
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
      SQL           DB DESIGN        TRANSACTIONS
       🔴               🔴                🔴
        │                │                │
        ▼                ▼                ▼
    Indexing        Relationships       ACID
       🔴               🔴                🔴
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                  PERFORMANCE
                         🟠
                         │
                         ▼
                   SCALING/HLD
                         🟠
🔴 Must be strong
SQL
Keys
Relationships
Normalization
Indexing
Query optimization
Transactions
ACID
Isolation levels
Database design
🟠 Understand well
Pagination
Replication
Read replicas
Partitioning
Sharding
High availability
🟢 Don't go deep yet
4NF/5NF
Deep MySQL internals
Advanced distributed database algorithms
Complex sharding algorithms
🧠 Most Important: Your Daily Revision Method

Don't make huge notes.

For every topic, create only this:

TOPIC: Index

1. What?
   → Structure used to speed up queries.

2. Why?
   → Avoid unnecessary full table scans.

3. How?
   → MySQL can navigate an index structure.

4. Example?
   → products.category_id

5. Trade-off?
   → Faster reads, additional storage/write overhead.

6. Interview question?
   → Why not index every column?

7. Real-world use?
   → Product/category search.

Use the same 7-question template for every database concept.
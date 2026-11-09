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

# 🟢 DAY 3 — Normalization + Database Design

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
1NF
Atomic values.

2NF
No partial dependency on a composite key.

3NF
No transitive dependency.

# 🔴 DAY 4 — Indexing + Query Performance

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

# 🔴 DAY 5 — Transactions + ACID + Isolation + Locks

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

# 🟠 DAY 6 — Pagination + Replication + Sharding + Partitioning

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
Explain the transaction during: Place Order

6. Scaling
Finally explain:

Primary
   ↓
Read Replicas
   ↓
Redis
   ↓
Potential Sharding

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
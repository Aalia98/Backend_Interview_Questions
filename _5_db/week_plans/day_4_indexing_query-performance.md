## Part 1 - Interview Answers

# 1. What is an index?

An index is a data structure maintained by the database to help locate rows efficiently without scanning the entire table.

# 2. Why do we need indexes?

We use indexes to improve query performance, especially for frequently filtered, joined, sorted, or searched columns.

Example:

SELECT *
FROM orders
WHERE user_id = 100;

An index on user_id can make this lookup much faster on a large table.

# 3. How does an index improve query performance?

- Without a useful index:

   Query
   ↓
   Scan many/all rows
   ↓
   Find matching rows

- With an index:

   Query
   ↓
   Index
   ↓
   Locate matching entries
   ↓
   Fetch rows
So fewer rows may need to be examined.

# 4. Why can't we index every column?

Indexes consume storage and add overhead to INSERT, UPDATE, and DELETE operations because the indexes also need to be maintained. Therefore, I create indexes based on actual query patterns rather than indexing every column.

# 5. What is a B-tree index?

A B-tree index is a tree-based index structure that maintains values in an ordered form and supports efficient equality and range lookups, as well as ordered access.

Examples:

WHERE id = 100
WHERE price > 1000
ORDER BY created_at

# 6. What is a hash index?

A hash index uses a hash-based structure to locate values efficiently, mainly for equality lookups such as =. It is not designed for ordered or range access in the same way a B-tree is.

# 7. B-tree vs Hash index

| B-tree                              | Hash                            |
| ----------------------------------- | ------------------------------- |
| Equality                            | Equality                        |
| Range queries                       | Not suited to range queries     |
| Ordering                            | Not suited to ordered traversal |
| Common for general-purpose indexing | Specialized use cases           |

# 8. What is a composite index?

A composite index is an index created on multiple columns.

Example:

CREATE INDEX idx_orders_user_status
ON orders(user_id, status);

It can help queries that use:

WHERE user_id = ?

or:

WHERE user_id = ?
AND status = ?

depending on the query and optimizer.

# 9. Why does column order matter?

Because the database organizes the composite index according to the specified column order.

For:

INDEX(user_id, status)

user_id is the leading column.

Therefore it can efficiently support access patterns beginning with user_id.

Changing it to:

INDEX(status, user_id)

creates a different index with different useful query patterns.

# 10. What is the leftmost-prefix rule?

For:

INDEX(A, B, C)

the index can generally support prefixes such as:

A
A + B
A + B + C

but not necessarily:

B
C
B + C

as the primary index access path.

# 11. What is selectivity?

Selectivity describes how well a column distinguishes between rows.

Example:

email → usually high selectivity
gender → usually low selectivity

If almost every row has a different email, searching by email can narrow the result significantly.

# 12. What is a full table scan?

A full table scan occurs when the database examines rows throughout the table to find the rows matching the query.

Example:

10 million rows
      ↓
check rows
      ↓
find matching rows

# 13. Is a full table scan always bad?

No.
For a tiny table: 50 rows
a full scan may be cheaper than using an index.
The optimizer considers the estimated cost.

So, a full table scan is not inherently bad; it becomes concerning when a large number of unnecessary rows are being scanned for a query that could benefit from a better execution strategy.

# 14. What is a query optimizer?

The query optimizer is the database component that determines an efficient execution plan for a SQL query based on factors such as indexes, statistics, estimated row counts, join strategies, and query conditions.

# 15. What is EXPLAIN?

EXPLAIN shows the execution plan MySQL expects to use for a query. I use it to understand how tables are accessed, which indexes may be used, estimated rows, and other execution details.

Example:

EXPLAIN
SELECT *
FROM orders
WHERE user_id = 100;

# 16. What does possible_keys mean?

possible_keys shows indexes that MySQL considers potentially usable for the query.

Important:

It doesn't mean MySQL actually used them.

# 17. What does key mean?

key shows the index that MySQL actually chose to use for that table access, if any.

So:

possible_keys → candidates
key           → selected index

# 18. What does rows mean?

rows is MySQL's estimate of how many rows it expects to examine for that part of the execution plan.

Lower isn't automatically better in every situation, but a huge estimated row count can be a signal worth investigating.

# 19. Why might MySQL not use an available index?

Several reasons:

The table is small
The optimizer estimates a table scan is cheaper
The index has poor selectivity
The query doesn't match the index's leading columns
A function/transformation prevents effective use
Data distribution/statistics
A large percentage of rows matches

Interview answer:

The existence of an index doesn't guarantee that MySQL will use it. The optimizer chooses the plan it estimates to be cheapest.

# 20. How would you investigate a slow query?

First I'd reproduce the query and use EXPLAIN or EXPLAIN ANALYZE to understand the execution plan. I'd check whether appropriate indexes are being used, how many rows are being examined, whether joins or sorting are expensive, and whether the query is filtering efficiently. Then I'd optimize the query or indexes based on the evidence and measure the performance again.

# 21. What is a covering index?

A covering index is an index that contains all the columns needed to answer a query, so the database may be able to get the required information directly from the index without fetching the full table row.

Example:

CREATE INDEX idx_user_status
ON orders(user_id, status);

Query:

SELECT user_id, status
FROM orders
WHERE user_id = 100;

The index already contains the selected columns.

# 22. What is a clustered index?

For MySQL/InnoDB:

The clustered index determines how the table's rows are organized around the primary key. The primary key is normally the clustered index.

Conceptually:

Primary Key
     ↓
Clustered index
     ↓
Row data

This is more important to understand than memorize deeply for SD1.

# 23. What is a secondary index?

A secondary index is an index other than the clustered/primary index.

Example:

CREATE INDEX idx_orders_user_id
ON orders(user_id);

That's a secondary index.

In InnoDB, a secondary index also contains the primary key value so the database can locate the corresponding clustered record when needed.

# 24. Primary key index vs secondary index

| Primary/clustered index            | Secondary index                     |
| ---------------------------------- | ----------------------------------- |
| Based on primary key               | Other indexed columns               |
| In InnoDB, clustered with row data | Separate index structure            |
| One clustered primary index        | Multiple secondary indexes possible |

Example:

PRIMARY KEY(id)

and:

INDEX(user_id)
INDEX(status)

The latter two are secondary indexes.

# 25. What happens to indexes during INSERT/UPDATE/DELETE?

- INSERT

Database must:
   Insert row
   +
   Update relevant indexes

- UPDATE

If an indexed value changes:
   Update row
   +
   Update index entries

- DELETE

   Delete row
   +
   Remove corresponding index entries

Therefore indexes improve reads but add write overhead.

# 26. Should foreign-key columns be indexed?

Foreign-key columns are often good candidates for indexes because they are commonly used in joins and filtering, but I wouldn't blindly index every foreign key. I'd consider the actual query patterns and database behavior.

For example:

CREATE INDEX idx_orders_user_id
ON orders(user_id);

# 27. Composite index vs multiple single-column indexes

Suppose:

WHERE user_id = ?
AND status = ?

Option A:

INDEX(user_id)
INDEX(status)

Option B:

INDEX(user_id, status)

A composite index can be more appropriate when the application frequently queries using that specific combination.

But there's no universal winner.

Choose based on actual workload and query patterns.

# 28. How does LIKE affect index usage?

Generally:

LIKE 'aalia%'

can use a B-tree index more effectively because the pattern has a known beginning.

But:

LIKE '%gmail.com'

starts with a wildcard, making normal B-tree index usage for locating the beginning of the value ineffective.

# 29. How can functions on indexed columns affect performance?

Example:

WHERE YEAR(created_at) = 2026

Applying a function to the indexed column can prevent the database from using the index as effectively for a range lookup.

Often prefer:

WHERE created_at >= '2026-01-01'
AND created_at < '2027-01-01'

This preserves a searchable range on created_at.

# 30. How would you optimize a slow JOIN?

I'd follow a process:

Slow JOIN
   ↓
EXPLAIN
   ↓
Check JOIN conditions
   ↓
Check indexes on JOIN columns
   ↓
Check filtering
   ↓
Check number of rows examined
   ↓
Check unnecessary columns
   ↓
Check sorting/grouping
   ↓
Optimize
   ↓
Measure again

Example:

SELECT u.name, o.amount
FROM users u
JOIN orders o
    ON u.id = o.user_id;

An index on:

orders.user_id can be useful for the join.

## Part 2 - Technical Practice

Use this schema:

   users
   ----------------
   id
   name
   email
   created_at

   orders
   ----------------
   id
   user_id
   status
   amount
   created_at

Assume:

users → 10 million rows
orders → 50 million rows

# 1. Create an index to make this query faster:

SELECT *
FROM orders
WHERE user_id = 100;

- CREATE INDEX idx_orders_user_id
ON orders(user_id);

# 2. Create a composite index for:

SELECT *
FROM orders
WHERE user_id = 100
  AND status = 'PAID';

- CREATE INDEX idx_orders_user_id_status
ON orders(user_id, status);

# 3. Would this composite index be useful for:

INDEX(user_id, status)

Query:

SELECT *
FROM orders
WHERE status = 'PAID';

Answer yes/no + why.

- No, not necessarily. Because user_id is the first column in the composite index, a query filtering only on status generally cannot use this index as effectively as an index beginning with status.

# 4. Which is generally more index-friendly?

A
WHERE email LIKE '%gmail.com'
B
WHERE email LIKE 'aalia%'

Explain why.

- B is more index friendly because a leading % prevents a normal B-tree index from efficiently narrowing the search based on the beginning of the string.

# 5.
You have:

SELECT *
FROM orders
WHERE user_id = 100;

There is already:
INDEX(user_id)

but the query is still slow.
What would you investigate before blindly adding another index?

- EXPLAIN
 ↓
Check chosen index
 ↓
Check estimated rows
 ↓
Check filtering
 ↓
Check JOINs
 ↓
Check sorting/grouping
 ↓
Check table size/data distribution
 ↓
Optimize
 ↓
Measure again

# 6. Given:

SELECT *
FROM orders
WHERE user_id = 100
ORDER BY created_at DESC
LIMIT 20;

What composite index would you consider?

- CREATE INDEX idx_orders_user_created
ON orders(user_id, created_at);

Why?

user_id
   ↓
find this user's orders
   ↓
created_at
   ↓
support ordering

This is better than just:
INDEX(created_at)

because the query first filters by user_id.

# 7. Why can adding too many indexes make INSERT slower? Explain in your own words.

- Because indexes also need to be maintain, if I insert new rows and any column apply index need to maintain.

# 8. What is the difference between: Index lookup and Full table scan?

- An index lookup uses an index to locate matching rows without scanning the entire table, whereas a full table scan examines rows across the table to find matches.

## Part 3 - Design Exercise

Now imagine our e-commerce application has:
50 million orders

and this endpoint is slow:
GET /users/100/orders?status=PAID

The SQL is:

SELECT *
FROM orders
WHERE user_id = 100
  AND status = 'PAID'
ORDER BY created_at DESC
LIMIT 20;

Currently:

   orders
   -------
   id
   user_id
   status
   amount
   created_at

and only this index exists:
PRIMARY KEY(id)

Your job

Design the indexing strategy.

Think about:

# What index would you add?

- CREATE INDEX idx_orders_user_status_created
ON orders(user_id, status, created_at);

# What column should come first?

- user_id → status → created_at

# Why?

- user_id and status are used for equality filtering, and created_at is used for ordering. Putting them together can allow MySQL to efficiently locate the relevant user's paid orders and retrieve them in the required order.

# What about created_at?

- apply composite index.

# Would you create separate indexes on user_id, status, and created_at?

- No, I use composite index for all three.

# How would you verify whether your index actually improved the query?

- use EXPLAIN to know the information.
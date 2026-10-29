## Part 1 — Interview Questions

# What is an index in a database?

- Indexing is used to improve the speed of data retrieval by creating a separate structure that stores references to data.
- It is similar to an index in a book, where we can directly find the required page instead of scanning everything.
- Internally, databases use structures like B-Trees to make searching faster.
- However, indexing increases storage usage and can slow down write operations since indexes need to be updated.

# Why do we use indexes?

Faster SELECT queries
Avoid full table scan
Improve search performance

# What is a full table scan?

When database checks every row to find matching data.

# What are the types of indexes?

Primary Index
Secondary Index
Unique Index
Composite Index
Clustered vs Non-clustered

# What is a clustered index?

It defines the physical order of data in the table. Only one per table.

# What is a non-clustered index?

It stores a separate structure with pointers to actual data rows.

# On which columns should we create an index?

Columns used in WHERE
JOIN conditions
ORDER BY / GROUP BY

# When should you NOT use an index?

Small tables
Columns with low cardinality (e.g., gender)
Frequently updated columns

# What is composite index?

Index on multiple columns (e.g., (user_id, order_id))

# What is cardinality?

Number of unique values in a column
High cardinality → good for indexing

# Q:
Query is slow:
SELECT * FROM users WHERE email = 'abc@gmail.com';
What will you do?

- Add index on email

# Q:
SELECT * FROM orders WHERE user_id = 10 AND status = 'completed';
Which index?
Single?
Composite?

- Composite index on (user_id, status)

# Q:
SELECT * FROM users WHERE name LIKE '%ali%';
Will index work?

- No (because wildcard at start)

# Q:
SELECT * FROM users ORDER BY created_at;
👉 Optimization?

- Index on created_at

# Q:
Table has many INSERT/UPDATE operations
Should you add many indexes?

- No, because indexes slow down write operations

# What is B-Tree index?

- Most databases use B-Tree structure for indexing (balanced tree → fast search)

# What is covering index?

- Index that contains all required columns → no need to access table

# What is index scan vs index seek?

- Index Seek → fast (uses index directly)
- Index Scan → scans entire index

# What is query optimization?

- Process of improving query performance using indexes, better queries, etc.

# Can too many indexes hurt performance?

- YES:
Slower INSERT/UPDATE/DELETE
More storage

# Create index on email column

- CREATE INDEX idx_email ON users(email);

# Create composite index

- CREATE INDEX idx_user_status ON orders(user_id, status);

# Drop index

- DROP INDEX idx_email;

# Q. Why is a database index faster than a full table scan?

- Without an index, the database performs a full table scan and checks rows one by one until it finds the required data, which takes O(n) time.

With a B-Tree index, the database can traverse the tree structure and quickly narrow down the search space, reducing lookup time to approximately O(log n). That's why indexes significantly improve read performance.

# Q. Why do databases use B-Trees instead of Binary Search Trees for indexes?

- Databases use B-Trees instead of Binary Search Trees because BSTs can become unbalanced and degrade to O(n) performance. B-Trees remain balanced and have a very small height because each node stores multiple keys. This reduces disk I/O operations and provides efficient O(log n) lookups, insertions, and deletions.

## 
# Q. What is a JOIN in SQL? And explain types:
INNER JOIN
LEFT JOIN

JOIN is used to combine rows from two or more tables based on a related column.

INNER JOIN returns only the records that have matching values in both tables.

LEFT JOIN returns all records from the left table and the matching records from the right table. If there is no match, NULL values are returned for the right table.

For example, if we have a users table and an orders table:

INNER JOIN → only users who placed orders
LEFT JOIN → all users, even those who didn’t place any orders

# Q Suppose: Your query is very slow in production, What steps will you take to debug and optimize it?

If a query is slow in production, I follow a step-by-step approach:

- First, I check the query execution plan using EXPLAIN to understand how MySQL is executing the query.
- Then I look for full table scans or missing indexes.
- If needed, I add appropriate indexes on frequently filtered or joined columns.
- I also check if the query can be optimized by reducing unnecessary joins, selecting only required columns, or simplifying conditions.
- Additionally, I verify if there is large data volume causing the issue, and consider pagination or batching.
- Finally, I monitor performance after changes to ensure improvement.
                                       OR
First I check query performance using EXPLAIN, then identify if indexing is missing, then add index or optimize query structure.

# What is the difference between: DELETE & TRUNCATE

- DELETE is used to remove specific rows using a WHERE condition, and it deletes data row by row, so it is slower and can be rolled back.

- TRUNCATE is used to remove all rows from a table quickly without using a WHERE clause. It is faster because it resets the table internally, and it usually cannot be rolled back.

# Primary Key vs Unique Key?

- Primary Key uniquely identifies every row in a table. It cannot be NULL, and there can be only one Primary Key (though it can be composite).

- Unique Key also prevents duplicate values, but it allows NULL values (database-dependent) and a table can have multiple Unique Keys.

# Why do we use Middleware in backend applications?

- Middleware executes before the request reaches the handler. It is used for common logic such as authentication, logging, CORS, rate limiting, request validation, and panic recovery, so we don't duplicate this code in every handler.

# What is Redis? What happens if Redis is removed?

- Redis stand for Remote Dictionary Server. It is an in-memory key-value database.It is mainly used for caching, sessions, rate limiting, and temporary data.

- Response become shower because to fetch data in redis time taken approx 1ms but to fetch data into database time taken approx 50-100ms.

- Redis reduces database load. Without Redis, repeated reads hit MySQL directly, increasing database CPU usage, latency, and potentially reducing throughput under high traffic.
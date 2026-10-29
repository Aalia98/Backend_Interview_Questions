## Part 1 — Normalization Interview Answers

# 1. What is normalization?

Normalization is the process of organizing data into well-structured tables to reduce unnecessary redundancy and prevent data anomalies while maintaining data integrity.

# 2. Why do we need normalization?

- Reduce duplicate data
- Avoid inconsistent data
- Prevent update anomalies
- Prevent insert anomalies
- Prevent delete anomalies
- Improve data integrity
- Represent relationships properly

Example:

Instead of storing customer information repeatedly in every order, keep it in Users and reference it using user_id.

# What are Anomalies?

Anomalies are problems caused by poor database design.

# 3. What problems does normalization solve?

The three major anomalies:

Update anomaly
Insert anomaly
Delete anomaly

It also reduces unnecessary duplication.

# 4. What is 1NF?

A table is in 1NF when each cell contains a single atomic value and there are no repeating groups.

# 5. What is 2NF?

A table is in 2NF when:
- It is already in 1NF
- It has no partial dependency

Example:
PRIMARY KEY (order_id, product_id)

If:
product_id → product_name

then product_name depends on only part of the key. That's a partial dependency → 2NF violation.

# 6. What is 3NF?

A table is in 3NF when:
- It is already in 2NF
- It has no transitive dependency

Example:

employee_id
department_id
department_name

If:

employee_id → department_id
department_id → department_name

then:

employee_id → department_name

indirectly.

That's a transitive dependency.

# 7. What is partial dependency?

A partial dependency occurs when a non-key attribute depends only on part of a composite primary key instead of the entire composite key.

Example:
PRIMARY KEY (order_id, product_id)

If:
product_id → product_name
then product_name depends on only part of the key. That's a partial dependency → 2NF violation.

# 8. What is transitive dependency?

A transitive dependency occurs when a non-key attribute depends on another non-key attribute instead of directly depending on the primary key.

Example:

employee_id
department_id
department_name

If:

department_id depends on employee_id
department_name depends on department_id

then:
department_name indirectly depends on employee_id. 

# 9. What is an update anomaly?

An update anomaly occurs when the same piece of information is stored in multiple rows and updating only some of those rows creates inconsistent data.

Example:

order 101 → Aalia → old_email
order 102 → Aalia → old_email
order 103 → Aalia → old_email

If only order 101 is updated, the data becomes inconsistent.

# 10. What is an insert anomaly?

An insert anomaly occurs when we cannot insert a piece of information without unnecessarily requiring unrelated information.

Example:

If customer information exists only inside Orders, a new customer who hasn't placed an order cannot be stored properly.

Normalization separates:
Users
Orders

so the customer can exist independently.

# 11. What is a delete anomaly?

A delete anomaly occurs when deleting one piece of information unintentionally removes other useful information.

Example:

If the only order of a customer contains the customer's information, deleting that order may also remove the only stored information about the customer.

# 12. What is denormalization?

Denormalization is the process of combining tables to improve read performance by reducing joins.

# 13. Why would you denormalize a database?

- Read-heavy workload
- Too many expensive joins
- Frequently accessed aggregated data
- Performance requirements/optimizations
- Reporting/analytics
- Reducing query complexity

Example:

Instead of calculating total order amount every time:

Orders
+
Order_Items

we might maintain:

Users
--------
id
name
total_spent

But now we have to keep total_spent synchronized.

# 14. Normalization vs Denormalization

| Normalization                  | Denormalization                 |
| ------------------------------ | ------------------------------- |
| Reduces redundancy             | Intentionally adds redundancy   |
| Improves data integrity        | Can complicate consistency      |
| More tables                    | Fewer tables / duplicated data  |
| More joins may be needed       | Fewer joins                     |
| Good for transactional systems | Useful for read-heavy workloads |

# 15. Is normalization always better?

No. Normalization is generally good for transactional data, but extreme normalization can make frequently executed read queries more complex because they require more joins.

A real system chooses based on:

Correctness
+
Consistency
+
Read/write workload
+
Performance
+
Query patterns

# 16. How would you normalize an e-commerce database?

I'd separate different entities:

Users
Orders
Products
Categories
Order_Items

Relationships:

User      1:N Orders
Order     1:N Order_Items
Product   1:N Order_Items
Category  1:N Products

Order_Items acts as the junction table between Orders and Products.

# 17. When would you choose denormalization?

I'd consider it when:

- The system is read-heavy
- A query is executed extremely frequently
- Joins are becoming a measurable performance bottleneck
- Some derived data is expensive to calculate repeatedly
- We have a clear consistency strategy

I would first measure the workload and query performance, then selectively denormalize if the joins or repeated computation are actually a bottleneck.

# 18. Can normalization hurt performance?

Yes, potentially.

Normalization can lead to:

More tables
     ↓
More JOINs
     ↓
More complex queries
     ↓
Potentially higher query cost

But don't assume normalization automatically makes a system slow.
Proper indexes, query design, schema design, caching, query optimization can make normalized databases perform very well.

# 19. How do indexes interact with normalized tables?

Normalization often creates more tables and foreign-key relationships.
Indexes on commonly joined/filtering columns can make those operations much faster.

For example:

Orders
------
id PK
user_id FK

An index on:

CREATE INDEX idx_orders_user_id
ON orders(user_id);

can help queries such as:

SELECT *
FROM orders
WHERE user_id = 10;

and joins between Users and Orders.
Don't blindly index every foreign key or every column.

Index based on:

query patterns
filtering
joins
sorting
cardinality
write overhead

# 20. Give an example where you would intentionally denormalize.

A good e-commerce example:

Suppose the admin dashboard constantly asks:

"What is the total amount spent by every customer?"

Calculating:

Users
 ↓
Orders
 ↓
Order_Items

and aggregating every request could become expensive at scale.

We could maintain a precomputed value such as:

customer_summary
----------------
user_id
total_orders
total_spent

This duplicates derived information, so it's denormalization.

But we'd need a reliable mechanism to keep it synchronized when orders are created, cancelled, refunded, etc.

## Part 1 — Technical Practice

# Practice 1
Is this table normalized?
Users

id | name  | phones
---|-------|------------------
1  | Aalia | 9876, 8765
2  | Rahul | 9999

If not, why?

- No, phone numbers of Aalia contain 2 numbers in one cell. 1NF requires each cell to contain a single atomic value and avoids repeating groups.

# Practice 2
Identify the problem:
Orders

order_id | customer_name | customer_email
---------|---------------|----------------
101      | Aalia         | a@gmail.com
102      | Aalia         | a@gmail.com
103      | Aalia         | a@gmail.com

What happens if Aalia changes her email?

- If Aalia's email is repeated in 10 rows, we have to update all 10 rows.
- If we miss one, the database becomes inconsistent.

# Practice 3
Identify the dependency:

Order_Items

order_id
product_id
product_name
quantity

with:
PRIMARY KEY (order_id, product_id)
Does product_name depend on:
order_id
product_id

or only:
product_id

- product_name only depend on product_id, it is partial dependency (2NF).

# Practice 4
Normalize this:

Employees
employee_id
employee_name
department_id
department_name

Create the appropriate tables.

- Employees
    employee_id
    employee_name
    department_id

- Departments
    department_id
    department_name

# Why don't we normalize everything as much as possible? Give me your answer.

- Over-normalization can increase the number of joins required for common read queries, which can add complexity and potentially affect performance. Therefore, we may selectively denormalize based on the workload and access patterns.

## Part 3 - Design Exercise

Order_Data

order_id
customer_name
customer_email
customer_phone
product_id
product_name
product_price
quantity
category_name

Your job will be to decide whether we need:

Users
Orders
Products
Categories
Order_Items

and determine the relationships:

User ──── Orders
Order ─── Order_Items
Product ── Order_Items
Category ─ Products

- USERS
----------------
id PK
name
email UNIQUE
phone UNIQUE

- ORDERS
----------------
id PK
user_id FK
created_at
status

- CATEGORIES
----------------
id PK
name

- PRODUCTS
----------------
id PK
category_id FK
name
price

- ORDER_ITEMS
----------------
id PK
order_id FK
product_id FK
quantity

- Relationships are :

User      1 ─── N Orders
Order     1 ─── N Order_Items
Product   1 ─── N Order_Items
Category  1 ─── N Products
# 1. What is Normalization?

- Normalization is the process of organizing data in a database to reduce redundancy and improve data integrity.
- It involves breaking down large tables into smaller tables and defining relationships between them.
- It helps to eliminate duplicate data and prevents anomalies during insert, update, or delete operations.
- This is achieved using normal forms like 1NF, 2NF, and 3NF.
- For example, we separate user and order data into different tables instead of storing repeated information.

# 2. Why do we need Normalization?

Reduces data redundancy
Prevents anomalies (insert, update, delete)
Improves data consistency
Makes database easier to maintain

# 3. What are Anomalies?

Anomalies are problems caused by poor database design:

Types:
Insertion Anomaly → Cannot insert data without other data
Update Anomaly → Same data repeated, needs multiple updates
Deletion Anomaly → Deleting one row removes important data

# 4. What is 1NF (First Normal Form)?

All columns contain atomic (single) values
No repeating groups or arrays
Example:
id | subjects
1  | Math, Science
Fix:
id | subject
1  | Math
1  | Science

# 5. What is 2NF (Second Normal Form)?

It is already in 1NF
No partial dependency exists
Partial Dependency : When a column depends on only part of a composite key.

Example:
(order_id, product_id) → product_name
product_name depends only on product_id

Fix:
Split into:

Orders table
Products table

# 6. What is 3NF (Third Normal Form)?

It is in 2NF
No transitive dependency
Transitive Dependency: When a non-key column depends on another non-key column.
Example:
emp_id → dept_id → dept_name
Fix:
Split into:

Employee table
Department table

# 7. What is BCNF?

BCNF is a stricter version of 3NF where: Every determinant must be a candidate key

# 8. What is Denormalization?

Denormalization is the process of combining tables to improve read performance by reducing joins.
When used:
Read-heavy systems
Performance optimization

# 9. Normalize This Table (IMPORTANT 🔥)
Given:
Order(order_id, customer_name, customer_address, product_name)
Step 1: Identify Problems
Customer data repeated
Product data repeated
Step 2: Convert to 3NF
Tables:

Customers
customer_id, name, address

Products
product_id, product_name

Orders
order_id, customer_id

Order_Items
order_id, product_id

# 10. What is Partial Dependency?

When a column depends only on part of a composite primary key.

# 11. What is Transitive Dependency?

When a non-key attribute depends on another non-key attribute.

# 12. Is Normalization Always Good?

No. Over-normalization can reduce performance because it increases joins.

# 13. When do you Denormalize?

For faster reads
In high-traffic systems
When joins become expensive

# 14. Does Normalization Improve Performance?

Not always.
Improves data integrity
May reduce read performance due to joins

# 15. Which Normal Form is Used in Industry?

Mostly 3NF, with some denormalization for performance.

# 16. Difference: Normalization vs Denormalization

Feature	          Normalization	      Denormalization
Goal	        Reduce redundancy	 Improve performance
Data integrity	      High	             Medium
Joins	              More	              Less

## 17. Real Interview Question
# Q: Design DB for e-commerce

Users table
Products table
Orders table
Order_items table

👉 Then explain:
Relationships
Keys
Normalization level
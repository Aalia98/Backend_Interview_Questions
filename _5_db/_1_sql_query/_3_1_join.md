📊 Customers
| id | name   |
| -- | ------ |
| 1  | Aalia  |
| 2  | Rahul  |
| 3  | Simran |
| 4  | Aman   |

📊 Orders
| id  | customer_id | amount |
| --- | ----------- | ------ |
| 101 | 1           | 500    |
| 102 | 1           | 700    |
| 103 | 2           | 300    |
| 104 | 5           | 900    |

📊 Payments
| id | order_id | status  |
| -- | -------- | ------- |
| 1  | 101      | Success |
| 2  | 102      | Failed  |
| 3  | 103      | Success |

# Q1 Get customer name and their order amount
- SELECT c.name, o.amount
FROM Customers c
JOIN Orders o
ON c.id = o.customer_id;

# Q2 Get all customers and their orders (include customers with no orders)
- SELECT c.name, o.amount
FROM Customers c
LEFT JOIN Orders o
ON c.id = o.customer_id;

# Q3 Get all orders with customer names (include orders with no valid customer)
- SELECT o.amount, c.name
FROM Orders o
LEFT JOIN Customers c
ON c.id = o.customer_id;

# Q4 Get customers who have NOT placed any orders
- SELECT c.name
FROM Customers c
LEFT JOIN Orders o
ON c.id = o.customer_id
WHERE o.customer_id IS NULL;

# Q5 Get total order amount per customer
- SELECT c.name, SUM(o.amount) AS total
FROM Customers c
LEFT JOIN Orders o
ON c.id = o.customer_id
GROUP BY c.id, c.name;

# Q6 Get customers who have placed more than 1 order
- SELECT c.name
FROM Customers c
JOIN Orders o
ON c.id = o.customer_id
GROUP BY c.id, c.name
HAVING COUNT(o.id) > 1;

# Q7 Get customer name and payment status
- SELECT c.name, p.status
FROM Customers c
JOIN Orders o
ON c.id = o.customer_id
JOIN Payments p
ON o.id = p.order_id;

# Q8 Get orders that do NOT have any payment
- SELECT o.id
FROM Orders o
LEFT JOIN Payments p
ON o.id = p.order_id
WHERE p.order_id IS NULL;

# Q9 Get customers whose ALL payments are successful
- SELECT c.name
FROM Customers c
WHERE c.id NOT IN (
    SELECT o.customer_id
    FROM Orders o
    JOIN Payments p
    ON o.id = p.order_id
    WHERE p.status != 'Success'
);

# Q10 Get customer with highest total order amount
- SELECT c.name, SUM(o.amount) AS total
FROM Customers c
JOIN Orders o
ON c.id = o.customer_id
GROUP BY c.id, c.name
ORDER BY total DESC
LIMIT 1;
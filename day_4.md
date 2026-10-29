# Q-1 What is the difference between monolith and microservices architecture?

- Monolithic Architecture
  - Entire application is built as one single codebase/project
  - All modules (UI, business logic, database access) are tightly coupled
  - Deployed as one unit

- Microservices Architecture
  - Application is split into multiple independent services
  - Each service is: Independently developed, Independently deployed, Focused on a single responsibility  

# Q-2 What is a database transaction? What are ACID properties?

- A database transaction is a set of operations executed as a single unit where either all operations succeed or none are applied. 
- ACID properties are a set of four rules—Atomicity, Consistency, Isolation, and Durability: Atomicity guarantees all-or-nothing execution, Consistency ensures valid state, Isolation prevents interference between transactions, and Durability guarantees data persistence after commit.

# Q-3 Your API is getting slow. How would you debug and improve performance?

- If my API is slow, I would first measure and identify the bottleneck using logs and monitoring tools like Prometheus or New Relic. Then I would check different layers: optimize backend code, analyze database queries and add indexing, and introduce caching using Redis for frequently accessed data. If the issue is due to high traffic, I would apply rate limiting and load balancing. Finally, I would improve response size and ensure efficient async processing.

# Q-4 Find the first non-repeating character in a string
Example:
"aabccbd" → output: d

- I would use a frequency map to count occurrences of each character in the string.
- Traverse the string and store frequency in a hash map
- Traverse the string again
- Return the first character with frequency = 1

# Q-5 What is horizontal vs vertical scaling?

- Vertical scaling means increasing the resources (CPU, RAM) of a single server, while horizontal scaling means adding multiple servers and distributing traffic using a load balancer. 
- Vertical scaling is simple but limited, whereas horizontal scaling is more scalable and fault-tolerant but complex to manage.
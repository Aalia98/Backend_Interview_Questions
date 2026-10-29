# Q-1 What is the difference between synchronous and asynchronous programming?

- Synchronous programming executes tasks one by one, where each task blocks the next until it completes.
- Asynchronous programming is non-blocking, meaning the program can continue executing other tasks while waiting for a response.
- For example, during an API call, synchronous code waits for the response, while asynchronous code allows other operations using promises or async/await.

# Q-2 What is normalization? Why do we use it?

- Normalization is the process of organizing data in a database to reduce redundancy and improve data integrity.
- It involves breaking down large tables into smaller tables and defining relationships between them.
- It helps to eliminate duplicate data and prevents anomalies during insert, update, or delete operations.
- This is achieved using normal forms like 1NF, 2NF, and 3NF.
- For example, we separate user and order data into different tables instead of storing repeated information.

# Q-3 What is rate limiting? How would you implement it?

- Rate limiting is used to control how many requests a client can make within a certain time period.
- It helps prevent server overload, abuse, and ensures fair usage.
- It can be implemented using algorithms like fixed window, sliding window, or token bucket.
- In practice, we can use Redis to track request counts per user or IP and return a 429 error if the limit is exceeded.

# Q-4 Find the duplicate number in an array of size n where numbers are from 1 to n-1?

- We can solve this using multiple approaches.
- First, by sorting the array and checking adjacent elements for duplicates.
- Second, by using a hash map to count frequencies and returning the element with count greater than one.
- The optimal approach is using Floyd’s Cycle Detection algorithm, which works in O(n) time and O(1) space by treating the array as a linked list.

# Q-5 What is load balancing? Why is it needed?

- Load balancing is used to distribute incoming requests across multiple servers so that no single server gets overloaded.
- It improves performance, ensures high availability, and helps in scaling applications.
- Common strategies include round robin and least connections.
- For example, in a high-traffic application, a load balancer distributes requests across multiple backend servers.


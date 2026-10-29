# Q-1 What is the difference between authentication and authorization?

- Authentication is the process of verifying a user’s identity, such as through login credentials or tokens while Authorization determines what actions or resources the authenticated user is allowed to access.
- Authentication always happens first, followed by authorization.
- For example, logging in is authentication, and accessing admin-only routes is authorization.

# Q-2 What is indexing in a database? Why does it improve performance?

- Indexing is used to improve the speed of data retrieval by creating a separate structure that stores references to data.
- It is similar to an index in a book, where we can directly find the required page instead of scanning everything.
- Internally, databases use structures like B-Trees to make searching faster.
- However, indexing increases storage usage and can slow down write operations since indexes need to be updated.

# Q-3 What is middleware in backend frameworks? Can you give an example?

- Middleware is a function that runs between receiving a request and sending a response in a backend application. It is used to process requests before they reach the main business logic.
- Common use cases include authentication, logging, validation, etc.
- Uses Middleware because many APIs require common logic. Middleware allows us to write that logic once and reuse it.
- For example, an authentication middleware verifies a JWT token before allowing access to a protected route.

# Q-4 How would you check if two strings are anagrams of each other?

- To check if two strings are anagrams, I first compare their lengths. If they are not equal, I return false.
Then I use a hash map to count character frequencies. I increment counts for the first string and decrement for the second string.
Finally, I check if all values in the map are zero. If yes, the strings are anagrams; otherwise, they are not.

# Q-5 What is caching? Where have you used Redis?

- Caching is used to store frequently accessed data in a faster layer to reduce database load and improve performance.
- Redis is a popular in-memory key-value store used for caching because it provides very fast access.
- I have used Redis to cache frequently accessed data like API responses, which helps reduce database queries and improves response time.
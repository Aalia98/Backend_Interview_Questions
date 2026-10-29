# Q. Difference between Array and Linked List?

| Array                                | Linked List                                    |
| ------------------------------------ | ---------------------------------------------- |
| Contiguous memory                    | Non-contiguous memory                          |
| O(1) random access                   | O(n) random access                             |
| Insert/Delete in middle is expensive | Insert/Delete is easier once the node is known |
| Better cache locality                | More memory overhead because of pointers       |

- If I need frequent random access, I'd choose an Array. If I need frequent insertions and deletions in the middle and don't require random access, I'd consider a Linked List.

# Q. Authentication vs Authorization?

- Authentication verify identities of an user while authorization determined what are allowed for authenticated user. 
- Authorization happens after successful authentication, because the system first needs to know who you are before deciding what you're allowed to do.
- for ex, user login is authentication but user is admin or normal user is authorization.

# Q. Suppose your users table has 10 million rows.
# This query is slow:
# SELECT * FROM users WHERE email = 'abc@gmail.com';
# How would you improve its performance?

- Since email is frequently used in search conditions, creating an index on the email column allows the database to locate matching rows using a B-Tree instead of scanning the entire table. This changes the lookup from a full table scan (O(n)) to approximately O(log n), significantly improving performance.

# Q. Hashing vs Encryption?

- Hashing is a one-way process, meaning the original data cannot be recovered from the hash. It is mainly used for password storage because we only need to verify the password, not retrieve it.

- Encryption is a two-way process. Data is encrypted using a key and can later be decrypted using the appropriate key. It is used where the original data must be recovered, such as HTTPS communication or end-to-end encrypted messaging like WhatsApp.

# Q. What happens after clicking Login?

User clicks Login
↓
Browser sends HTTP POST /login
↓
Backend receives request
↓
Validate request body
↓
Query database
↓
Find user
↓
bcrypt.CompareHashAndPassword()
↓
Generate JWT
↓
Return token
↓
Browser stores token (cookie or other appropriate storage depending on the application)
↓
Browser requests dashboard
↓
Dashboard loads

# Q. Should we index product_name?

- If searching by product_name is a frequent operation, I would create an index because it improves read performance significantly. However, indexes also increase storage usage and slow down insert, update, and delete operations. Therefore, I would only create the index if search queries are common enough to justify the trade-off.

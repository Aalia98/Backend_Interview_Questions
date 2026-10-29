# Q. What is validation?

- Validation is the process of checking whether incoming data satisfies the expected format, type, constraints, and business rules before processing it.
- Validation means checking whether the data received from a client is valid according to the application's rules before processing it.

# Q. Where should validation happen?

- Basic request validation should happen near the API boundary, usually in the handler or DTO validation layer. Business validation belongs in the service layer, while important data integrity rules should also be enforced at the database level.

# Q. Why validate at multiple layers?

- Because each layer protects against different problems.

Handler
→ Invalid request format

Service
→ Invalid business operation

Database
→ Data integrity

# Q. Should frontend validation be enough?

- No.
- Frontend validation improves user experience, but it cannot be trusted for security because clients can bypass it.
- Backend must always validate.

# Q. What is the difference between validation and authorization?

Validation:
Is the data valid?

Authorization:
Is this user allowed to perform this operation?

Example:

price = -100
Validation fails.
Customer tries to delete product
Authorization fails.

# Q. What is business validation?

Business validation checks whether an operation is allowed according to application rules.

Example:

Product stock = 0
User places order

→ Request format is valid
→ But business rule doesn't allow order

# Q. Why should database constraints still exist if we validate in backend?

- Because application validation is not enough.
- Multiple requests can arrive concurrently.

Example:

Request A → Check email doesn't exist
Request B → Check email doesn't exist

Both see "available"

A → INSERT
B → INSERT

Without a database UNIQUE constraint, duplicates may be created.

With:

UNIQUE(email)

the database guarantees integrity.
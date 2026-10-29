# Q1: What is a transaction?

- A database transaction is a set of operations executed as a single unit where either all operations succeed or none are applied. 

# Why Do We Need Transactions?

- To maintain correctness and prevent partial updates when multiple related operations are involved.

# Q2: What are ACID properties?

- ACID properties are a set of four rules—Atomicity, Consistency, Isolation, and Durability: Atomicity guarantees all-or-nothing execution, Consistency ensures valid state, Isolation prevents interference between transactions, and Durability guarantees data persistence after commit.

# A = Atomicity

- Atomicity ensures that a transaction's operations are treated as a single unit. Either all required changes are committed or the transaction is rolled back.

# C = Consistency

- A successful transaction should take the database from one valid state to another valid state while respecting defined constraints and rules.

# I = Isolation

- Isolation determines how one transaction's intermediate changes are visible to other transactions.

# D = Durability

- Changes successfully save ho gaye hain; crash ke baad bhi database ko unhe preserve karna chahiye.

# Real-world example of transaction ?

- Bank transfer, order creation, stock update, payment processing.

# Q3: What is COMMIT and ROLLBACK?

- COMMIT → Makes the transaction's changes permanent.
- ROLLBACK → Undoes the transaction's changes.

# Q4: What happens if transaction fails in between?

- Entire transaction is rolled back (Atomicity)

# Q5: What is isolation level?

- Defines how visible one transaction’s changes are to others

# Q6: Types of isolation levels?

- Read Uncommitted
- Read Committed
- Repeatable Read
- Serializable

# Q7: What is dirty read?

- T1 updates value but not committed
- T2 reads it
Wrong data → Dirty read

# Q8: What is non-repeatable read?

- Same row read twice → different values

# Q9: What is phantom read?

- New rows appear between queries

# Q10: Difference between Read Committed & Repeatable Read?

- Read Committed → prevents dirty reads
- Repeatable Read → prevents non-repeatable reads

## Q11: Bank transfer example

- UPDATE accounts SET balance = balance - 100 WHERE id = 1;
- UPDATE accounts SET balance = balance + 100 WHERE id = 2;

# What if system crashes after first query?
Transaction ensures rollback → no money lost

# Q12: Two users updating same row
Problem?
Lost update

- Isolation / locking

# Q13: Why do we need isolation?

- To avoid concurrency issues like dirty reads, lost updates

# Q14: What is deadlock?

- Two transactions waiting for each other’s lock

# Q15: How to prevent deadlock?

- Access resources in same order
- Keep transactions short
- Use proper indexing

## Practical SQL Questions
# Q16: Start transaction
- BEGIN;

# Q17: Commit
- COMMIT;

# Q18: Rollback
- ROLLBACK;

# Q19: Can a transaction be partially committed?
- No (Atomicity)

# Q20: If DB crashes after COMMIT, is data safe?
- Yes (Durability)

# Q21: Which isolation level is safest?
- Serializable but slowest

# Q22: Which isolation level is most commonly used?
- Read Committed
# ACID Properties in Database Management Systems

ACID is an acronym that represents four key properties ensuring reliable database transactions:

## 1. Atomicity

### Definition
A transaction must be treated as a single, indivisible unit that either completes entirely or fails entirely.

### Explanation
Atomicity guarantees that database operations are performed as if they are a single action, despite being composed of multiple individual operations. Either all operations in a transaction are successfully completed and committed to the database, or none of them are.

### Example
Consider a bank transfer where $100 moves from Account A to Account B:

- **Step 1**: Deduct $100 from Account A
- **Step 2**: Add $100 to Account B

With atomicity, if any step fails (e.g., the system crashes after Step 1), the entire transaction is rolled back. Account A would still have its original balance - it's as if the transaction never happened.

## 2. Consistency

### Definition
A transaction must transform the database from one valid state to another valid state, maintaining all predefined rules and constraints.

### Explanation
Consistency ensures that any transaction will bring the database from one valid state to another. A valid state means that all defined rules, constraints, cascades, and triggers are enforced. This includes entity integrity, referential integrity, and user-defined constraints.

### Example
If a database rule states that all bank accounts must maintain a minimum balance of $50:

- If a withdrawal would bring Account A below $50, the entire transaction is rejected
- The database remains in a consistent state where all accounts have at least $50
- Foreign key constraints, check constraints, and triggers are all satisfied

## 3. Isolation

### Definition
Concurrent transactions must not interfere with each other, making it appear as if transactions are executed sequentially.

### Explanation
Isolation ensures that concurrent execution of transactions leaves the database in the same state as if the transactions were executed sequentially. This prevents dirty reads, non-repeatable reads, and phantom reads depending on the isolation level implemented.

### Example
Two people simultaneously access an inventory system:

- **User 1**: Checks if Product X is in stock (quantity = 5)
- **User 2**: Checks if Product X is in stock (quantity = 5)
- **User 1**: Orders 3 units of Product X
- **User 2**: Orders 4 units of Product X

Without isolation, both orders would proceed despite not having enough inventory. With proper isolation, User 2's transaction would wait for User 1's transaction to complete, then see the updated inventory (quantity = 2) and fail appropriately.

## 4. Durability

### Definition
Once a transaction is committed, its effects persist even in the event of system failures.

### Explanation
Durability guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure. This is typically achieved through persistent storage mechanisms like transaction logs that allow the database to recover to the most recent consistent state.

### Example
After a bank transfer completes:

- The system confirms "Transfer successful" to the user
- If the system crashes immediately after, the transfer is still recorded
- When the system recovers, Account A still shows the deduction and Account B shows the addition

### Implementation Techniques
Durability is typically achieved through:
- Write-ahead logging (WAL)
- Database backups
- Redundant storage systems
- Journaling file systems
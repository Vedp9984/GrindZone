# Transactions and Concurrency Control in DBMS

## Introduction to Transactions

A transaction is a logical unit of work that must be either completely executed or not executed at all. Transactions are used to maintain database consistency in multi-user environments and in the presence of failures.

## ACID Properties of Transactions

Transactions must exhibit the following properties, known as ACID properties:

### 1. Atomicity
- **Definition**: A transaction is treated as a single, indivisible unit of work.
- **Example**: If a bank transfer involves debiting one account and crediting another, either both operations succeed or neither does.
- **Implementation**: Maintained through the use of logs and recovery mechanisms that can undo (rollback) partial transactions.

```sql
BEGIN TRANSACTION;
  UPDATE accounts SET balance = balance - 100 WHERE account_id = 'A123';
  UPDATE accounts SET balance = balance + 100 WHERE account_id = 'B456';
COMMIT; -- If any statement fails, the entire transaction is rolled back
```

### 2. Consistency
- **Definition**: A transaction must transform the database from one consistent state to another.
- **Example**: If a rule states that all accounts must have a non-negative balance, no transaction should violate this rule.
- **Implementation**: Ensured through constraints, triggers, and transaction logic.

```sql
BEGIN TRANSACTION;
  UPDATE accounts SET balance = balance - 100 WHERE account_id = 'A123';
  -- Check if balance went negative
  IF (SELECT balance FROM accounts WHERE account_id = 'A123') < 0 THEN
    ROLLBACK; -- Cancel the transaction if constraint is violated
  ELSE
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 'B456';
    COMMIT;
  END IF;
```

### 3. Isolation
- **Definition**: Concurrent transactions should not interfere with each other.
- **Example**: If two users are transferring money from the same account simultaneously, the final balance should be as if the transactions occurred one after another.
- **Implementation**: Achieved through concurrency control mechanisms like locks or multiversion concurrency control.

### 4. Durability
- **Definition**: Once a transaction is committed, its changes persist even in the event of system failure.
- **Example**: If a power outage occurs immediately after a transaction is committed, the changes are still preserved.
- **Implementation**: Maintained through write-ahead logging (WAL) and database backups.

## Transaction States

A transaction can exist in one of the following states:

1. **Active**: The transaction is being executed.
2. **Partially Committed**: The transaction has executed its final operation but not yet been committed.
3. **Committed**: The transaction has completed successfully and its changes are permanent.
4. **Failed**: The transaction cannot be completed normally.
5. **Aborted**: The transaction has been rolled back and the database is restored to its state prior to the transaction.
6. **Terminated**: The transaction has either committed or aborted.

![Transaction State Diagram](https://example.com/transaction-states.png)

## Concurrency Control

Concurrency control ensures multiple transactions can operate simultaneously without interfering with each other. This is crucial for multi-user database systems.

### Concurrency Problems

When multiple transactions execute concurrently, several problems can arise:

#### 1. Lost Update Problem
- **Definition**: Occurs when two transactions read and update the same data, and one transaction's changes overwrite the other's without taking it into account.
- **Example**:
  - Transaction T1 reads balance = $1000
  - Transaction T2 reads balance = $1000
  - T1 updates balance = $1000 - $100 = $900
  - T2 updates balance = $1000 - $200 = $800 (overwriting T1's update)
  - Final balance is $800, but should be $700

#### 2. Dirty Read Problem
- **Definition**: Occurs when a transaction reads data that has been modified by another transaction that hasn't yet committed.
- **Example**:
  - Transaction T1 updates balance = $1000 - $100 = $900
  - Transaction T2 reads balance = $900
  - T1 aborts and rolls back (balance returns to $1000)
  - T2 now has inconsistent data ($900 instead of $1000)

#### 3. Unrepeatable Read Problem
- **Definition**: Occurs when a transaction reads the same data twice and gets different values.
- **Example**:
  - Transaction T1 reads balance = $1000
  - Transaction T2 updates balance = $1000 - $100 = $900 and commits
  - T1 reads balance again, now gets $900

#### 4. Phantom Read Problem
- **Definition**: Occurs when a transaction re-executes a query that returns a set of rows and finds different rows.
- **Example**:
  - Transaction T1 reads all accounts with balance > $1000
  - Transaction T2 inserts a new account with balance = $1500
  - T1 reads all accounts with balance > $1000 again and finds a new row

### Concurrency Control Techniques

#### 1. Lock-Based Concurrency Control

Locks are mechanisms used to control access to data items. The two most common locks are:

##### a. Shared Lock (S-lock)
- Multiple transactions can hold shared locks on the same data item simultaneously.
- Used for read operations.
- If a transaction holds an S-lock, other transactions can also acquire S-locks, but not X-locks.

##### b. Exclusive Lock (X-lock)
- Only one transaction can hold an exclusive lock on a data item.
- Used for write operations.
- If a transaction holds an X-lock, no other transaction can acquire any type of lock on that data item.

**Example of Lock-Based Protocol**:
```
Transaction T1:
1. Acquire S-lock on account A
2. Read balance of account A
3. Release S-lock on account A
4. Acquire X-lock on account A
5. Update balance of account A
6. Release X-lock on account A
```

#### 2. Two-Phase Locking (2PL)

Two-Phase Locking is a protocol that ensures serializability by dividing transaction execution into two phases:

##### a. Growing Phase
- Transactions can acquire locks but cannot release any locks.

##### b. Shrinking Phase
- Transactions can release locks but cannot acquire any new locks.

**Types of Two-Phase Locking**:
- **Basic 2PL**: Locks are released gradually during the shrinking phase.
- **Strict 2PL**: All locks are held until the transaction commits or aborts.
- **Rigorous 2PL**: All locks are held until the transaction commits. Most commonly used in practice.

**Example of 2PL**:
```
Transaction T1:
1. Acquire S-lock on account A (growing phase)
2. Acquire X-lock on account B (growing phase)
3. Read balance of account A
4. Update balance of account B
5. Release S-lock on account A (shrinking phase)
6. Release X-lock on account B (shrinking phase)
```

#### 3. Timestamp-Based Concurrency Control

Each transaction is assigned a unique timestamp. The order of timestamps determines the serializability order of transactions.

**Basic Rules**:
- If transaction T1 wants to read/write an item last written by T2, and T1's timestamp is smaller than T2's, T1 is aborted and restarted with a new timestamp.
- If transaction T1 wants to write an item last read by T2, and T1's timestamp is smaller than T2's, T1 is aborted and restarted.

**Example**:
```
Transaction T1 (timestamp = 100) wants to update data item X,
which was last updated by Transaction T2 (timestamp = 105).
Since 100 < 105, T1 is aborted and restarted with a new timestamp.
```

#### 4. Multiversion Concurrency Control (MVCC)

MVCC maintains multiple versions of data items. Each transaction sees a snapshot of the database as it existed at the start of the transaction.

**Advantages**:
- Readers don't block writers, and writers don't block readers.
- High concurrency and good performance for read-heavy workloads.

**Example from PostgreSQL**:
Each row has hidden system columns:
- `xmin`: Transaction ID that created the row
- `xmax`: Transaction ID that deleted the row (or zero if not deleted)

A transaction with ID=100 will only see rows where:
- `xmin` ≤ 100 (row created before or by the current transaction)
- `xmax` = 0 or `xmax` > 100 (row not yet deleted or deleted after the current transaction)

#### 5. Optimistic Concurrency Control

Assumes conflicts are rare and doesn't use locks. Instead, it has three phases:

1. **Read Phase**: Transaction reads data and keeps track of read/write sets.
2. **Validation Phase**: Checks if conflicts would occur.
3. **Write Phase**: If validation succeeds, makes changes permanent.

**Example**:
```
Transaction T1:
1. Read Phase: Read account A balance = $1000
2. Compute: New balance = $1000 - $100 = $900
3. Validation Phase: Check if account A has been modified by another transaction
4. Write Phase: If not modified, update account A balance to $900; otherwise, abort and retry
```

## Isolation Levels

ANSI SQL defines four isolation levels that provide different trade-offs between consistency and performance:

### 1. Read Uncommitted
- Lowest isolation level
- Allows dirty reads
- No locks on SELECT statements
- Best performance but lowest consistency

### 2. Read Committed
- Prevents dirty reads
- Holds read locks briefly
- Allows unrepeatable reads and phantom reads
- Default isolation level in many DBMSs (e.g., PostgreSQL, SQL Server)

### 3. Repeatable Read
- Prevents dirty reads and unrepeatable reads
- Holds read locks until transaction end
- Allows phantom reads
- Default isolation level in MySQL/InnoDB

### 4. Serializable
- Highest isolation level
- Prevents all concurrency problems
- Implements full isolation between transactions
- Lowest performance but highest consistency

**Example**:
```sql
-- Set isolation level
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

BEGIN TRANSACTION;
  SELECT * FROM accounts WHERE account_id = 'A123';
  -- In serializable mode, this data will be consistent even if other
  -- transactions modify it before this transaction completes
  UPDATE accounts SET balance = balance - 100 WHERE account_id = 'A123';
COMMIT;
```

## Deadlocks

A deadlock is a situation where two or more transactions are waiting indefinitely for each other to release locks.

### Example of a Deadlock
```
Transaction T1:
1. Acquire X-lock on account A
2. Attempt to acquire X-lock on account B (but must wait because T2 holds it)

Transaction T2:
1. Acquire X-lock on account B
2. Attempt to acquire X-lock on account A (but must wait because T1 holds it)
```

Neither transaction can proceed, creating a deadlock.

### Deadlock Handling Techniques

#### 1. Deadlock Prevention
- **Timeout**: Abort a transaction if it has waited too long for a lock.
- **Wait-Die**: If T1 wants a lock held by T2, T1 waits if it's older than T2; otherwise, it "dies" (aborts).
- **Wound-Wait**: If T1 wants a lock held by T2, T1 "wounds" (forces abort) T2 if T1 is older; otherwise, T1 waits.

#### 2. Deadlock Detection
- Periodically check for cycles in a wait-for graph.
- If a cycle is detected, select a victim transaction to abort.

#### 3. Deadlock Avoidance
- Require transactions to declare their maximum resource needs in advance.
- Use algorithms like Banker's algorithm to determine if granting a request could lead to a deadlock.

## Recovery from Transaction Failures

Database systems need mechanisms to recover from failures without losing data or consistency.

### Types of Failures
1. **Transaction Failure**: Individual transaction fails due to logical errors or system constraints.
2. **System Failure**: System crashes but data on disk remains intact.
3. **Media Failure**: Physical damage to storage media.

### Recovery Techniques

#### 1. Write-Ahead Logging (WAL)
- Before making any changes to the database, write a log record describing the change.
- If a failure occurs, the log can be used to redo committed transactions and undo uncommitted ones.

#### 2. Checkpointing
- Periodically save the current state of the database to disk.
- After a failure, recovery only needs to start from the most recent checkpoint.

#### 3. Shadow Paging
- Keep two copies of the database: the current version and a shadow version.
- Make changes to the shadow version.
- If the transaction commits, make the shadow the new current version.

## Real-World Database Systems and Their Approaches

### PostgreSQL
- Uses MVCC for concurrency control
- Default isolation level: Read Committed
- Dead transactions are cleaned up by a background vacuum process

### MySQL (InnoDB)
- Uses a combination of MVCC and locking
- Default isolation level: Repeatable Read
- Automatic deadlock detection with transaction rollback

### Oracle
- Uses MVCC with read consistency
- Default isolation level: Read Committed
- Provides snapshot isolation through "Serializable" isolation level

### SQL Server
- Uses lock-based concurrency control with row versioning
- Default isolation level: Read Committed
- Supports snapshot isolation as an alternative to serializable

## Conclusion

Transaction management and concurrency control are crucial aspects of database systems that ensure data consistency in multi-user environments. Different applications may require different trade-offs between performance and isolation, and modern database systems provide various mechanisms to achieve these goals.

Understanding these concepts helps database administrators and application developers design systems that can handle concurrent operations efficiently while maintaining data integrity.
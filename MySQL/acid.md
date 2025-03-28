## ACID 

ACID properties are a set of principles that guarantee reliable transaction processing in database systems. They ensure that transactions are processed accurately and consistently, even in the face of errors or failures. Let's break down each property:

**1. Atomicity (A):**

* **Definition:** Atomicity ensures that a transaction is treated as a single, indivisible unit of work. Either all operations within the transaction are completed successfully, or none of them are. There's no partial completion.
* **Analogy:** Think of an "all-or-nothing" operation. If any part of the transaction fails, the entire transaction is rolled back to its initial state.
* **Example:** In a bank transfer, if the debit from one account succeeds but the credit to the other account fails, the entire transfer is canceled, and the debit is reversed.

**2. Consistency (C):**

* **Definition:** Consistency guarantees that a transaction brings the database from one valid state to another valid state. It ensures that the database remains consistent by adhering to predefined rules, constraints, and integrity checks.
* **Analogy:** The database follows its rules. If a transaction violates any rules, it's rejected, preserving the database's integrity.
* **Example:** If a database has a constraint that an account balance must always be non-negative, a transaction that would result in a negative balance is rejected.

**3. Isolation (I):**

* **Definition:** Isolation ensures that concurrent transactions are executed as if they were executed serially (one after the other). It prevents interference between transactions, ensuring that the effects of one transaction are not visible to other transactions until it's committed.
* **Analogy:** Transactions are kept separate from each other. One transaction cannot "see" the intermediate changes made by another transaction until the latter is complete.
* **Example:** If two transactions are updating the same account balance simultaneously, isolation ensures that the final balance is correct, as if the transactions had been executed one at a time. The database uses locking and other concurrency control mechanisms to achieve isolation.
* **Isolation Levels:** There are different levels of isolation (e.g., READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE), which offer varying degrees of protection against concurrency problems.

**4. Durability (D):**

* **Definition:** Durability ensures that once a transaction is committed, its changes are permanent and survive system failures (e.g., power outages, crashes).
* **Analogy:** Once a transaction is committed, it's written to persistent storage (e.g., disk) and cannot be lost.
* **Example:** After a bank transfer is committed, the changes are written to the database's transaction log and data files. Even if the server crashes immediately afterward, the transfer is guaranteed to be preserved when the system recovers.
* **Implementation:** Databases achieve durability through techniques like write-ahead logging (WAL), where changes are written to a log file before being applied to the data files.

**Importance of ACID Properties:**

* ACID properties are essential for ensuring the reliability and integrity of database transactions, especially in applications that handle critical data, such as financial systems, e-commerce platforms, and healthcare applications.
* They provide a framework for managing concurrent access to data and preventing data corruption.
* They simplify application development by relieving developers from the burden of implementing complex error-handling and recovery mechanisms.

```sql
-- Create a table for bank accounts
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    balance DECIMAL(10, 2) NOT NULL
);

-- Insert some sample accounts
INSERT INTO accounts (account_id, balance) VALUES
(1, 1000.00),
(2, 500.00);

-- Start a transaction to transfer funds
START TRANSACTION;

-- Atomicity: Debit from account 1
UPDATE accounts SET balance = balance - 200.00 WHERE account_id = 1;

-- Atomicity: Credit to account 2
UPDATE accounts SET balance = balance + 200.00 WHERE account_id = 2;

-- Consistency: Check if balances are valid (e.g., non-negative)
-- This is a simple check, real-world consistency checks are more complex.
-- If the balance of account one is less than zero, the transaction will rollback.
SELECT balance FROM accounts WHERE account_id = 1;

-- Example of a check, if the balance is less than zero, the transaction will be rolled back.
-- if (balance of account one) < 0 then ROLLBACK;

-- Commit the transaction (if everything is OK)
COMMIT;

-- Or, rollback the transaction (if something went wrong)
-- ROLLBACK;

-- Isolation: If another transaction tries to update the same accounts, it will wait
-- until this transaction is committed or rolled back.
-- The isolation level will determine the behavior of concurrent transactions.
-- The default isolation level is usually REPEATABLE READ.
-- Durability: Once committed, the changes are permanent.
-- You can check the account balances after the transaction:

SELECT * FROM accounts;
```

**Explanation and ACID Properties:**

1.  **Atomicity (A):**
    * `START TRANSACTION` begins a transaction.
    * The two `UPDATE` statements are treated as a single unit.
    * If either `UPDATE` fails, or if a `ROLLBACK` is issued, neither update will be applied.
    * `COMMIT` applies both updates, or `ROLLBACK` undoes both.

2.  **Consistency (C):**
    * The database moves from one valid state (balances before transfer) to another valid state (balances after transfer).
    * The example includes a check to ensure that the balance of account 1 is not negative. In a real application, you would add more complex constraints and checks.
    * If a constraint is violated, the transaction is rolled back, preventing an inconsistent state.

3.  **Isolation (I):**
    * MySQL's default isolation level (REPEATABLE READ) ensures that concurrent transactions don't interfere with each other.
    * If another transaction tries to update the same accounts while this transaction is running, it will wait until this transaction is committed or rolled back.
    * This prevents issues like lost updates or dirty reads.

4.  **Durability (D):**
    * Once `COMMIT` is executed, the changes are permanently stored in the database's transaction log and data files.
    * Even if the server crashes, the changes will be recovered when the database restarts.
    * MySQL uses write-ahead logging (WAL) to ensure durability.

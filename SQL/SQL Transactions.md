## What are SQL Transactions?
- s a sequence of one or more database operations (such as `INSERT`, `UPDATE`, or `DELETE`) treated as a single, indivisible unit of work
- either all the changes within the transaction are applied successfully, or none of them are

### Example: Money Transfer between 2 bank accounts
1. Deduct $100 from Account A
2. Add $100 to Account B
	- If 1 operation fails without a transaction, you risk inconsistent data - money deducted but not credited
	- By grouping these steps into a transaction, you ensure that both operations succeed or neither is applied

## Key properties of transactions: ACID #SQL_Transactions
### 1.Atomicity:
- means that a transaction is all or nothing
- if any part of the transaction fails, the entire transaction is rolled back -> leaving the database unchanged
```sql
BEGIN TRANSACTION; UPDATE accounts SET balance = balance - 100 WHERE account_id = 1; UPDATE accounts SET balance = balance + 100 WHERE account_id = 2; -- Commit only if both operations succeed COMMIT;
```
- If an error occurs during the second `UPDATE`, the database reverts to its original state, ensuring no partial changes

### 2. Consistency: 
- ensures that a transaction brings the database from one valid state to another
- all rules, constraints, and relationships are maintained throughout the transaction

#### Example:
- if a table has a `NOT NULL` constraint on a column, a transaction that attempts to insert a `NULL` value will fail, preserving data integrity

### 3. Isolation:
- ensures that transactions do not conflict with each other, even when executed simultaneously

#### Example:
- Say we have `BankAccount`

| id  | balance |
| --- | ------- |
| 1   | 100     |
Now, two users try to update the balance at the same time:
1. **User A** deposits `+50`.
2. **User B** withdraws `-30`

There are 2 isolation level that determine how the DB handles this:
1. **Read Committed/ Repeatable Read (with row-level locks**
	- When User A starts updating, the DB **locks** that row.
	- User B’s transaction must **wait** until User A commits or rolls back.
	- Final outcome depends on order of commit:
	    - If A commits first → balance = 150, then B’s withdrawal makes it 120.
	    - If B commits first → balance = 70, then A’s deposit makes it 120.  
	        ✅ Both lead to **consistent balance = 120**, no corruption.

2. **Serializable isolation**
	- The DB ensures transactions run as if they were executed **one after the other** (serially).
	- User B’s update won’t even start until User A finishes, or vice versa.
	- This is the strictest but can reduce concurrency.

### 4. Durability:
- guarantees that a database's changes are permanent once a transaction is committed, even during a system failure
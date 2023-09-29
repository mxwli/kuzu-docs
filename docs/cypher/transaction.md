---
title: Transaction
sidebar_position: 6
---

# Transaction

Kùzu is a transactional system. Specifically, it implements a transaction management sub-system that is atomic, durable and supports serializability (satisfying these properties is traditionally known as being ACID-compliant in database terminology). That is, every query, data manipulation command, every DDL (i.e., new node/rel table schema definitions), or `COPY FROM` commands to Kùzu is part of a transaction. Therefore they depict all-or-nothing behavior, so after these commands or a set of them execute and committed successfully, you are guaranteed that all of their changes will persist entirely. If they do not execute successfully or are rolled back, you are guaranteed that none of their changes will persist. These conditions hold, even if your system crashes at any point during a transaction. That is, after committing successfully, all your changes will persist even if there is an error after committing. Similarly, if your system crashes before committing or rolling back, then none of your updates will persist.

#### Important Properties of Kùzu Transactions: 
- Each transaction is identified as a write or read transaction (see below for how this is done).
- At any point in time, there can be multiple read transactions but one write transaction.
- There are two ways to use transactions: (i) manually beginning and committing/rolling back transactions; 
or (ii) auto-committing. These are reviewed below.

## Manual Transaction

### Transaction Statement
Kùzu supports the following transaction statements.
- `BEGIN READ TRANSACTION`: starts a read-only transaction.
- `BEGIN WRITE TRANSACTION`: starts a read-write transaction.
- `COMMIT`: commit change in current transaction.
- `ROLLBACK`: rollback change in current transaction.

### Manual Transaction
```
BEGIN WRITE TRANSACTION;
CREATE (a:User {name: 'Alice', age: 72});
MATCH (a:User) RETURN *;
COMMIT;
```
The above statements start a manual write transaction, add a new node, and within the same transaction also read all of the tuples in User table. Finally, commit the transaction.

You can also start a read-only transaction with `BEGIN READ TRANSACTION`. Read only transactions are not allowed to write to the database. You should start a read-only transaction for two main reasons: (i) if you want to run multiple read queries ensuring that the database does not change in-between those transactions; and/or (ii) you don't want to block a write transaction from writing to the database in parallel (recall that at any point in time Kùzu allows 1 write transaction in the system).

If you replace `COMMIT` with `ROLLBACK`, the added ('Alice', 72) node record will not persist in the database.

### Auto Transaction
If you send a command without manually beginning a transaction and it will automatically be wrapped around a transaction. For example, the following query will be automatically wrapped around a transaction that will be executed in a serializable manner.
```
CREATE (a:User {name: 'Alice', age: 72});
```
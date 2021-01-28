---
title: [WIP] Transactions in MySQL
date: 01-27-2021
---

In this post, I'll be going over transactions in MySQL with a focus on common pitfalls of using transactions, and how to avoid them. If you're not familiar with transactions, I suggest you do some additional research as the brief overview I provide may not suffice.

#### What's a *transaction* again?

A transaction, at its core, is an atomic (indivisible) unit of work. In SQL, transactions are usually composed of multiple statements that must *all* succeed in order to have any effect on the database. For example, if multiple `UPDATE` and `SELECT` statements are wrapped in a single transaction, it must be committed to alter the physical storage. If any of the queries fail or the transaction is rolled back, the database remains in the state it was in before the transaction started.

Transactions are a critical feature of relational databases and are supported by most major DBMSs. They are generally necessary in any sufficiently complex application. A standard example is transferring funds from one bank account to another. Two actions *must* happen, namely subtracting the specified amount from one account and adding it to the other. These two actions make up a single transaction, that is they both must execute successfully or have no effect.

#### Transaction Isolation Levels

Transactions must adhere to certain *isolation* properties which ensure consistency and reproducibility when there are many transactions running concurrently.

InnoDB, the default storage engine used by MySQL, offers four transaction isolation levels.

- **Repeatable Read** - This is the default isolation level for InnoDB. When operating at this level, `SELECT` statements are consistent in a single transaction. That is, if another transaction updates data in a set of rows that have been selected by a different transaction, the first transaction will not see the new values when issuing more `SELECT` statements. For example, consider the following transactions:
  ```sql
  START TRANSACTION;
  SELECT * FROM customers WHERE id = 1;  
  ```
  Suppose there are only two columns in the table, `(id, name)`, and the `SELECT` returns one result: `(1, "Rohan")`. In a separate connection, another transaction begins:
  ```sql
  START TRANSACTION;
  UPDATE customers SET name = "Jim" WHERE id = 1;
  ```
  After the second transaction updates the row, the first transaction issues the same `SELECT`. However, `name` is not updated. Repeatable Read guarantees that the result of subsequent reads are consistent with the first read in the transaction. Note that this applies even when a single transaction updates a row it had just selected. Attempting to select the record again in the same transaction will not return the updated record.


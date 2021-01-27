---
title: Transactions in MySQL
date: 01-27-2020
---

In this post, I'll be going over transactions in MySQL with a focus on common pitfalls of using transactions, and how to avoid them. If you're not familiar with transactions, I suggest you do some additional research as the brief overview I provide may not suffice.

What's a *transaction* again?

A transaction, at its core, is an atomic (indivisible) unit of work. In SQL, transactions are usually composed of multiple statements that must *all* succeed in order to have any effect on the database. For example, if multiple `UPDATE` and `SELECT` statements are wrapped in a single transaction, it must be committed to alter the physical storage. If any of the queries fail or the transaction is rolled back, the database remains in the state it was in before the transaction started.

Transactions are a critical feature in relational databases and are supported by most major DBMSs. They are generally necessary in any sufficiently complex application. A standard example is transferring funds from one bank account to another. Two actions *must* happen, namely subtracting the specified amount from one account and adding it to the other. These two actions make up a single transaction, that is they both must execute successfully or have no effect.


---
title: Transactions in MySQL
date: 01-27-2020
---

In this post, I'll be going over transactions in MySQL with a focus on common pitfalls of using transactions, and how to avoid them. If you're not familiar with transactions, I suggest you do some additional research as the brief overview I provide may not suffice.

Well what is a transaction?

A transaction, at its core, is an atomic (indivisible) unit of work. In SQL, transactions are usually composed of multiple statements that must *all* succeed in order to have any effect on the database. For example, if multiple `UPDATE` and `SELECT` statements are wrapped in a single transaction, it must be committed to alter the physical storage. If any of the queries fail or the transaction is rolled back, the database remains in the state it was before the transaction started.

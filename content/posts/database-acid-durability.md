+++
title = "Database ACID Principles: Durability"
date = "2024-12-04"
author = "Mateus Sampaio"
description = "Once a transaction is committed, the data will remain permanently and consistently stored, regardless of failures, power outages, or system restarts.."
+++

Picture the frustration of placing a big order with a waiter, only for them to forget it and force you to start over from the beginning. The **durability** concept in databases ensures that a confirmed operation is not lost, providing reliability for users.

Following this **ACID** principle, once a transaction is committed, the data will remain permanently and consistently stored, regardless of failures, power outages, or system restarts. Both PostgreSQL and MySQL implement durability using reliable mechanisms.

In PostgreSQL, the Write-Ahead Log (WAL) records every data modification before the transaction is committed, allowing full recovery in case of a failure. In MySQL (using the InnoDB storage engine), changes are written to a binary log before being applied to the database, ensuring data consistency and persistence.

Both systems balance efficiency and security to safeguard data while offering configurable adjustments to optimize performance in specific scenarios.

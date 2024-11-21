+++
title = "Database ACID Principles: Consistency"
date = "2024-11-22"
author = "Mateus Sampaio"
description = "Databases that follow the ACID principles ensure that each transaction only completes if it leaves the database in a valid state."
+++

Picture, for instance, two people in a kitchen preparing sandwiches. At the same moment, both reach for a piece of cheese. One of them could use all the cheese before the other finishes, leaving the second person without enough to complete their sandwich.

In databases, similar issues arise with concurrency when multiple transactions try to access or modify the same data simultaneously, leading to conflicts and an inconsistent state. This can be prevented by enforcing integrity rules such as locks or versioning, which ensure that every transaction respects the current state of the data and adheres to the system's rules.

For example, in a banking scenario, two transactions might simultaneously attempt to withdraw funds from the same account with insufficient balance to support both withdrawals. Without consistency mechanisms, this could result in an overdrawn account and inaccurate balances.

Databases that follow the ACID principles ensure that each transaction only completes if it leaves the database in a valid state. They achieve this by employing techniques like locks or transaction isolation to prevent conflicts, guaranteeing that the final state of the database remains accurate and consistent.

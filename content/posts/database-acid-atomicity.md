+++
title = "Database ACID Principles: Atomicity"
date = "2024-11-21"
author = "Mateus Sampaio"
description = "This principle is "all or nothing". When someone transfers money to another individual, for example, the transaction is either properly completed or not; even in the event of failures such as a database crash, there is no partial completion."
+++

# Database ACID Principles: Atomicity

Consider this situation: would you also throw away your appetizer and main course if your dessert fell off the plate? Thanks to **atomicity**, one of a transaction's fundamental characteristics, a relational database works almost exactly like that.

To put it simply, this principle is *"all or nothing"*. When someone transfers money to another individual, for example, the transaction is either properly completed or not; even in the event of failures such as a database crash, there is no partial completion.

Both **MySQL** and **PostgreSQL** guarantee transaction atomicity by utilizing techniques such as 𝘸𝘳𝘪𝘵𝘦-𝘢𝘩𝘦𝘢𝘥 𝘭𝘰𝘨𝘨𝘪𝘯𝘨 (WAL). In PostgreSQL, WAL records all transaction changes to disk before applying them to the database structure, enabling it to roll back incomplete operations if errors occur.

In MySQL, when using storage engines like InnoDB, atomicity is managed through 𝘶𝘯𝘥𝘰 𝘭𝘰𝘨𝘴, which roll back uncommitted changes to restore the database to its previous state in the event of failures. This ensures that a transaction is either fully completed or entirely rolled back, maintaining data consistency even in the face of unexpected problems.

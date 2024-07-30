---
title: Drop
description: Drop DDL statement
---

## Drop a table

Dropping a table removes the table and all its data from the database.

:::caution[Note]
- To drop a node table, you need to first drop all of the relationship tables that refer to X in
  its `FROM` or `TO` first.
- You can drop any relationship table without affecting its underlying nodes.
:::

For example, consider the following database:

```sql
CREATE NODE TABLE User(name STRING, age INT64, reg_date DATE, PRIMARY KEY (name));
CREATE REL TABLE Follows(FROM User TO User, since DATE);
```

Consider that you try to directly drop the `User` node table without first dropping the associated
relationship tables.
```sql
DROP TABLE User
```
This will raise the following exception:
```
Binder exception: Cannot delete a node table with edges. It is on the edges of rel: Follows.
```

You can first delete the `Follows` rel table, and subsequently the `User` table as follows:

```sql
DROP TABLE Follows
---------------------------------------
| RelTable: Follows has been dropped. |
---------------------------------------
DROP TABLE User
-------------------------------------
| NodeTable: User has been dropped. |
-------------------------------------
```

<<<<<<< HEAD
<<<<<<< HEAD
## Drop if exists
If the given table does not exist in the database, Kùzu throws an exception when you try to drop it.
To avoid the exception being raised, use the `IF EXISTS` clause. This instructs Kùzu to do nothing when
=======
## IF EXISTS
If the given table does not exist in the database, Kùzu throws an exception when you try to drop it. To avoid the exception being raised, use the `IF EXISTS` clause. This instructs Kùzu to do nothing when
>>>>>>> dff485a (Ziyi v0.5.0 (#182))
the given table name does not exist in the database.

Example:
=======
## IF EXISTS

Alternatively, you can avoid the exception being raised by using the `IF EXISTS` clause. This instructs
Kùzu to do nothing when the given table name does not exist in the database.

>>>>>>> c5e8393 (Updates to DDL, map and struct data type docs (#203))
```sql
DROP TABLE IF EXISTS User
```
<<<<<<< HEAD
<<<<<<< HEAD
This query tells Kùzu to drop the `UW` table only if it exists.
=======
This query tells Kùzu to drop the `UW` table only if it exists.
>>>>>>> dff485a (Ziyi v0.5.0 (#182))
=======

## Drop SEQUENCE

You can drop a `SEQUENCE` similar to the way you drop a table:

```sql
DROP SEQUENCE sequence_name;
```
>>>>>>> c5e8393 (Updates to DDL, map and struct data type docs (#203))

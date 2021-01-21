# Postgresql

## Problem with MySQL [1]
- When defining a field as int(11) you can just happily insert textual data and MySQL will try to convert it

```
mysql> create table example ( `number` int(11) not null );
Query OK, 0 rows affected (0.08 sec)

mysql> insert into example (number) values (10);
Query OK, 1 row affected (0.08 sec)

mysql> insert into example (number) values ('wat');
Query OK, 1 row affected, 1 warning (0.10 sec)

mysql> insert into example (number) values ('what is this 10 nonsense');
Query OK, 1 row affected, 1 warning (0.14 sec)

mysql> insert into example (number) values ('10 a');
Query OK, 1 row affected, 1 warning (0.09 sec)

mysql> select * from example;
+--------+
| number |
+--------+
|     10 |
|      0 |
|      0 |
|     10 |
+--------+
4 rows in set (0.00 sec)
```
- Any table modification (e.g. adding a column) will result in the table being locked for both reading and writing.
- Any operation using such a table will have to wait until the modification has completed. For tables with lots of data this could take hours to complete, possibly leading to application downtime
- SoundCloud to develop tools such as `lhm` to deal with this.
- 

## Postgresql [1]
- Has the capability of altering tables in various ways without requiring to lock it for every operation.
- Support for querying JSON
- Querying/storing key-value pairs
- Pub/sub support and more.
- PostgreSQL strikes a balance between performance, reliability, correctness and consistency.

### Transactional DDL


## Replication jargon
### Write-ahead log(WAL)
- Is the log that keeps track of all transactions. PostgreSQL makes the logs available to the slaves.
- Once slaves have puled the logs, they just need to execute the transactions therein.

### Synchronous replication
- A transaction on the master will not be considered complete until at least one synchronous slave listed in `synchronous_standby_names` and reports back.

### Asynchronous replication
- A transaction on the master will commit even if no slave updates
- This is expedient for distent servers where you don't want transactions to wait because of network latency, but the downside is that your dataset on the slave might lag behind

### Streaming replication
- The slave does not require direct file access between master and slaves. Instead, it relies on the PostgreSQL connection protocol to transmit the WALs. 

### Cascading replication
- Slaves can receive logs from nearby slaves instead of directly from the master

### Checkpoint
During its operation, PostgreSQL records changes to transaction log files, but it doesn't immediately flush them to the actual database tables. It usually just keeps the changes in memory, and returns them from memory when they are requested, unless RAM starts getting full and it has to write them out.

This means that if it crashes, the on disk tables won't be up to date. It has to replay the transaction logs, applying the changes to the on-disk tables, before it can start back up. That can take a while for a big, busy database.

For that reason, and so that the transaction logs do not keep growing forever, PostgreSQL periodically does a checkpoint where it makes sure the DB is in a clean state. It flushes all pending changes to disk and recycles the transaction logs that were being used to keep a crash recovery record of the changes.

## What is publication
A publication can be defined on any physical master. The node where a publication is dfined is refered to as publisher. A publication is a set of changes generated from a table or a group of tables.

## Configuration
1. pg_hba: host-based authentication

## Summary

you should never start your project with a non-relational data store. It's much easier to add one later on (which will probably never happen) than it is to move from non-relational to relational.
## References

https://developer.olery.com/blog/goodbye-mongodb-hello-postgresql/
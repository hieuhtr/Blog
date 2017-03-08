# What is FLUSH TABLES WITH READ LOCK in MySQL?

**Command:** 
```sql
mysql> FLUSH TABLES WITH READ LOCK;
```
**What exactly does command do?**

1. Set the global read lock - after this step, `insert/update/delete/replace/alter statements` cannot run
2. Close open tables - this step will block until all statements started previously have stopped
3. Set a flag to block commits

**It means:** 

- Closes all open tables 
- Locks all tables for all databases with a global read lock
- MySQL is in READ only mode, cannot WRITE anything, except one case (see below)
- `insert/update/delete/replace/alter` statements cannot run

**Note:**

- `FLUSH TABLES WITH READ LOCK` does **not prevent** the server from **inserting rows into the log tables**

**How to unlock and keep MySQL back to normal?**
- Use `UNLOCK TABLES` to release the lock

## Important:
- The session that holds the lock can read the table (but not write it)
- It prevents other sessions from modifying tables during periods
- If you don't keep this session open to keep Locking
   - Other sessions can write into tables
   - If you run `FLUSH TABLES WITH READ LOCK` for preparing `mysqldump`, you must keep this session, and run `mysqldump` in **another session**. 
   - Because If you don't keep, the `binlog` and the `position` of `binlog` may be changed during this session


--
***Hieu Huynh*** Mar 8, 2017.
---
date: 02-05-21
id: 5
path: ''
tags: []
title: PostgreSQL
type: note
---

Show the PG version in a SQL shell:

```sql
SELECT version();
```

Force psql to not require SSL during connection (ref https://www.postgresql.org/docs/current/libpq-connect.html):

```bash
 PGSSLMODE=prefer psql -h hostname -U username -d databasename
```

Reference: https://stackoverflow.com/questions/760210/how-do-you-create-a-read-only-user-in-postgresql


Query pg_stat_statements:

```sql
SELECT rolname, calls, total_time, mean_time, max_time, stddev_time, rows,
    regexp_replace(query, '[ \t\n]+', ' ', 'g') AS query_text
FROM pg_stat_statements
JOIN pg_roles r ON r.oid = userid
WHERE calls > 100
AND rolname NOT LIKE '%backup'
ORDER BY mean_time DESC
LIMIT 15;
```

Show long-running SQL queries:

```sql
SELECT pid, usename, now() - pg_stat_activity.query_start AS duration, query, state
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '2 minutes'
ORDER BY duration desc;
```

Cancel a query by PID or by user:

```sql
SELECT pg_cancel_backend(pid);
SELECT pg_cancel_backend(pid) FROM pg_stat_activity WHERE usename = 'username';
```

Display/terminate connections to a database:

```sql
-- Display database connections:
SELECT * FROM pg_stat_activity WHERE datname = 'targetdb';
-- Terminate database connection:
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity
WHERE pid <> pg_backend_pid() AND pg_stat_activity.datname = 'targetdb';
```

Create user (role) and grant:

```sql
CREATE USER mynewrole PASSWORD 'password';
ALTER USER mynewrole CREATEDB;
GRANT rolename TO otherrolename;
```

Create a readonly user:

```sql
GRANT SELECT ON ALL TABLES IN SCHEMA public TO username;
-- Automatically have default roles assigned to new objects in the future:
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO username;
```

Get the size of all databases:

```sql
SELECT t1.datname AS db_name, pg_size_pretty(pg_database_size(t1.datname)) AS db_size
FROM pg_database t1
ORDER BY pg_database_size(t1.datname) DESC;
```

Get the size of the ten largest tables:

```sql
SELECT
t.tablename,
pg_size_pretty(pg_total_relation_size('"' || t.schemaname || '"."' || t.tablename || '"')) AS table_total_disc_size
FROM pg_tables t
WHERE
t.schemaname = 'public'
ORDER BY
pg_total_relation_size('"' || t.schemaname || '"."' || t.tablename || '"') DESC
LIMIT 10;
```

Other versions:

```sql
SELECT pg_database.datname
           AS database_name,
       pg_database_size(pg_database.datname)
           AS database_size_bytes,
       pg_size_pretty(pg_database_size(pg_database.datname))
           AS database_size
FROM pg_database
UNION ALL
SELECT 'TOTAL'
           AS database_name,
       sum(pg_database_size(pg_database.datname))
           AS database_size_bytes,
       pg_size_pretty(sum(pg_database_size(pg_database.datname)))
           AS database_size
FROM pg_database
ORDER BY database_size_bytes ASC;
-- Database tables: number of rows and disk space:
SELECT stats.relname
           AS table,
       pg_size_pretty(pg_relation_size(statsio.relid))
           AS table_size,
       pg_size_pretty(pg_total_relation_size(statsio.relid)
           - pg_relation_size(statsio.relid))
           AS related_objects_size,
       pg_size_pretty(pg_total_relation_size(statsio.relid))
           AS total_table_size,
       stats.n_live_tup
           AS live_rows
FROM pg_catalog.pg_statio_user_tables AS statsio
JOIN pg_stat_user_tables AS stats
USING (relname)
WHERE stats.schemaname = current_schema  -- Replace with any schema name
UNION ALL
SELECT 'TOTAL'
           AS table,
       pg_size_pretty(sum(pg_relation_size(statsio.relid)))
           AS table_size,
       pg_size_pretty(sum(pg_total_relation_size(statsio.relid)
           - pg_relation_size(statsio.relid)))
           AS related_objects_size,
       pg_size_pretty(sum(pg_total_relation_size(statsio.relid)))
           AS total_table_size,
       sum(stats.n_live_tup)
           AS live_rows
FROM pg_catalog.pg_statio_user_tables AS statsio
JOIN pg_stat_user_tables AS stats
USING (relname)
WHERE stats.schemaname = current_schema  -- Replace with any schema name
ORDER BY live_rows ASC;
```

! Diagnose PG database performance issues

Method: https://jee-appy.blogspot.com.au/2016/10/debug-postgresql-performance-issues.html

```sql
# Select all long-running queries:
select pid, usename, now() - pg_stat_activity.query_start as duration, query, state from pg_stat_activity where (now() - pg_stat_activity.query_start) > interval '2 minutes' order by duration desc;
# Cancel all queries by database user:
select pg_cancel_backend(pid) from pg_stat_activity where usename='username';
# Terminate a query by PID:
select pg_terminate_backend(pid) from pg_stat_activity where pid=<PID>;
# Longest queries:
SELECT max(now() - xact_start) as duration FROM pg_stat_activity WHERE state IN ('idle in transaction', 'active');
SELECT pid, usename, datname, now() - xact_start as duration, state, query FROM pg_stat_activity WHERE state IN ('idle in transaction', 'active') order by duration desc;
```
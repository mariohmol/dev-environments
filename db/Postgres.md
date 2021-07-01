
## Users

Permissions:
```sql
GRANT USAGE ON SCHEMA databasename to "username";
GRANT ALL PRIVILEGES ON DATABASE databasename to "username";
```

## Backup

```sh
# Simple dump of database 
pg_dump dbname > outfile

# DDL and DML only
pg_dump  --no-privileges --no-owner -U postgres dbname > dump.sql

# Simple restore database
psql dbname < infile

# Restore an database from server
psql -U postgres -h amazon -d dbname -P < dump.sql
```

## Manage Process

```sh
pg_ctl start
pg_ctl stop

# Kill process by name
pkill pg_ctl

# Kill process by 
ps aux | grep pg_ctl
kill -9 PID
```

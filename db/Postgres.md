
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
# Start the Process
pg_ctl start

# Stop the process
pg_ctl stop

# Kill process by name
sudo pkill postgres

# Find the process
ps aux | grep postgres

# Kill process by Process ID
sudo kill -9 PID

# If you receive a message of lock file postmaster.pid
rm /usr/local/var/postgres/postmaster.pid
```

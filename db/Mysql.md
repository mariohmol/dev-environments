
* Manage the Service

```sh
sudo systemctl start mysql
sudo mysqld_safe --skip-grant-tables --skip-networking &
```

* Connect to server

```sh
mysql -u root -p
sudo mysql
```

* Changing password

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';

UPDATE mysql.user SET authentication_string = PASSWORD('new_password') WHERE User = 'root' AND Host = 'localhost';

FLUSH PRIVILEGES;

-- Alternative way
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');
FLUSH PRIVILEGES;
```


# Migrate Locale

* Dump the database schema (use your own user + password + database):

```sh
mysqldump -Q -d -h localhost -u root -p --default-character-set=latin1 --skip-set-charset old_database | sed 's/latin1/utf8/gi' > dump.schema.sql
```

* Dump the database data (use your own user + password + database):

```sh
mysqldump -Q --insert-ignore -t -h localhost -u root -p --default-character-set=latin1 --skip-set-charset old_database | iconv -c -f utf8 -t utf8 > dump.data.sql
```

Connect to the instance, 
```sh 
mysql -uadmin -pPASSWORD -hmyhost.com new-utf8
````


Create a new UTF-8 database:

```sh
CREATE DATABASE newdb_utf8 CHARACTER SET utf8 COLLATE utf8_general_ci;
```

Import into the new database:

```sh
mysql -h localhost -u root -p --default-character-set=utf8 newdb_utf8 < dump.schema.sql; 
mysql -u root -p --default-character-set=utf8 newdb_utf8 < dump.data.sql;
```

Other way to import files:
```sh
mysql
use newdb_utf8;
source dump.newdb.sql;
source dump.newdb.data.sql;
```
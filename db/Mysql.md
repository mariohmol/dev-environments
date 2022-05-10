
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
-- with a custom pass
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
-- with empty pass
ALTER USER 'root'@'localhost' IDENTIFIED BY '';
FLUSH PRIVILEGES;

UPDATE mysql.user SET authentication_string = PASSWORD('new_password') WHERE User = 'root' AND Host = 'localhost';
FLUSH PRIVILEGES;

-- Alternative way
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');
FLUSH PRIVILEGES;

-- grating previleged
SELECT host FROM mysql.user WHERE user = "root";
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
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

### Mysql 5.7 

Following the [instructions](https://www.fosstechnix.com/how-to-install-mysql-5-7-on-ubuntu-20-04-lts/):
```sh
apt-key adv --keyserver hkps://keyserver.ubuntu.com --refresh-keys
wget https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
dpkg -i mysql-apt-config_0.8.12-1_all.deb

apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 467B942D3A79BD29
apt install -f mysql-client=5.7.37-1ubuntu18.04

apt install -f mysql-community-client=5.7.37-1ubuntu18.04
apt install -f mysql-community-server=5.7.37-1ubuntu18.04

apt install -f mysql-client=5.7.37-1ubuntu18.04
apt install -f mysql-server=5.7.37-1ubuntu18.04
```


## Config files

* /etc/mysql/my.cnf
* /etc/mysql/conf.d/mysql.cnf
* /home/linuxbrew/.linuxbrew/etc/my.cnf



## Initialize


```sh
# With a custom datadir:
mysqld --initialize --datadir=/var/lib/mysql &
# granting tables (withou password)
mysqld --skip-grant-tables --datadir=/var/lib/mysql &
```

Custom properties:
```conf
# specific ip
bind-address                   = 168.0.0.1
# all ips
bind-address                   = 0.0.0.0
datadir=/var/lib/mysql-data
```



-----

## On Memory

Install mysql on memory for faster execution:

```sh
apt install mysql-server-core-5.7/bionic-security
mkdir /var/lib/mysql-data
mount -t tmpfs -o size=512m tmpfs /var/lib/mysql-data
chmod 700 /var/lib/mysql-data
sudo chown mysql:mysql /var/lib/mysql-data
sudo chown ubuntu:ubuntu /var/lib/mysql-data
sudo su - ubuntu

mysqld --skip-grant-tables --datadir=/var/lib/mysql --user=mysql &

mysql
# change the password as you
use mysql;
update user set authentication_string=password('123456') where user='root';
FLUSH PRIVILEGES;
```

## Process

To kill all the process for a specific user:
```sql
select concat('CALL mysql.rds_kill( ',id,');') from information_schema.processlist where user='admin'; 
```

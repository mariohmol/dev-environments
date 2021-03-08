# Server CentOS


Install dependencies

```sh
dnf groupinstall 'Development Tools'
yum install rsync nano mod_security
yum install epel-release -y
yum install perl-Net-SSLeay -y
yum install perl-Authen-PAM -y
yum install wget
```


**Supervisor**

```
yum install supervisor
supervisorctl status
supervisorctl start
```

**Nodejs**
```sh
yum module install nodejs/development
node --version
nano /etc/supervisord.d/appname.ini
```

**Snap**
```sh
yum install snapd
systemctl enable --now snapd.socket
snap install core; sudo snap refresh core
```
 
## Webmin


Install webmin:

```sh
(echo "[Webmin]
name=Webmin Distribution Neutral
baseurl=http://download.webmin.com/download/yum
enabled=1
gpgcheck=1
gpgkey=http://www.webmin.com/jcameron-key.asc" >/etc/yum.repos.d/webmin.repo; yum -y install webmin)
```

ls /etc/webmin/htusers


Install webming with nginx:
```sh
yum install wbm-virtualmin-nginx wbm-virtualmin-nginx-ssl
/etc/init.d/webmin status
/etc/init.d/webmin restart
```

Config:
```sh
nano /var/webmin/blocked
nano /etc/webmin/miniserv.conf
```
 


### VirtualMin

```sh
# Install
wget http://software.virtualmin.com/gpl/scripts/install.sh
/bin/sh install.sh

# Check logs
tail -f  /var/log/virtualmin/appname.com_access_log 
```

### Webmail

```
virtualmin modify-web --all-domains --webmail
```

## Mongo

Change the repos to add mongo: `nano /etc/yum.repos.d/mongodb-org.repo`

Add this to the conf file:
```conf
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc
```

Install Mongo:
```sh
dnf install mongodb-org
systemctl enable mongod --now
mongo
```

Config/Managment:
```sh
nano /etc/mongod.conf 
service mongod restart
```

 ## Firewall

```sh
# Managing using systemctl
systemctl status firewalld
systemctl disable firewalld
systemctl enable firewalld
systemctl stop firewalld
systemctl unmask firewalld

# Manage using firewall-cmd
firewall-cmd --zone=public --add-port=10000/tcp --permanent
firewall-cmd --reload
firewall-cmd start
firewall-cmd load
firewall-cmd --permanent --add-port=10000/tcp
```

### Iptables

```sh
# Backup the iptables rules
iptables-save > /root/tabsav

# Restore iptables
iptables-restore < /tmp/tabsav

#check the status
systemctl status iptables

# Conf
nano /tmp/tabsav

iptables -I INPUT 1 -p tcp --dport 10000 -j ACCEPT
```

   
## DNS

Conf
```sh
nano /etc/named.conf
nano /var/named/domain.com.hosts
nano /etc/sysconfig/named
systemctl restart named
```

Test DNS:
```sh
dig domain.com
dig NS domain.com 
dig @localhost NS domain.com
```

## Mail

```sh
# Install
yum install dkim-milter
# Config
cd  /etc/mail/dkim-milter/keys/
```  


## SSL


Install:
```sh
yum install certbot
yum install python3-certbot-apache.noarch
```

Conf
```sh
certbot --apache
# debug using logs
nano /var/log/letsencrypt/letsencrypt.log
# install mod prox
a2enmod proxy_http
```

## Network
  
**NC: Check Port**
```sh
# Install nc
dnf install nc

# list ports
nc -l -p 1299

netstat -plan|grep :80|awk {'print $5'}|cut -d: -f 1|sort|uniq -c|sort -nk 1

# check port 80
nc -zvw10 localhost 80

nc localhost 8010
```


**Speedtest**
```sh
ping 107.161.50.3
wget https://nj-lg.shockhosting.net/static/1000MB.test
wget -qO- bench.sh | bash
```

**Failtoban**
Fail2Ban is an intrusion prevention software framework that protects computer servers from brute-force attacks
```sh 
# Logs
tail -f fail2ban.log
```

**Netstat**
```sh
dnf install -y netstat
netstat -antup
netstat -an | grep :10000
```

Logs:

```sh
dnf install -y rsyslog
systemctl status rsyslog
dnf install -y syslog-ng
```

**Interface**
```sh  
# Install list of interfaces
yum install lsof

# list port 10000
lsof -i:10000

fuser 10000/tcp
ufw allow 10000

ifconfig
if
ifstat
ifnames

nano /etc/pam.d/smtp
```

## Utils

```sh
# find files
dnf install mlocate
# update the index for files
updatedb
# show top resources
top
# show free mem
free -m
```

Test server networks speed:
```sh
wget https://nj-lg.shockhosting.net/static/1000MB.test--2020-08-28
```


## Security


### CSF

Install 
```sh
cd /usr/src/
wget https://download.configserver.com/csf.tgz
tar -xzf csf.tgz
cd csf
sh install.sh
```

Config:
```sh
nano /etc/csf/csf.conf
nano /etc/csf/csf.allow 
```

Management:
```sh
csf -x
csf -e

# reload/restar
csf -r
service csf reload
service csf restart
```

## Nginx

Install:
```sh
yum install nginx
/etc/init.d/nginx start
systemctl start nginx
systemctl status nginx
systemctl enable nginx
```

Logs:
```sh
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
cd /etc/nginx/sites-available 
cd /etc/nginx/conf.d/php-fpm.conf 
```

Remove apache to install nginx:
`service httpd stop ; chkconfig httpd off`

### Certbot

```sh
yum install python3-certbot-nginx
```

## SSH

```sh
ssh-keygen
cat ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub 
ssh-keygen
cat ~/.ssh/id_rsa.pub
cd ~/.ssh/authorized_keys
touch ~/.ssh/authorized_keys
nano ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```


## Certbot

Install:
`yum install certbot python-certbot-apache`

Use:
`certbot --apache`

Check the logs at:
`nano /var/log/letsencrypt`


## Apache 

```sh
# Http conf
nano /etc/httpd/conf/httpd-le-ssl.conf
nano /etc/httpd/conf/httpd.conf
nano /etc/httpd/conf.d/appname.conf

# Folder with active apps
ls /etc/apache2/sites-available/

# Logs
nano /usr/local/apache/logs/error_log

# Manage httpd
service httpd status
service httpd restart
service httpd start
service httpd stop
```
 
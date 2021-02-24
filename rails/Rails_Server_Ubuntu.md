# Rails Server Ubuntu

Update system

```sh
sudo nano /etc/apt/sources.list
#add this line to the file
deb http://security.ubuntu.com/ubuntu bionic-security main
sudo apt update
sudo apt install g++ bison libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libyaml-dev sqlite3 libgmp-dev libreadline-dev libssl1.0-dev libtool pkg-config zlib1g-dev build-essential ruby-dev libmysqlclient-dev libcurl4-openssl-dev python2
```

Ruby usually ask for libssl1.0-dev, so install it:
```sh
sudo apt update && apt-cache policy libssl1.0-dev
sudo apt-get install libssl1.0-dev
```


Configure the limit for open files on linux:
```sh
sudo nano /etc/security/limits.conf 
# Add this lines
* hard nofile 97816
* soft nofile 97816
```
Check the configured limits using: `ulimit –nS`

## Ruby

Install [RVM](https://rvm.io/rvm/install):

```sh
gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash
```
To start using RVM you need to run `source /home/appname/.rvm/scripts/rvm`

Install ruby 2.1: `rvm install 2.1.10`
Install ruby 2.2: `rvm install 2.2`
Install ruby 2.3: `rvm install 2.3`

You can change the default version for ruby:
`rvm use --default 2.2.10`

Always use the app user to make the following installations.

## Brew

Install [brew](https://docs.brew.sh/Homebrew-on-Linux):
* `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

```sh
echo 'eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >> /home/appname/.bash_profile
eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
brew install imagemagick jpeg libtiff jasper gcc
```
- Run `brew help` to get started

## Node

You can install a single nodejs version:
```sh
sudo wget https://nodejs.org/dist/v14.15.4/node-v14.15.4-linux-x64.tar.xz
sudo tar --strip-components 1 -xf node-v* -C /usr/local
```

## Nginx 

Install the Nginx:
```sh
sudo apt install nginx passenger certbot nginx-extras
rvmsudo gem install passenger
passenger-config about ruby-command
```

Add to the `/etc/nginx/nginx.conf` , to support bigger post request for uploadfiles.
```conf
html {
        client_max_body_size 50M;
}
```


App configuration:
```sh
sudo nano /etc/nginx/sites-available/proprod.appname.com.conf
sudo cp /etc/nginx/sites-available/proprod.appname.com.conf /etc/nginx/sites-enabled/proprod.appname.com.conf
sudo certbot
```

Give permisssions for you app folder:
```sh
chmod g+x,o+x /opt/appname/
````

Create a basic Passenger File:
```
nano /opt/appname/Passengerfile.json
chown appname /opt/appname/Passengerfile.json
```

Files:
```sh
# Global nginx conf
sudo nano /etc/nginx/sites-enabled/default

# Prod conf
sudo nano /etc/nginx/sites-available/appname.com.conf

# Enable conf
cp /etc/nginx/sites-available/appname.com.conf /etc/nginx/sites-enabled/appname.com.conf
```

Example config for nginx for the app you can check in [AppName Prod Conf](../files/appname.com.conf).


### Passenger

```sh
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo nano /etc/apt/sources.list.d/passenger.list
`deb [arch=amd64] https://oss-binaries.phusionpassenger.com/apt/passenger focal main`
sudo apt update
sudo apt-get install libnginx-mod-http-passenger
rvmsudo passenger-install-nginx-module --trace
ldconfig
```

### Cert

```sh
sudo certbot --nginx -d preprod.appname.com
sudo certbot --nginx -d appname.com
apt install python3-certbot-nginx
```

## MySQL

Install MySQL using brew: `brew install mysql`

```sh
echo 'export PATH="/home/linuxbrew/.linuxbrew/opt/mysql@5.7/bin:$PATH"' >> /home/appname/.bash_profile
#For compilers to find mysql@5.7 you may need to set:
export LDFLAGS="-L/home/linuxbrew/.linuxbrew/opt/mysql@5.7/lib"
export CPPFLAGS="-I/home/linuxbrew/.linuxbrew/opt/mysql@5.7/include"

#For pkg-config to find mysql@5.7 you may need to set:
export PKG_CONFIG_PATH="/home/linuxbrew/.linuxbrew/opt/mysql@5.7/lib/pkgconfig"
```

You will have problems to install libmysql if you dont run `export PATH="/home/linuxbrew/.linuxbrew/opt/mysql@5.7/bin:$PATH"`into your path. The error will be something like: `/home/linuxbrew/.linuxbrew/bin/ld: cannot find -lmysqlclient`


You can enable security for the dabatase using:
```sh
mysql_secure_installation
mysql -uroot
```

The mysql config can be found at `/etc/mysql/my.cnf`.
After that you will be able to install the mysql2 gem without error: `gem install mysql2`


## Application 

Yu can use the gem maintenance to start/end the application during a deploy or migration process:
```sh
rake maintenance:start
rake maintenance:end
```

**Files to keep tracking**
```sh
# Nginx error
sudo tail -f /var/log/nginx/error.log
# Ruby email config
nano config/initializers/setup_mail.rb 
# If you use cache, refresh it when needed
redis-cli flushall
```

**Throubleshooting**
* https://youtu.be/ghAn4H3piaA
* https://youtu.be/fEUcEgpmHDc



## SSL

```sh
# Add new nginx domain for https
sudo certbot --nginx -d appname.com

# For wildcard subdomains
certbot certonly --manual -d *.appname.com -d appname.com --agree-tos --manual-public-ip-logging-ok --preferred-challenges dns-01 --server https://acme-v02.api.letsencrypt.org/directory

# If have issues, use the debug mode
sudo certbot --debug

# Renew 
certbot --nginx -d *.appname.com --force-renewal
```


### Migration Plam

1. Send an email to your use base scheduling a day , preferred at night and weekends, or when is less busy.
2. In the production server, stop your application, put a good Maintanance page to alert your users, run `rake maintenance:start`.
3. You had to have prepared the server before hand and copy most of the files. But after you stop your app put to rsync again and copy latest files.
4. Change your DNS A Record to point to the new server. The new server should be on Maitenance Page as well, until the rsync is done.
5. Run the certification process into the new server, ex: `sudo certbot --nginx -d mydomain.com.br`
6. When rsync is down, take of the Maintenance page and test a lot your app.

Good luck!
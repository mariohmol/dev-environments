# Rails Server CentOS

Install dependencies:

```sh 
sudo yum update
sudo yum install git epel-release dnf wget nano
sudo yum groupinstall 'Development Tools'
```


## Ruby

Install [RVM](https://rvm.io/rvm/install):

```sh
gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash
```
To start using RVM you need to run `source /home/centos/.rvm/scripts/rvm`

Install ruby 2.1: `rvm install 2.1.10`

## Brew

Install [brew](https://docs.brew.sh/Homebrew-on-Linux):
* `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

```sh
echo 'eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >> /home/centos/.bash_profile
eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
brew install imagemagick jpeg libtiff jasper gcc
```
- Run `brew help` to get started

## Node

```sh
sudo wget https://nodejs.org/dist/v14.15.4/node-v14.15.4-linux-x64.tar.xz
sudo tar --strip-components 1 -xf node-v* -C /usr/local
```

## Nginx

Install:
```sh
sudo yum install nginx
rvmsudo gem install passenger
passenger-config about ruby-command
sudo yum install passenger mod_passenger
sudo service nginx start
```

Configure:
```sh
sudo mkdir /etc/nginx/sites-available
sudo mkdir /etc/nginx/sites-enabled
sudo nano /etc/nginx/nginx.conf
# Add this line at the bottom, inside the http{}:  include /etc/nginx/sites-enabled/*;

# To use in Nginx with passenger
# passenger_ruby /home/centos/.rvm/gems/ruby-2.1.10/wrappers/ruby

sudo service nginx restart
```


### Passenger 

Install using the official [Passenger Doc](https://www.phusionpassenger.com/library/install/nginx/install/oss/el7/).

Install passenger with nginx support:
```sh
sudo yum install pygpgme curl nginx-mod-http-passenger
sudo curl --fail -sSLo /etc/yum.repos.d/passenger.repo https://oss-binaries.phusionpassenger.com/yum/definitions/el-passenger.repo
sudo yum install -y nginx passenger || sudo yum-config-manager --enable cr && sudo yum install -y nginx passenger
```


To find the path for `passenger_root`, you have to run `passenger-config --root`, 
then edit the passenger config: `sudo nano /etc/nginx/conf.d/passenger.conf`

Take the path from the command, config add the lines like this example: 
```conf
passenger_root /home/centos/.rvm/gems/ruby-2.1.10/gems/passenger-6.0.7/locations.ini;
passenger_ruby /usr/bin/ruby;
passenger_instance_registry_dir /var/run/passenger-instreg;
```

You can check the installation executing `sudo /usr/bin/passenger-config validate-install`.


Create an config for your app into nginx:
```sh
sudo nano /etc/nginx/sites-available/appname.conf
sudo cp /etc/nginx/sites-available/appname.conf /etc/nginx/sites-enabled/appname.conf
```

You can find here [Nginx config](../files/appname.com.conf) example.


### SSL

Use the certbot to install an ssl certificate with https:
```sh
yum install certbot python2-certbot-nginx
sudo certbot --nginx -d appname.com -d www.appname.com
```

## Application

Install the app in a folder `/opt/appname`

If your app use MySQL/Mariadb, you have to install some libraries.
```sh
sudo install mariadb-libs.i686 mariadb-devel
gem install mysql2 -v '0.3.17'
gem install bundler:1.17.3
npm run bundleinstall
```

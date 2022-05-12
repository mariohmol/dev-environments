# Ubuntu environment

A cheatsheet with step by step to configure a Ubuntu OS from the scratch with the best tools for development. 
Tested  on latest 20 LTS and 22 LTS.

Install [brew](https://brew.sh)
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Add this to your ~/.bashrc as well
# eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

sudo apt install build-essential
brew install gcc
```

**Troubleshoot**
If you have errors:
```sh
git config --global http.postBuffer 5242880000
git config --global http.sslVerify "false" 
```

Install [VSCode](https://code.visualstudio.com/docs/?dv=linux64_deb),


**Utils**
brew install wget
apt install locate

## Node

Install [NVM](https://github.com/nvm-sh/nvm#installing-and-updating)
for multiple node versions
```sh
source ~/.bashrc
nvm install 14
```

Install Single [Node](https://nodejs.org/en/download/)

Install useful node libs:
* `sudo npm i -g ts-node nodemon`

You can have issues on the future installing gihub repos as modules, to fix it use:
```sh
git config --global url."https://github.com/".insteadOf git://github.com/
```

## Ruby

You can check the official [RVM](* https://rvm.io/rvm/install)

```sh
# Install dependency
sudo apt install curl gawk g++ gcc autoconf automake bison libc6-dev libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libtool libyaml-dev make pkg-config sqlite3 zlib1g-dev libgmp-dev libreadline-dev libssl-dev

# Install RVM
\curl -sSL https://get.rvm.io | bash

# Open a new terminal and use
rvm use --login

# Install Ruby version 
rvm install 2.6.3

# Use the ruby version
rvm use 2.6.3

# Install Gems
sudo apt install ruby-rubygems

# Install bundler for gems control
sudo gem install bundler
```


### OPENSSL

Ruby needs openssl1.x to work, so we have to manually instal it.
On ubuntu 22 the only way it works is using brew.
```sh
# Using Brew
# First you can try to find the paths for zlib/openssl using:
brew info openssl
# Install if needed
brew install openssl@1.1
# For rbenv: brew install rbenv/tap/openssl@1.0
rvm install ruby-2.7.6 --with-openssl-dir=$(brew --prefix openssl)

# Check what brew --prefix openssl returns, if not manually include the folder on the command
rvm install ruby-3.0.4 --with-openssl-dir=/home/linuxbrew/.linuxbrew/opt/openssl@1.1

# or force the link with openssl: 
brew link --force openssl
```


If you still have issues with brew, use RVM:
```sh
# If you have issues installing old rubies.
# You can intall openssl using rvm:
rvm pkg install openssl
# Then install 
rvm reinstall 2.7.6 --with-openssl-dir=$HOME/.rvm/usr

# You can set the autolibs on with homebrew:
rvm autolibs enable
rvm autolibs homebrew
```

To test use: `ruby -ropenssl -e 'puts OpenSSL::OPENSSL_VERSION'`

You can try to manually include the paths for the openssl installation and try to compile ruby:
```sh
export LDFLAGS="-L/usr/local/opt/openssl@1.0/lib"
export CPPFLAGS="-I/usr/local/opt/openssl@1.0/include"
export PKG_CONFIG_PATH="/usr/local/opt/openssl@1.0/lib/pkgconfig"
export optflags="-Wno-error=implicit-function-declaration"
rvm_rubygems_version=2.7.3 rvm reinstall ruby-2.2.10 --with-openssl-dir=/usr/local/openssl@1.0/1.0.2t --with-openssl-lib=/usr/local/openssl@1.0/1.0.2t/lib --with-openssl-include=/usr/local/openssl@1.0/1.0.2t/include
```

### Libs

You might need some aditional libs for rails project:

```sh
# imagemagick
sudo apt-get install imagemagick


# zlib - To test zlib we can do
ruby -e'require "zlib"'
```

----

## Flutter

Use the [official docs](https://flutter.dev/docs/get-started/install/linux) or download, unzip and 
* sudo snap install flutter --classic
* Add: ~/projetos/flutter/bin
* flutter doctor --android-licenses
* flutter emulators --launch apple_ios_simulator


Install the Dart and Flutter Plugin for VSCode.

## Android

Install the [Android Studio](https://developer.android.com/studio) and:
* `brew install bundletool android-platform-tools`

Instruction for adding into [Ubuntu environment](https://docs.expo.io/workflow/android-studio-emulator/).


Add paths to profile:
```sh
ANDROID_SDK=$HOME/Library/Android/sdk || ANDROID_SDK=$HOME/Android/Sdk

echo 'export ANDROID_HOME=/Users/$USER/Library/Android/sdk' >> ~/.bashrc
echo 'export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/build-tools/28.0.3' >> ~/.bashrc
```

## Java

Install JSE 8 
* https://www.oracle.com/java/technologies/javase-jre8-downloads.html

Install JDK using the installer in [JDK download page](https://www.oracle.com/java/technologies/javase-jdk15-downloads.html)

## Python

```sh
sudo apt install python
```

## Docker

Installing [Docker on Ubuntu](https://computingforgeeks.com/how-to-install-docker-on-ubuntu/)
```sh
sudo apt -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
sudo apt install docker.io
sudo systemctl start docker.service
sudo systemctl enable docker.service
```

curl -s https://api.github.com/repos/docker/compose/releases/latest | grep browser_download_url  | grep docker-compose-linux-x86_64 | cut -d '"' -f 4 | wget -qi -
chmod +x docker-compose-linux-x86_64
sudo mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose


## Databases

### Tools

Install [Dbeaver](https://dbeaver.io/download/) to manage all databases.
Download the .dbk, use dpkg -i db file and if you have issues during the installation use `sudo apt install -f`

### PostgreSQL

`brew install postgres`

```sh
To have launchd start postgresql now and restart at login:
  brew services start postgresql
Or, if you don't want/need a background service you can just run:
  pg_ctl -D /usr/local/var/postgres start
```
### Mongo

Install [MongoDB](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/) with debian.
You might need a custom [OpenSSL](http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb) package:
```sh
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2_amd64.deb

# To manage the service
sudo service mongod start
sudo service mongod status

# Having issues check the log
/var/log/mongodb/mongod.log

# Check if this file is created
touch /tmp/mongodb-27017.sock

# Check if this folder was created as well
/var/lib/mongodb
```



#### Using Brew

Install with brew:
```sh
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

Or if you install with apt-get:
```sh
service mongod restart
```

if you don't want/need a background service you can just run:
  `mongod --config /usr/local/etc/mongod.conf`

Database location is `/usr/lobal/var/mongodb` (be confirmed by `brew cat mongodb-comunnity`).



* [Mongo Compass](https://www.mongodb.com/try/download/compass)
* [RoboMongo](https://robomongo.org/)


You can find the logs:
```sh 
tail -f /var/log/mongodb/mongod.log 
```

### MySQL

Install with apt
```sh
sudo apt-get install mysql-server-8.0 libmysqlclient-dev
```

**Using Brew**
Install mysql via brew, `brew install mysql`
To start the service use `brew services start mysql` e para conectar via console `mysql -uroot`

At the end it will echo export export LDF CPP.., run those commands.
To start use .../mysql/


If you need to have mysql@5.6 first in your PATH, run: echo 'export PATH="/home/linuxbrew/.linuxbrew/opt/mysql@5.6/bin:$PATH"' >> /home/username/.bash_profile For compilers to find mysql@5.6 you may need to set: export LDFLAGS="-L/home/linuxbrew/.linuxbrew/opt/mysql@5.6/lib" export CPPFLAGS="-I/home/linuxbrew/.linuxbrew/opt/mysql@5.6/include" Warning: mysql@5.6 provides a launchd plist which can only be used on macOS! You can manually execute the service instead with: /home/linuxbrew/.linuxbrew/opt/mysql@5.6/bin/mysql.server start




Use the [MySQLWorkbench](https://dev.mysql.com/downloads/workbench/) to manage the data using GUI.

For older versions: install `brew install mysql@5.7`, `brew postinstall mysql@5.7` and 
`brew services start mysql@5.7`.
If you need the paths, run `brew info mysql@5.7` to get the info.
For config go to: `/usr/local/etc/my.cnf`


apt install libmysqlclient-dev 
apt install mysql-devel ???

**START**
Every time you restart your computer, you might have to run `mysqld` to start a new 

#### Throubleshoot


Set a New Password:


If you have problems with installation, password or anything else, try this process:
```sh
# Test mysql port
telnet localhost 3600

brew remove mysql@5.7
brew cleanup

# Use the apt install for mysql
sudo apt install mysql-server-5.7
sudo systemctl edit mysql
```

Add this conf to the editor, press ctrl+x and Y to save.
```conf
[Service]
ExecStart=
ExecStart=/usr/sbin/mysqld --skip-grant-tables --skip-networking
```

Run the following to apply the changes:
```sh
sudo systemctl daemon-reload
sudo systemctl start mysql
# In case you want to test to stop and start
sudo systemctl stop mysql
```


## Misc

Gimp: Manipulate images
* `brew install gimp`

Network:
Test ports used by services like mysql, for instance:
* telnet localhost 3306

[Spotify](https://www.spotify.com/us/download/linux)

## RobotJS

sudo apt install libx11-dev
sudo apt install libxtst-dev
brew install -v glibc

## User Conf

**AUDIO**
Set default audio conf:
```sh
pactl list short sources  
# Example
pactl set-default-sink 'alsa_output.pci-0000_01_00.1.hdmi-stereo-extra1.monitor'
sudo nano /etc/pulse/default.pa

### Make some devices default - name or id?
set-default-sink alsa_output.pci-0000_00_1f.3.analog-stereo
set-default-source alsa_output.pci-0000_00_1f.3.analog-stereo.monitor

rm -r ~/.config/pulse and reboot
```

**SCREEN SHARING**
```sh
sudo nano /etc/gdm3/custom.conf
# Uncomment line , setting it to false
WaylandEnable=false
reboot
```
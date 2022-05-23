# Ubuntu environment

A cheatsheet with step by step to configure a Ubuntu OS from the scratch with the best tools for development. 
Tested on latest 20 LTS and 22 LTS.

Lets start installing basic libs
```sh
sudo apt install curl git build-essential gcc

apt-get install linux-headers-$(uname -r)
```

We will install some packages and most of them are Debian packages (.deb files). You can right click in the deb file -> Open with Other application -> Software Install. Or you can use the `dpkg -i filename.deb`. 
If you have issues running the package, you can try `sudo apt install -f` and then try to install the deb again.

Install [brew](https://brew.sh)
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Run the next steps they provide after this instalattion, something like this
# ==> Next steps:
# - Run these two commands in your terminal to add Homebrew to your PATH:
#     echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/dev/.profile
#     eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
# - Install Homebrew's dependencies if you have sudo access:
#     sudo apt-get install build-essential
#   For more information, see:
#     https://docs.brew.sh/Homebrew-on-Linux
# - We recommend that you install GCC:
#     brew install gcc
```

After this steps, close all your terminals.

**Troubleshoot**
If you have errors:
```sh
git config --global http.postBuffer 5242880000
git config --global http.sslVerify "false" 
```

Install [VSCode](https://code.visualstudio.com/docs/?dv=linux64_deb)

**Git config**
```sh
# Prevent the VSCode/Git to keep tracking file permissions (usually bugs on WSL)
# For the current repository
git config core.filemode false   

# Globally do: git config --global core.filemode false

# Configure your username
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```



## Node

Install [NVM](https://github.com/nvm-sh/nvm#installing-and-updating)
for multiple node versions
```sh
source ~/.bashrc
nvm install 14
```

Install Single [Node](https://nodejs.org/en/download/)

```sh
# Open a new terminal and install useful node libs:
npm i -g ts-node nodemon

# You can have issues on the future installing gihub repos as modules, to fix it use:
git config --global url."https://github.com/".insteadOf git://github.com/
```

## Ruby

You can check the official [RVM](* https://rvm.io/rvm/install)

```sh
# Install dependency
sudo apt install gawk g++ gcc autoconf automake bison libc6-dev libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libtool libyaml-dev make pkg-config sqlite3 zlib1g-dev libgmp-dev libreadline-dev libssl-dev

# Install RVM
\curl -sSL https://get.rvm.io | bash
```

### OPENSSL

Ruby needs openssl1.x to work, so we have to manually install it.
On ubuntu 22 the only way it works is using brew.

**Using Brew**

```sh
# First you can try to find the paths for zlib/openssl using: brew info openssl
# Install if needed
brew install openssl@1.1
# For rbenv: brew install rbenv/tap/openssl@1.0

# Check what brew returns for path
brew --prefix openssl
# it should return a path, if it points to a version different from 1.1, change manually
# for example, the commands returns: /home/linuxbrew/.linuxbrew/opt/openssl@3
# so use /home/linuxbrew/.linuxbrew/opt/openssl@1.1
rvm install ruby-2.7.4 --with-openssl-dir=/home/linuxbrew/.linuxbrew/opt/openssl@1.1
rvm use --default 2.7.4
# if doesnt work, try force the link with openssl: brew link --force openssl
```

**Using RVM**
This process works on Ubuntu 20. If you still have issues with brew, you can try using RVM:
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

**USING PATHS**
To test use: `ruby -ropenssl -e 'puts OpenSSL::OPENSSL_VERSION'`

You can try to manually include the paths for the openssl installation and try to compile ruby:
```sh
export LDFLAGS="-L/usr/local/opt/openssl@1.0/lib"
export CPPFLAGS="-I/usr/local/opt/openssl@1.0/include"
export PKG_CONFIG_PATH="/usr/local/opt/openssl@1.0/lib/pkgconfig"
export optflags="-Wno-error=implicit-function-declaration"
rvm_rubygems_version=2.7.3 rvm reinstall ruby-2.2.10 --with-openssl-dir=/usr/local/openssl@1.0/1.0.2t --with-openssl-lib=/usr/local/openssl@1.0/1.0.2t/lib --with-openssl-include=/usr/local/openssl@1.0/1.0.2t/include
```

Using RVM:
```sh
# (optional) Open a new terminal and use: rvm use --login

# Install Ruby version 
rvm install 2.7.4

# Use the ruby version
rvm use 2.7.4

# Install Gems
sudo apt install ruby-rubygems

# Install bundler for gems control
sudo gem install bundler


# **Libs**: You might need some aditional libs for rails project:
# 
# imagemagick
sudo apt-get install imagemagick

# zlib - To test zlib we can do: ruby -e'require "zlib"'
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
sudo apt -y install apt-transport-https ca-certificates gnupg-agent software-properties-common
sudo apt install docker.io
sudo systemctl start docker.service
sudo systemctl enable docker.service
```

curl -s https://api.github.com/repos/docker/compose/releases/latest | grep browser_download_url  | grep docker-compose-linux-x86_64 | cut -d '"' -f 4 | wget -qi -
chmod +x docker-compose-linux-x86_64
sudo mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose

### Kubernetes

Install [VirtualBox](#VirtualBox), start it and do:
* Go to Create Virtual Machine -> give a name, choose type Linux and version Ubuntu.
* Choose 5Gb of Ram, a dinamic hard drive, choose a location and create it.
* Right click on the new Virtual Machine -> Settings -> Storage -> IDE -> Choose Empty -> Click on the disk icon -> Choose a disk file -> Select  .iso for the Ubuntu install file - OK to save
* Start the Virtual Machine, this will open a Ubuntu installation and complete until the end.

## Databases

### Tools

Install [Dbeaver](https://dbeaver.io/download/) to manage all databases. For Ubuntu we can download the [Dbeaver Deb](https://dbeaver.io/files/dbeaver-ce_latest_amd64.deb).

### PostgreSQL

`brew install postgres`

```sh
To have launchd start postgresql now and restart at login:
  brew services start postgresql
Or, if you don't want/need a background service you can just run:
  pg_ctl -D /usr/local/var/postgres start
```

### Mongo


You might need a custom [OpenSSL](http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb) package:
```sh
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2_amd64.deb
```

Install following the [MongoDB Installation Docs](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition) with debian.

To manage Mongo you can use:
```sh
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

#### Install Mysql

Install with apt:
```sh
sudo apt-get install mysql-server-8.0 libmysqlclient-dev
```

**Using Brew**
Install mysql via brew, `brew install mysql`
To start the service use `brew services start mysql` e para conectar via console `mysql -uroot`

At the end it will echo export export LDF CPP.., run those commands.
To start use .../mysql/


If you need to have mysql@5.6 first in your PATH, run: echo 'export PATH="/home/linuxbrew/.linuxbrew/opt/mysql@5.6/bin:$PATH"' >> /home/username/.bash_profile For compilers to find mysql@5.6 you may need to set: export LDFLAGS="-L/home/linuxbrew/.linuxbrew/opt/mysql@5.6/lib" export CPPFLAGS="-I/home/linuxbrew/.linuxbrew/opt/mysql@5.6/include" Warning: mysql@5.6 provides a launchd plist which can only be used on macOS! You can manually execute the service instead with: /home/linuxbrew/.linuxbrew/opt/mysql@5.6/bin/mysql.server start



**Older MySQLs**
Use the [MySQLWorkbench](https://dev.mysql.com/downloads/workbench/) to manage the data using GUI.

For older versions: install `brew install mysql@5.7`, `brew postinstall mysql@5.7` and 
`brew services start mysql@5.7`.
If you need the paths, run `brew info mysql@5.7` to get the info.
For config go to: `/usr/local/etc/my.cnf`


**START**
Every time you restart your computer, you might have to run `mysqld` to start a new 

**Throubleshoot**

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


## RobotJS

sudo apt install libx11-dev
sudo apt install libxtst-dev
brew install -v glibc

## User Conf

### AUDIO

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

### SCREEN SHARING

```sh
sudo nano /etc/gdm3/custom.conf
# Uncomment line , setting it to false
WaylandEnable=false
reboot
```


### VIDEO CARD

```sh
# Instal view utils
sudo apt install mesa-utils
# check the driver
glxinfo -B
# Show the PCI Graphic card
lspci -nn | grep -E 'VGA|Display'
# check the fps with
glxgears

# Using OPen GL
sudo apt-get install glmark2
```

Download the [Official Drivers](https://www.nvidia.com/Download/index.aspx) specific for you video card and Linux 64bits.
If you dont know  exactly the video card,:
```sh
lspci
# You will see something like this
# 01:00.0 VGA compatible controller: NVIDIA Corporation GP106 [GeForce GTX 1060 6GB] (rev a1)
```

For linux and the driver requires nouveau to be off:

```sh
# add nouveau to blacklist and reboot the machine 
nano /etc/modprobe.d/blacklist.conf
blacklist nouveau

# after reboot, then give x permission and execute the file
chmod +x NVIDIA-Linux-x86_64-470.42.01.run
sudo ./NVIDIA-Linux-x86_64-470.42.01.run
```

**PLAYERS**

* For video player install VLC using Ubuntu Store


### AUDIO

* Install [Spotify](https://www.spotify.com/us/download/linux) with `snap install spotify`

**VIRTUAL SOUND**
You can use (Virtual sound)[https://askubuntu.com/questions/572120/how-to-use-jack-and-pulseaudio-alsa-at-the-same-time-on-the-same-audio-device] on Ubuntu using 
[Qjackctl](https://www.youtube.com/watch?v=fOsQr4DuSX0)

```sh
# Install libs
sudo apt install jackd2 pulseaudio-module-jack

# list audio devices
pacmd list-sinks
# connect jackd
pactl load-module module-jack-sink channels=2
# pacmd load-module module-jack-source channels=2; 
pactl load-module module-jack-source 


#####
# ALSA
#####
# list all source for recordings
arecord -L

# test alsa
alsamixer

# control i/o for every audio source
sudo apt-get install pavucontrol
pavucontrol

# if you have a integrated mic, you can find with
lspci

# if u have an usb mic, find with
lsusb

# if you mic/phone is working will show here
cat /proc/asound/cards

# you should see the connection with alsa
pacmd list-sources
# brings all the input/outpus
pacmd list-sources | grep name:
```


**PLAYERS**
Using Ubuntu Store install those:
* Qmmp: player like winamp
* Rhythmbox: Popular linux player
* Audacity: For recording (sudo snap connect audacity:alsa)


### Misc

Gimp: Manipulate images
* `brew install gimp`

Office files: Install LibreOffice using Ubuntu Store

Network:
Test ports used by services like mysql, for instance:
* telnet localhost 3306


**Utils**
brew install wget
apt install locate

**Comunication**
Download [Discord](https://discord.com/download) deb package and install it.

**Virtualization**
Install VirtualBox using `sudo apt install virtualbox`


### STREAM

For Stream we can use [OBS](https://obsproject.com/wiki/install-instructions#supported-builds)
```sh
sudo add-apt-repository ppa:obsproject/obs-studio
sudo apt update
sudo apt install obs-studio
```

### Migrate

**Chrome Passwords**
* Export your password visiting chrome://settings/passwords?search=pass and clicking in ... button , then Export passwords.
* Import your passwords visiting chrome://flags, search by Password import, enable it and relaunch chrome.
Go to chrome://settings/passwords?search=pass and on the ... button you have an option to import, so choose the export file and thats it.

**Database Connections**
* Export: Click on Dbeaver File menu -> Export -> DBeaver Project.
* Import: Click on  Dbeaver File menu -> Import -> DBeaver Project.

**Database Data**
* Export: Right click on a database -> Tools -> Dump Database and select all you want to export
* Import: 

**Export Database Data**
On Mysql Workbench, right click on a database -> Tools -> Dump Database and select all database you want to export.

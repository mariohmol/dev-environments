# Ubuntu Server for Node

Install dependencies:

```sh
sudo apt install curl 
```

Install NVM:

```sh
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
```

You can execute this same line for different users, usually `root` and `ubuntu` users.

Install node as you want:
```sh
nvm install 12
node --version
```


## Supervisor

Useful for keeping your nodejs project always running.

```
sudo apt install supervisor
```

To configure one app, you can create an file like `/etc/supervisor/conf.d/appname.conf` e add the following:
```conf
[program:appname]
command=node dist/main/server.js
process_name=%(program_name)s
numprocs=1
username=ubuntu
autostart=true
directory=/home/ubuntu/appname
stderr_logfile = /home/ubuntu/logs/appname-stderr.log
stdout_logfile = /home/ubuntu/logs/appname-stout.log
```

If you have issues with commands not found, like `can't find command 'node'`, you can set the environment for your task:
```conf
environment=PATH="/home/ubuntu/.nvm/versions/node/v12.18.3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```


To manage the apps, you can use:

```sh
# Restart the supervisor process
service supervisor restart 

# Reload a supervisor with new conf
supervisorctl reload

# Check the status for the apps configured
supervisorctl status
```

## PM2

An alternative for supervisor it's [PM2](https://pm2.keymetrics.io/docs/usage/quick-start/), for that install:
* `npm install pm2@latest -g`

Inside the project folder, you can start the execution:
* `pm2 start npm --name "appame" -- start`
* `pm2 restart appame`

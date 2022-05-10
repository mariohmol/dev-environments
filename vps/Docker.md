
# Docker

* sudo docker start container_ID_or_name
* sudo docker stop container_ID_or_name
* sudo docker container ls 
* sudo docker exec -it <container-id> bash
* sudo docker logs <container-id>
* sudo docker rm <container-id>
* sudo docker stop <container-id>
* sudo docker network ls
* sudo docker image ls
* sudo docker image prune -a
* sudo docker system prune -f

Scripts
* sudo docker ps --all
* sudo docker stop $(sudo docker ps -a -q)
* sudo docker rm $(sudo docker ps -a -q)

Reiniciar o serviço: sudo systemctl restart docker


## Docker Compose

Gerar versão:
* sudo docker-compose build

Gerenciar:
* sudo docker-compose up -d
* sudo docker-compose down
* sudo docker-compose down --remove-orphans
* sudo docker-compose restart

Config:
* sudo docker-compose config
* sudo docker-compose down --force-recreate

Custom conf file.
```sh
sudo docker-compose -f docker-compose-custom.yml restart

sudo docker-compose -f docker-compose-custom.yml build
sudo docker-compose -f docker-compose-custom.yml down --remove-orphans
sudo docker-compose -f docker-compose-custom.yml up -d --remove-orphans
```


Algumas tarefas úteis:
* sudo docker-compose run app rake db:create RAILS_ENV=production
* sudo docker-compose run app rake db:migrate RAILS_ENV=production


**RUN ON START**

```sh
sudo systemctl enable docker

and your services in your docker-compose.yml has
restart: always

all of the services run when you reboot your system if you run below command only once
docker-compose up -d
```


## SSL

You can use [self signed certs](https://codingwithmanny.medium.com/configure-self-signed-ssl-for-nginx-docker-from-a-scratch-7c2bcd5478c6)


```sh
apt install certbot

openssl req -x509 -nodes -days 365 -subj "/C=CA/ST=QC/O=Company, Inc./CN=mydomain.com" -addext "subjectAltName=DNS:mydomain.com" -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt;
```

Add the SSL confs to nginx:
```conf
listen 80 default_server;
listen [::]:80 default_server;
listen 443 ssl http2 default_server;
listen [::]:443 ssl http2 default_server;
ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
```

Copy the generated certs from the docker instance to your local computer:
```sh
# certificate
docker cp dockername:/etc/ssl/certs/nginx-selfsigned.crt ./;
# private key
docker cp dockername:/etc/ssl/private/nginx-selfsigned.key ~/Documents/dockername/config;
# nginx configuration file
docker cp dockername:/etc/nginx/conf.d/default.conf ~/Documents/dockername/config;
```

## Entrypoint

chmod +x entrypoint.sh
RUN ["chmod", "+x", "/usr/src/app/docker-entrypoint.sh"]


----


To deploy without building again, you can login in the instance
and the pull the changes, after just restart the docker and the updates will be live.

```sh
sudo docker exec -it 95bbca79f57b bash
root@95bbca79f57b:/myapp/app# nano config/myapp.yml
root@95bbca79f57b:/myapp/app# exit
sudo docker restart 95bbca79f57b

sudo docker container ls
```


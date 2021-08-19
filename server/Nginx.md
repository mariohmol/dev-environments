


## Config

### Body

Add to the `/etc/nginx/nginx.conf` , to support bigger post request for uploadfiles.
```conf
html {
        client_max_body_size 50M;
}
```



### Certbot

Use the certbot to install an ssl certificate with https:
```sh
sudo certbot --nginx -d appname.com -d www.appname.com
```

### Security

Remove sensitive data from nginx response headers
```sh
# Install nginx extras
sudo apt install nginx-extras

# Edit the nginx conf file
nano /etc/nginx/nginx.conf

# Add these conf under http
# Hide the nginx version
server_tokens off;
# Hide the server type
more_clear_headers Server;
# Hide any proxy header (passenger, cgi ...)
more_clear_headers 'X-Powered-By';

# Restart the server
nginx -t
service nginx restart
```


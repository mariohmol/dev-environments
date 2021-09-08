


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

# Add new nginx domain for https
sudo certbot --nginx -d appname.com

# If have issues, use the debug mode
sudo certbot --debug

# Renew 
certbot --nginx -d *.appname.com --force-renewal

# Renew all certs
certbot renew
```

#### Manual Instalation

To manually install a certification you need to to first execute certbot with manual flag like:
```sh
# For wildcard subdomains
certbot certonly --manual -d *.appname.com -d appname.com --agree-tos --manual-public-ip-logging-ok --preferred-challenges dns-01 --server https://acme-v02.api.letsencrypt.org/directory
```

* It will create and TXT token and a DNS name
* Create in your DNS records something like `_acme-challenge.appname.com. 3600 IN TXT "ToM3qi5_Toglx1n4O6cLToWe73Z90MxoeMHcK3VPSM8"`
* Restart your bind server: `service named restart`
* Check if the TXT has been updated: `dig _acme-challenge.appname.com txt`
* Accept final confirmation of certbot and it will be validate


### Security

Remove sensitive data from nginx response headers
```sh
# Install nginx extras
sudo apt install nginx-extras

# Edit the nginx conf file
nano /etc/nginx/nginx.conf
```

**Add these conf under http**
```conf
# Hide the nginx version
server_tokens off;
# Hide the server type
more_clear_headers Server;
# Hide any proxy header (passenger, cgi ...)
more_clear_headers 'X-Powered-By';

## Control Buffer Overflows ##
client_body_buffer_size  1K;
client_header_buffer_size 1k;
client_max_body_size 60M;
large_client_header_buffers 2 1k;

## Timeouts ##
client_body_timeout   60;
client_header_timeout 10;
keepalive_timeout     65;
send_timeout          30;

## Security Headers ##
# clickjacking
add_header X-Frame-Options SAMEORIGIN;
# content-type sniffing: instruct browsers that support MIME sniffing to use the server-provided Content-Type and not interpret the content as a different content type.
add_header X-Content-Type-Options nosniff;
# XSS filter
add_header X-XSS-Protection "1; mode=block";
# insecure SSL
add_header 'Content-Security-Policy' 'upgrade-insecure-requests';
# HSTS
add_header Strict-Transport-Security "max-age=3600; includeSubDomains" always;

# Strong protocols
ssl_protocols TLSv1.2 TLSv1.3;
```



# Restart the server

```sh
nginx -t
service nginx restart
```


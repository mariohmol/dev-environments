
## Connect via SSH

Creates an local port 27018 that drives connections to server
```sh
# Create ssh link
ssh -i ~/.ssh/id_rsa root@server.com -p 22 -Nf -L 27018:localhost:27017

# Connect to the server dv
mongo --port 27018 mydb
```


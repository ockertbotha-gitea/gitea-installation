# Setup HTTPS Using the Gitea built-in server on Ubuntu Server 18.04 LTS

## Introduction
I'll follow the information from the current
[Gitea docs](https://docs.gitea.io/en-us/https-setup/) so it might be worth
checking there for the most up to date information.

Be aware that if you've not setup HTTPS access to anything else before this can be a tricky process and if you have done it before there are probably parts
of this guide you may think can be improved; but here are the steps to follow for our installation:
### Create directory to store the keys
```
sudo mkdir -p /var/lib/gitea/keys
```

### Create self-generated certificate
This works for a host name or IP address
```
gitea cert --host [HOST]
```

Copy the key files into the keys directory and set the permissions
```
sudo cp /home/user/{cert.pem,key.pem} /var/lib/gitea/keys
sudo chown -R git:git /var/lib/gitea/keys
sudo chmod -R 750 /var/lib/gitea/keys
```

### Update the gitea and nginx configurations
Open the app.ini file
```
sudo nano /etc/gitea/app.ini
```

Change the following parts to reflect your values
```
[server]
PROTOCOL = https
ROOT_URL = `https://[HOST]`
HTTP_PORT = 3000
CERT_FILE = /var/lib/gitea/keys/cert.pem
KEY_FILE = /var/lib/gitea/keys/key.pem  
```

Restart gitea
```
sudo systemctl restart gitea
```

Check gitea has started correctly; if this is the case the following will
report that the service is active(running), if not it will detail the error.
```
sudo systemctl status gitea
```

Open the Gitea nginx configuration file
```
sudo nano /etc/nginx/sites-available/git
```

Change your file to the same as the following with the [HOST] parts
set to reflect your server
```
upstream gitea {
    server 127.0.0.1:3000;
}

server {
  listen                80 default_server;
  listen                [::]:80 default_server;
  server_name           [HOST];
  return 301            https://$host$request_uri;  
}

server {
    listen              443 ssl;
    listen              [::]:443 ssl;
    server_name         [HOST];
    ssl_certificate     /var/lib/gitea/keys/cert.pem;
    ssl_certificate_key /var/lib/gitea/keys/key.pem;
    root                /var/lib/gitea/public;
    access_log          /var/log/nginx/gitea-access.log;
    error_log           /var/log/nginx/gitea-error.log debug;

    location / {
      try_files maintain.html $uri $uri/index.html @node;
    }

    location @node {
      client_max_body_size 0;
      proxy_pass http://localhost:3000;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header Host $http_host;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_max_temp_file_size 0;
      proxy_redirect off;
      proxy_read_timeout 120;
    }
}
```

Test the config changes by running
```
sudo nginx -t
```

Reload the Nginx Service
```
sudo systemctl reload nginx.service
```

## Initial Configuration
Next web browse to your server or IP address:
```
http://example.com/install
```
and continue with the [guide.](/configuration/01-InitialConfiguration.md)

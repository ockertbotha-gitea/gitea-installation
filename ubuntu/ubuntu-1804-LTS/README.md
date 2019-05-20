# Gitea on Ubuntu Server 18.04 LTS

## Introduction
For much of this I'm going to follow Gitea's own
[Installation from binary](https://docs.gitea.io/en-us/install-from-binary/)
documentation and with additional information from
[this guide](https://www.vultr.com/docs/how-to-install-gitea-on-ubuntu-18-04).

## Install Nginx
```
sudo apt update
sudo apt -y install nginx
```

Use curl to check this worked and that Nginx is working:
```
curl http://example.com
```
If all is working this will return the html source of a page with a
title value of "Example Domain".

### Some handy systemctl commands
```
sudo systemctl stop nginx.service
sudo systemctl start nginx.service
sudo systemctl restart nginx.service
sudo systemctl reload nginx.service
sudo systemctl enable nginx.service
```

## Install Git
```
sudo apt -y install git
```
Check it worked with:
```
git --version
```
## Install MariaDB Database Server
```
sudo apt -y install mariadb-server mariadb-client
```

Secure the MariaDB server by creating a rood password and disallowing remote
access.
```
sudo mysql_secure_installation
```

When prompted provide the following answers (create your own password):
```
Enter current password for root (enter for none): Just press the Enter
Set root password? [Y/n]: Y
New password: Enter password
Re-enter new password: Repeat password
Remove anonymous users? [Y/n]: Y
Disallow root login remotely? [Y/n]: Y
Remove test database and access to it? [Y/n]:  Y
Reload privilege tables now? [Y/n]:  Y
```

Restart MariaDB
```
sudo systemctl restart mariadb.service
```

Test if MariaDB is installed, logon:
```
sudo mysql -u root -p
```
You will be prompted for the password you created earlier.

Once you're logged in, the MariaDB welcome message will be displayed.

Create a database called *gitea*.
```
CREATE DATABASE gitea;
```

Create a database user called *giteauser* with a new password.
```
CREATE USER 'giteauser'@'localhost' IDENTIFIED BY 'new_password_here';
```

Then grant the user fill access to the database.
```
GRANT ALL ON gitea.* TO 'giteauser'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;
```

Finally, save the changes and exit.
```
FLUSH PRIVILEGES;
EXIT;
```

## Prepare the Gitea environment
Creat a group for the user
```
sudo addgroup --system git
```

Create user to run Gitea
```
sudo adduser \
  --system \
  --shell /bin/bash \
  --gecos 'Git Version Control' \
  --ingroup git \
  --disabled-password \
  --home /home/git \
  git
```

## Create directory structure
```
sudo mkdir -p /var/lib/gitea/{custom,data,indexers,public,log}
sudo chown -R git:git /var/lib/gitea/{data,indexers,log}
sudo chmod -R 750 /var/lib/gitea/{data,indexers,log}
sudo mkdir /etc/gitea
sudo chown root:git /etc/gitea
sudo chmod 770 /etc/gitea
```

## Get the installation files
```
wget -O gitea https://dl.gitea.io/gitea/1.8/gitea-1.8-linux-amd64
chmod +x gitea
```

## Verify GPG signature
```
wget https://dl.gitea.io/gitea/1.8/gitea-1.8-linux-amd64.asc
gpg --keyserver pgp.mit.edu --recv 7C9E68152594688862D62AF62D9AE806EC1592E2
gpg --verify gitea-1.8-linux-amd64.asc gitea
```

## Copy Gitea binary to global location
```
sudo cp gitea /usr/local/bin/gitea
```

## Running Gitea
These instructions will setup Gitea to run as an Ubuntu systemd service.
We're going to be using Ubuntu 18.04 LTS.

### 1. Create the service descriptor
We'll create the file, copy the contents into it.
We have modified  the following items,
so check the values are relevant to your requirements:
- Since we're using MariaDB  uncomment this.
- User and home directory values.
```
sudo nano /etc/systemd/system/gitea.service
```

```
[Unit]
Description=Gitea (Git with a cup of tea)
After=syslog.target
After=network.target
#Requires=mysql.service
Requires=mariadb.service
#Requires=postgresql.service
#Requires=memcached.service
#Requires=redis.service

[Service]
# Modify these two values and uncomment them if you have
# repos with lots of files and get an HTTP error 500 because
# of that
###
#LimitMEMLOCK=infinity
#LimitNOFILE=65535
RestartSec=2s
Type=simple
User=git
Group=git
WorkingDirectory=/var/lib/gitea/
ExecStart=/usr/local/bin/gitea web -c /etc/gitea/app.ini
Restart=always
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea
# If you want to bind Gitea to a port below 1024 uncomment
# the two values below
###
#CapabilityBoundingSet=CAP_NET_BIND_SERVICE
#AmbientCapabilities=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
```

### 2. Enable the service to start at boot
```
sudo systemctl daemon-reload
sudo systemctl enable gitea
sudo systemctl start gitea
```
Check the status command
```
sudo systemctl status gitea
```

## Configure Nginx as a reverse proxy
Delete the default Nginx configuration file.
```
sudo rm /etc/nginx/sites-enabled/default
```

Create a reverse proxy configuration for Gitea and copy the contents:
```
sudo nano /etc/nginx/sites-available/git
```

Change the server_name value to the correct machine name / ip address value.
```
upstream gitea {
    server 127.0.0.1:3000;
}

server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name example.com;
    root /var/lib/gitea/public;
    access_log off;
    error_log off;

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

Enable the Gitea Nginx reverse proxy configuration
```
sudo ln -s /etc/nginx/sites-available/git /etc/nginx/sites-enabled
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

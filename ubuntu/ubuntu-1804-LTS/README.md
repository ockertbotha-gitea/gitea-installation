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

Use curl to check this worked that Nginx is working:
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

## Test
This runs Gitea, the application can be moved. It can be stopped with a good old
**Ctrl+C**
```
./gitea web
```

## Configure the Server

## 1. Prepare environment
Check that Git is installed. If not, install it first.
```
git --version
```
Create user to run Gitea
```
sudo adduser \
  --system \
  --shell /bin/bash \
  --gecos 'Git Version Control' \
  --group \
  --disabled-password \
  --home /home/git \
  git
```

## 2. Create directory structure
```
sudo mkdir -p /var/lib/gitea/{custom,data,log}
sudo chown -R git:git /var/lib/gitea/
sudo chmod -R 750 /var/lib/gitea/
sudo mkdir /etc/gitea
sudo chown root:git /etc/gitea
sudo chmod 770 /etc/gitea
```

## 3. Copy Gitea binary to global location
```
sudo cp gitea /usr/local/bin/gitea
```

## Running Gitea
These instructions will setup Gitea to run as an Ubuntu systemd service.
We're going to be using Ubuntu 18.04 LTS.

## 1. Create the service descriptor
We'll create the file, copy the contents into it.
We have modified  the following items,
so check the values are relevant to your requirements:
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
#Requires=mariadb.service
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

## 2. Enable set the service to start at boot
We'll create the file, copy the contents into it.

```
sudo systemctl enable gitea
sudo systemctl start gitea
```

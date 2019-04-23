# Gitea on Ubuntu 18.04 LTS

## Introduction
For much of this I'm going to follow Gitea's own
[Installation from binary](https://docs.gitea.io/en-us/install-from-binary/),
documentation.

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

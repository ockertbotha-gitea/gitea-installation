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
```
git --version
```

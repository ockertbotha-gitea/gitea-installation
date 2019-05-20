# Gitea configuration

This is a basic configuration, accessed by web browsing to
http://your-gitea-domain/install or http://your-gitea-ip/install

## Initial Configuration

### Database Settings
```
Database Type:  MySQL
Host:           127.0.0.1:3306
Username:       giteauser
Password:       new_password_here
Database Name:  gitea
```

### General Settings
```
Site Title:               Your Site Title
Repository Root Path:     /home/git/gitea-repositories
Git LFS Root Path:        /var/lib/gitea/data/lfs
Run As Username:          git
SSH Server Domain:        localhost
SSH Server Port:          22
Gitea HTTP Listen Port:   3000
Gitea Base URL:           http://localhost:3000
Log Path:                 /var/lib/gitea/log
```

### Optional Settings

#### Email Settings
Leaving these as standard for now.

#### Server and Third-Party Service Settings
Leaving these as standard for now.

#### Administrator Account Settings
```
Administrator Username:   new_administrator_name
Password:                 new_password
Confirm Password:         new_password
Email Address:            (must be provided if you want administrator account)
```
### Hit that install button!
The installer will automatically redirect you to a url with
http://localhost:3000/user/login which might work depending on your context,
but you'll probably need to change that part of it to
http://your-gitea-domain-or-ip/user/login

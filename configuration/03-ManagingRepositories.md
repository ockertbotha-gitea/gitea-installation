# Gitea managing repositories

This is a basic guide to user account management, start by browsing to
http://your-gitea-domain/admin/users or http://your-gitea-ip/admin/users

## Create User Accounts
The **User Account Management** screen lists all the current users and if you've
been following these guides there will be only be the one: *'gitadmin'*

Let's create a few users; press the **Create User Account** button and
make the following test user:

```
Authentication Source:            Local
Username:                         JaneTest
Email Address:                    jane@test.com
Password:                         password
Require user to change password:  unchecked                  
```

Press the **Create User Account** button. This creates the user account and
navigates to the associated **Edit User Account** page. As we're only creating
a few more test users navigate back to **../admin/users** and create
the following test users.

```
Authentication Source:            Local
Username:                         PeterTest
Email Address:                    peter@test.com
Password:                         password
Require user to change password:  unchecked                  
```

```
Authentication Source:            Local
Username:                         SusanTest
Email Address:                    susan@test.com
Password:                         password
Require user to change password:  unchecked                  
```

With that all done your lift of users should resemble the following:
![](screenshots/0201-user-accounts.png?raw=true)

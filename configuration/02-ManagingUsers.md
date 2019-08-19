# Gitea managing users

Managing users and groups is straightforward and what we'll do now is
configure our shiny new system to have two groups of users each with their own
project and with both groups having access to a shared project:
![](screenshots/0202-UserGroupProjectMap-v01.png?raw=true)

Start by browsing to
http://your-gitea-domain/admin/users or http://your-gitea-ip/admin/users

## Create User Accounts
The **User Account Management** screen lists all the current users and if you've
been following these guides there will be only be the one: *'gitadmin'*

Let's create a few users; press the **Create User Account** button and
make the following test user:

```
Authentication Source:            Local
Username:                         Jane
Email Address:                    jane@test.com
Password:                         password
Require user to change password:  unchecked                  
```

Press the **Create User Account** button. This creates the user account and
navigates to the associated **Edit User Account** page. As we're only creating
some test users navigate back to **../admin/users** and create
the following:

```
Authentication Source:            Local
Username:                         Peter
Email Address:                    peter@test.com
Password:                         password
Require user to change password:  unchecked                  
```

```
Authentication Source:            Local
Username:                         Linda
Email Address:                    linda@test.com
Password:                         password
Require user to change password:  unchecked                  
```

```
Authentication Source:            Local
Username:                         John
Email Address:                    john@test.com
Password:                         password
Require user to change password:  unchecked                  
```

```
Authentication Source:            Local
Username:                         Mary
Email Address:                    mary@test.com
Password:                         password
Require user to change password:  unchecked                  
```

```
Authentication Source:            Local
Username:                         Paul
Email Address:                    paul@test.com
Password:                         password
Require user to change password:  unchecked                  
```

With that all done your list of users should resemble the following:
![](screenshots/0201-UserAccounts-v04.png?raw=true)

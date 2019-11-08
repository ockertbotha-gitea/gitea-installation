# Gitea managing repositories

Now we're going to create the *Repositories* for our *Teams*:
![](screenshots/0202-UserGroupProjectMap-v02.png?raw=true)

In the time honoured tradition of TV cookery and software demos everywhere,
we have some repositories ready and in the oven.

## HelloWorld Repository
In the Gitea dashboard, from the menu options on top right, select
**+** then **New Migration**
Use the following values on the **New Migration** screen:
```
Migrate / Clone From URL: https://github.com/ockertbotha-gitea/hello-world.git
Clone Authorization:      Leave blank
Owner:                    TestCompany
Repository Name:          hello-world
Visibility:               Check to make private  
Migration Type:           Leave unchecked
Description:              Leave blank
```
Hit the **Migrate Repository** button.

Now transfer the repository to *Development*: From the Gitea dashboard select the
**Organization** button, then under *My Organizations* select **TestCompany**
then under *Teams* select **Development**, then select **repositories**.
```
Search repository: hello-world
```
click the **Add Team Repository** button.

## New DeviceConfig Repository
In the Gitea dashboard, from the menu options on top right, select
**+** then **New Migration**
Use the following values on the **New Migration** screen:
```
Migrate / Clone From URL: https://github.com/ockertbotha-gitea/device-config.git
Clone Authorization:      Leave blank
Owner:                    TestCompany
Repository Name:          device-config
Visibility:               Check to make private  
Migration Type:           Leave unchecked
Description:              Leave blank
```
Hit the **Migrate Repository** button.

Now transfer the repository to *Operations*: From the Gitea dashboard select the
**Organization** button, then under *My Organizations* select **TestCompany**
then under *Teams* select **Operations**, then select **repositories**.
```
Search repository: device-config
```
click the **Add Team Repository** button.

## New SupportWiki Repository
In the Gitea dashboard, from the menu options on top right, select
**+** then **New Migration**
Use the following values on the **New Migration** screen:
```
Migrate / Clone From URL: https://github.com/ockertbotha-gitea/support-wiki.git
Clone Authorization:      Leave blank
Owner:                    TestCompany
Repository Name:          support-wiki
Visibility:               Check to make private  
Migration Type:           Leave unchecked
Description:              Leave blank
```
Hit the **Migrate Repository** button.

Now transfer the repository to *DevOps*: From the Gitea dashboard select the
**Organization** button, then under *My Organizations* select **TestCompany**
then under *Teams* select **DevOps**, then select **repositories**.
```
Search repository: support-wiki
```
click the **Add Team Repository** button.

## Summary
At this point you should be able to login as any of the users and only be able
to see the repositories for the groups to which the user belongs. For now we're
going to stop our guides as this point.

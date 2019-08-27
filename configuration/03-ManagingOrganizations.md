# Gitea managing organizations and teams

We're going to create the *Organization* and *Teams* so that we can group our users:
![](screenshots/0202-UserGroupProjectMap-v02.png?raw=true)

Start by browsing to
http://your-gitea-domain/admin/orgs or http://your-gitea-ip/admin/orgs`

## Create Organizations
The **Organizations Management** screen lists all the current users and if you've
been following these guides there shouldn't be any.

Press the **New Organization** button:
```
Organization Name:  TestCompany
Visibility:         Private (Visible only to organization members)
```

Press the **Create Organization** button. This creates the user account and
navigates to the associated **TestCompany/dashboard** page. From here Press
the **View TestCompany** button which should take you to **../TestCompany**
.

## Create Teams
We will now add the teams.

### 1. DevOps
Press the **New Team** button and let's start with *DevOps*:
```
Team Name:          DevOps
Description:        The combined Development and Operations team.
Permission:         Write Access

Allow Access to Repository Sections: We will leave all of these on for now.
```

Press the **Create Team** button. This creates the team and navigates to
**../org/TestCompany/teams/devops**, now we'll add all the users to team;
search for each user by entering their name search bar and once found
press the **Add Team Member** button, do this for John, Mary, Paul, Jane,
Peter and Linda. Once all the users have been added the *DevOps* team should
look like this:
![](screenshots/0203-DevOpsTeam-v01.png?raw=true)

### 2. Development
Navigate to **.../org/TestCompany/teams** press that **New Team** button
and add *Development*:
```
Team Name:          Development
Description:        All the cool developers hang out here.
Permission:         Write Access

Allow Access to Repository Sections: We will leave all of these on for now.
```

Press the **Create Team** button, then navigate to
**../org/TestCompany/teams/development** and add Jane, Peter and Linda to
the team.

### 3. Development
Navigate to **.../org/TestCompany/teams** press that **New Team** button
and add *Development*:
```
Team Name:          Operations
Description:        All the gnarly technicians hang out here.
Permission:         Write Access

Allow Access to Repository Sections: We will leave all of these on for now.
```

Press the **Create Team** button, then navigate to
**../org/TestCompany/teams/operations** and add John, Mary and Paul to
the team.

## Teams
Now that we're done with creating teams and adding members if you browse to
**../org/TestCompany/teams** it should look like this:
![](screenshots/0203-AllTeams-v01.png?raw=true)

## Managing Repositories
Follow the [Managing Repositories Guide](./04-ManagingRepositories.md).

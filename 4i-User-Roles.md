#Basic Concepts

The security of MES can be divided in two large concepts:

Authentication :  The method used to verify who the user is.
Authorization: The method used to assign different permissions to operate the system to a user

##Authentication
4i uses the underlying SQL Server authentication. SQL Server has two different methods to authenticate logins:  Active Directory (AD) and  SQL Authentication. Both methods can be used in 4i.

###Active Directory
When a user opens the website, a login dialog opens, the user is prompted for a username and password
The combination is sent to AD for its validation, if it is valid the website is opened.

###SQL Authentication
When a user opens the website, a login dialog opens, the user is prompted for a username and password
The combination is sent to SQL Server for its validation, if it is valid the website is opened.

##Authorization

Once a user is identified, there a few tables in IHBoxSystem database that are used to grant different permissions. 

* A user can have zero or more profiles
* A profile can be assigned to zero or more users
* A profile has a list of Modules associated (Modules are typically MES Screens)
* A module has a list of Actions (Typically read and read/write)
* An action is associated to a SQL Application Role 
* A SQL Application Role has a list of allowed stored procedures

In addition to this a user can have Modules/Actions and/or SQL Roles assigned but this is not recommended.


![image.png](/.attachments/image-22f5ad06-edab-49f7-8955-baa91e8a2818.png)

# Users/Profiles Configuration Screens
There are two screens related to the security functions:

The Users Configuration screens are used to create users and assign profiles to them
![image.png](/.attachments/image-98852551-d656-4fc6-ab7b-a694ff94c888.png)
![image.png](/.attachments/image-fed69dff-6fe3-4f14-97c8-20900881a892.png)

The Profiles Configuration screen is used to assign modules/actions to profiles
![image.png](/.attachments/image-d052804e-3f61-41e7-adfb-e74191702b63.png)


#Internal Workings

##When a user is created using the web app 

- A SQLServer Login is created in SQL Server. This login is always of SQL User type (not windows user)

- if the user created is a Domain user, the login name is created by changing the backslash for a forwardslash ie. 
domain\jsmith => domain/jsmith, we will call this login name _shadow login_ . The password for this login is randomly generated and then stored in the column PasswordHash in IHBoxSystem.CAT.Users

- A database user is created in IHBoxSystem database pointing to the newly created login 

- Some standard roles are assigned to the database user  

## When a user tries to login 
There are two different pages for login: Login and Domain Login

If the user logs in using standard login:
Username and passwords are applied to a connection string (APP) in IndustrialDashboard web.config, a connection is attempted and if it works (basically SQL Server has a Login with that name and password), a long sql statement is called to retrieve the user's permissions. 

For domain users, the username and password are authenticated with the Domain Controller and if that succeds, the username is transformed into a SQL Login username by changing the \ into a / , then the password is retrieved from the CAT.Users table and then it follows the same logic as the standard login


##When a profile is changed by adding/removing permissions in the web app
- The permissions (actually database roles) are written in the "module actions" table

##When a user is assigned/deassigned to a profile in the web app
- The system reviews the database roles that the profile has and assign those roles to the database user





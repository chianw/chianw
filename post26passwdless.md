## Enabling passwordless sign-in for Azure portal

In this post, I will go through the steps of enabling passwordless authentication sign-in for Azure portal. With this, the user does not need to key in a password to sign into Azure portal, but rather the user is prompted via MS Authenticator mobile application to key in a number shown in on the portal during sign-in.

> Passwordless authentication is now the recommended method to log into Azure portal. See [here](https://www.microsoft.com/en-us/security/business/solutions/passwordless-authentication). To enable Passwordless sign-in to Azure, you need XXXX license. 

(https://github.com/chianw/chianw/blob/main/comparisonofauthc.png)


**1. Overview of setup - Azure SQL server and databases**

As setting up Azure SQL server is relatively straightforward, the step-by-step details of doing so are not shown for brevity. The below resources are all created in the same resource-group. The Azure SQL server in Australia East has 2 databases - geosqldb1 and TutorialDB. The Azure SQL server in Japan has the replicated instance of TutorialDB database, but not geosqldb1 which is not configured for geo-replication. To simplify testing, the database firewalls are configured to allow access from my public IP and basic user/password for SQL authentication is being used. 

![azuresqlreplica1.png](https://github.com/chianw/chianw/blob/main/azuresqlreplica1.png)
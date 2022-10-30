## Enabling passwordless sign-in for Azure portal with MS Authenticator

In this post, I will go through the steps of enabling passwordless authentication sign-in for Azure portal. With this, the user does not need to key in a password to sign into Azure portal, but rather the user is prompted via MS Authenticator mobile application to key in a number shown in on the portal during sign-in.

> Passwordless authentication is now the recommended method to log into Azure portal. See [here](https://www.microsoft.com/en-us/security/business/solutions/passwordless-authentication). To enable Passwordless sign-in to Azure, you need XXXX license. 

![comparisonofauthc.png](https://github.com/chianw/chianw/blob/main/comparisonofauthc.png)


**1. First go to Azure Active Directory > Manage > Security**

Select Authentication methods

![passwdless1.png](https://github.com/chianw/chianw/blob/main/passwdless1.png)

Then select Microsoft Authenticator

![passwdless2.png](https://github.com/chianw/chianw/blob/main/passwdless2.png)

**2. Enable passwordless for the user(s)**

![passwdless3.png](https://github.com/chianw/chianw/blob/main/passwdless3.png)


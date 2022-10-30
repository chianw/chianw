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

**3. Configure the passwordless settings**
In the next tab, you have the option to configure passwordless settings like number matching, showing geographical location etc. Remember to save your settings

![passwdless4.png](https://github.com/chianw/chianw/blob/main/passwdless4.png)

**4. At this point, you are done with the configuration on Azure Portal. Next you have to configure MS Authenticator to support passwordless**
Here it is assumed that the user already has MS authenticator installed and registered. Click on the "Enable phone sign-in" option and follow through the steps.

![passwdless7.png](https://github.com/chianw/chianw/blob/main/passwdless7.png)





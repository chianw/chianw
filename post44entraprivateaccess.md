# Microsoft Entra Private Access - basic setup

In this example, [Global Secure Access](https://learn.microsoft.com/en-gb/azure/global-secure-access/overview-what-is-global-secure-access) is used to provide private secured access to an IIS web server in a virtual network that is used to simulate an on-premises network. Also in the same *on-premises network* is a [Connector](https://learn.microsoft.com/en-gb/azure/global-secure-access/how-to-configure-connectors) which is essentially a Windows Server that has Proxy connector software installed. One or more connectors can be deployed on-premises to be part of a [Connector Group](https://learn.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-connector-groups) for high availability and for separating different groups of applications for access. The Connector will make outbound HTTP 80/HTTPS 443 connections to Microsoft Cloud.

The remote client is a VM in another virtual network that has the Global Secure Access (GSA) client installed. This VM **must** be Azure AD joined, not just registered. The GSA client will make outbound HTTP/2 GRPC connections to Microsoft Cloud. 

The GSA's outbound connection to Microsoft Cloud will ***meet*** the Connector's outbound connection to form a conduit for the client to access the IIS web server. 

![Entra Private Access](https://github.com/chianw/chianw/blob/main/entraprivateaccess.png)

The advantage of this is that there is no need to open up inbound ports on the on-premise network to allow remote clients to access the resources within it. 

## Pre-requisites
The following are pre-requisites for the solution:

 - Entra Premium P1 licenses 
 - Azure-AD **joined** remote clients
 - [Global Secure Administrator](https://learn.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#global-secure-access-administrator) to perform required configuration on Entra Admin portal

### Step 1 - Download the application proxy connector from Entra Admin Center
The Entra Admin Center is accessible at https://entra.microsoft.com and you have to log in with your credentials. Download the application proxy connector and note the installation pre-requisites.

![entraprivateaccess1.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess1.png)

### Step 2 - install the application proxy connector
Before installing application proxy connector, first disable IE security configuration. 

![entraprivateaccess5.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess5.png)

During installation you'll be prompted to sign into Entra

![entraprivateaccess6.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess6.png)

After successful login, the installation of application proxy connector should be successful.

![entraprivateaccess8.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess8.png)

### Verify successful application proxy connector installation
You can check on Entra admin center that the connector is registered.

![entraprivateaccess9.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess9.png)

You can also check on the Windows services of the host that the connector is running.

![entraprivateaccess10.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess10.png)

### Create connector group and add the connector to it
Here we create a connector group and add the connector to it. We also verify that the connector status is OK

![entraprivateaccess11.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess11.png)

Connector group with connector status as OK. Notice the recommendation to have at least 2 connectors in a group. Since this is for testing we will only use 1 connector

![entraprivateaccess12.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess12.png)

### Create new enterprise application and add application segment
Here we create a new application to represent the on-premise IIS web server. Associate it with the connector group created earlier and define the IP/port of the on-premise IIS webserver in the application segment. 

![entraprivateaccess29.png](https://github.com/chianw/chianw/blob/main/entraprivateaccess29.png)

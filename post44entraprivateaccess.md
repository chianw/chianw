# Microsoft Entra Private Access - basic setup

In this example, [Global Secure Access](https://learn.microsoft.com/en-gb/azure/global-secure-access/overview-what-is-global-secure-access) is used to provide private secured access to an IIS web server in a virtual network that is used to simulate an on-premises network. Also in the same *on-premises network* is a [Connector](https://learn.microsoft.com/en-gb/azure/global-secure-access/how-to-configure-connectors) which is essentially a Windows Server that has Proxy connector software installed. One or more connectors can be deployed on-premises to be part of a [Connector Group](https://learn.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-connector-groups) for high availability and for separating different groups of applications for access. The Connector will make outbound HTTP 80/HTTPS 443 connections to Microsoft Cloud.

The remote client is a VM in another virtual network that has the Global Secure Access (GSA) client installed. This VM **must** be Azure AD joined, not just registered. The GSA client will make outbound HTTP/2 GRPC connections to Microsoft Cloud. 

The GSA's outbound connection to Microsoft Cloud will ***meet*** the Connector's outbound connection to form a conduit for the client to access the IIS web server. 

[Entra Private Access](https://github.com/chianw/chianw/blob/main/entraprivateaccess.png)

The advantage of this is that there is no need to open up inbound ports on the on-premise network to allow remote clients to access the resources within it. 

## Pre-requisites
The following are pre-requisites for the solution:

 - Entra Premium P1 licenses 
 - Azure-AD **joined** remote clients

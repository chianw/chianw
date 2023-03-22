# End to End SSL with custom domain on Application Gateway Standard V2 and App Services

In this example, an Application Gateway is used to front an App Service and clients access the service via https://www.azcloudhub.com . The Application Gateway uses a self-signed certificate with CN=www.azcloudhub.com generated from Key Vault, while the App Sservice uses an App Service Managed Certificate with CN=www.azcloudhub.com , note that the App Service Managed Certificate is issued by public CA Digicert. The domain azcloudhub.com was registered via godaddy.com and the management of DNS record for this domain is delegated to an Azure public DNS zone. 

To simulate DNS resolution from the test local computer, host file entries are added to the Windows host file for www.azcloudhub.com pointing to the front-end public IP of the Application Gateway.

The important points to note:

 - the certificate associated with the frontend IP that is not the same as the certificate on the App Service. In practice you will use the same official CA-issued public certificate on both the App GW and the backend App Service
 - since the certificate presented by App GW to clients is self-signed, clients will need to accept the self-signed certificate to continue


A diagram for this is shown below
![Overview](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvcOverall.png)


> The steps of creating the Application Gateway and the backend pools are not explained in detail in this post. Rather the more important points/caveats are highlighted


## First create an Azure public DNS zone azcloudhub.com and configure GoDaddy to delegate the DNS management of azcloudhub.com to Azure DNS


![DNSdelegation](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc3.png)



## Custom Domain setting on App Service

Configure the App Service as normal, and set the custom domain setting as below. Copy the CNAME and TXT record to the Azure DNS zone that was created. Notice the Name Server records in the Azure DNS zone which is what has been copied over to GoDaddy

![appservicecustomdomain](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc1.png)

Azure DNS zone configuration

![OverrideHostname](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc2.png)


## Generate self-signed certificate in Azure Key Vault with CN=www.azcloudhub.com

![selfsigned](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc4.png)


## Create managed identity for the App GW to access the certificate in Key Vault and assigned necessary permissions in Key Vault Access Policies

![WindowsClientDNS](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc5.png)


## App GW version

![appgwversion](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc10.png)

## Listener settings of App GW

Set to use the self signed certificate from Key Vault

![appgwlistenersettings](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc9.png)


## Backend settings of App GW 

Since the App Service certificate is managed and issued by public CA, select "Well known CA certificate" and set to use custom health probe

![appgwbackendsettings](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc6.png)

## Health probe settings of App GW 

![appgwhealthprobe](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc7.png)

## Verify backend pool is healthy

![appgwbackendhealth](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc8.png)

## Modify host file entry on client to map AppGW public IP to www.azcloudhub.com

![hostfile](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc11.png)

## verify access to https://www.azcloudhub.com

![test](https://github.com/chianw/chianw/raw/main/e2eSSLAppGWAppSvc12.png)

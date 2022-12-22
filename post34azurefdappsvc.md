## Using Azure Front Door with custom domain for App Services

In this example, Azure Front Door is used to provide front-end caching capabilities for an App Service application. By default, Azure Front Door is accessible via a url like  https://xxxx.azurefd.net but in this example we configure a custom domain https://www.azfasttrack.com that provides access to Front Door and its backend App Service application. 

The normal steps of creating App Services and Traffic Manager are not documented here, but rather the more important points are stated in this article for the setup to work. In addition to creating Front Door and its backend origin of App Service, this example also uses an App Service Domain used to manage the DNS zone for azfasttrack.com

**DNS zone for azfasttrack.com**
Here we need to add a CNAME for www.azfasttrack.com pointing to the Azure Front Door FQDN. Note that the 2nd TXT record entry is automatically added to this DNS zone for validation after this custom domain has been added in Front Door

![afdappsvc-customdomain6.png](https://github.com/chianw/chianw/blob/main/afdappsvc-customdomain6.png)

**Adding www.azfasttrack.com as custom domain for Azure Front Door**
Note that after adding the custom domain to the Front Door, you need to validate the association which will automatically add TXT record into the DNS zone as per earlier screenshot, and you need to associate this custom domain with the Front Door origin.

![afdappsvc-customdomain4.png](https://github.com/chianw/chianw/blob/main/afdappsvc-customdomain4.png)

After associating with the Front Door route, you should see that the Front Door route works for both the original xxxx.azurefd.net as well as the custom domain www.azfasttrack.com 

![afdappsvc-customdomain2.png](https://github.com/chianw/chianw/blob/main/afdappsvc-customdomain2.png)


**Accessing https://www.azfasttrack.com and viewing the certificate**

![afdappsvc-customdomain.png](https://github.com/chianw/chianw/blob/main/afdappsvc-customdomain.png)


**Accessing https://afdappsvc-awcphufbbkffered.z01.azurefd.net and viewing the certificate**

![afdappsvc-customdomain7.png](https://github.com/chianw/chianw/blob/main/afdappsvc-customdomain7.png)

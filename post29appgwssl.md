## HealthCheck probe and App Gateway v1 Backend Settings for end to end SSL

I was testing end-to-end TLS encryption using Azure Application Gateway v1 and a HTTPS-enabled NGINX using self-signed certificate on the NGINX web server, and a separate self-signed certificate in PFX format installed on the Application Gateway. The Azure Application Gateway accepts certificates in PFX format for installation. This means there are 2 separate self-signed certificates involved, one on the NGINX web server and one on the Application Gateway itself. In production, you should be using public CA signed certificates instead of self-signed certificates. 

> Note for end to end SSL on Application Gateway v1, the backend web server certificate should be uploaded to the Application Gateway in order for it to trust the web server. In Application Gateway v2, you must upload the root CA public certificate to the Application Gateway instead of the backend web server certificate and this root CA must be the one that signed and issued the web server certificate.

![appgwv1-uploadwebcert.png](https://github.com/chianw/chianw/blob/main/appgwv1-uploadwebcert.png)


With this setup, this means there is end-to-end TLS between web client and Application Gateway, and between Application Gateway and the web server. In this setup, the following Common Names are used on the certificates issued to the Application Gateway and to the NGINX web server:
- CN=alpharednginx.io for the web server
- CN=bravoblue.io for the application gateway

![end2endssl.png](https://github.com/chianw/chianw/blob/main/end2endssl.png)

This post aims to document some of the settings on the health probe and backend settings of the Application Gateway to get the setup to work.

**1. Set the correct hostname for the Backend Settings of the Application Gateway, this should be equal to the CN of the certificate used HTTPS-enabled web server**


![backendsettings_correct.png](https://github.com/chianw/chianw/blob/main/backendsettings_correct.png)


**2. Set the Host value in the health probe to the equal to the certificate Common Name used by the web server**

![healthprobe.png](https://github.com/chianw/chianw/blob/main/healthprobe.png)


**3. Browsing directly to the web server reveals its certificate with CN=alpahrednginx.io**

![webserver.png](https://github.com/chianw/chianw/blob/main/webserver.png)

**4. Browsing to the Application Gateway public IP shows its certificate with CN=bravoblue.io**

![appgw.png](https://github.com/chianw/chianw/blob/main/appgw.png)




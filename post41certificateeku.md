# Creating certificate with extendedKeyUsage for use in Azure App Service

When adding a custom Enterprise Certificate to an Azure App Service, you need to ensure that the certificate contains extendedKeyUsage attribute of serverAuth. This post aims to describe the steps required to do this. First it is assumed that a Linux VM is being to install and configure Enterprise Root CA and subsequent certificates are signed by this CA.

## Creating root CA and generating Certificate Signing Request

The steps for creating a root CA and generating a Certificate Signing Request is documented [here](https://learn.microsoft.com/en-us/azure/application-gateway/self-signed-certificates). Follow these steps to get root CA up and running and to generate a Certificate Signing Request.


## Create a .cnf text file to include extendedKeyUsage of serverAuth

Next create a text file with .cnf extension that contains the extendedKeyUsage attribute of serverAuth, a sample is shown below and assume this is named myopenssl.cnf

```
[req]
distinguished_name = req_distinguished_name
req_extensions = v3_req

[req_distinguished_name]
countryName = SG
stateOrProvinceName = Singapore
localityName = CityHall
organizationName = NTUC
commonName = ntucapp1.azfasttrack.com

[v3_req]
extendedKeyUsage = serverAuth
```
## generate certificate from CSR and specify cnf file which includes serverAuth extension

Here using -extensions flag to reference the code block within the .cnf file that contains the ==extendedKeyUsage==
```
openssl x509 -req -in mycsr.csr -CA myrootca.crt -CAkey myrootca.key -CAcreateserial -out mysignedcert.crt -days 365 -sha256 -extfile myopenssl.cnf -extensions v3_req
```

## view the certificate
 openssl x509 -in ntucapp1new.crt -text![image](https://github.com/chianw/chianw/assets/91816369/04e12505-9a50-4efa-8ff4-fcc41afafc0a)




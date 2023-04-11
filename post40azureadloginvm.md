# Using Azure AD credentials to log into Azure Windows/Linux VMs

## To use Azure AD user/password authentication to log into Azure Windows VM:
- the client used for logging in must be registered to, or joined to the same Azure AD tenant as the Azure Windows VM
- the target Azure Windows VM will automatically be joined to Azure AD tenant when you select the option of logging in via Azure AD credentials during its deployment
- User(s) must be assigned Virtual Machine User Login or Virtual Machine Administrator Login roles at the same Resource Group as where Windows VM reside

## To use Azure AD user/password authentication to log into Azure Linux VM:
- there is NO NEED for the client that is used for logging in to be registered to, or joined to the same Azure AD tenant as the Azure Windows VM
- User(s) must be assigned Virtual Machine User Login or Virtual Machine Administrator Login roles at the same Resource Group as where Windows VM reside
- client used for logging in must have Azure CLI with SSH extension installed

## The target Azure Windows VM will be automatically joined to Azure AD tenant, while the client used to RDP to it has to be registered to the same Azure AD tenant

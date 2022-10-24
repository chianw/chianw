## Nested Azure Stack HCI setup experience

In this post, I will share some of the gotchas that I hit into while installing Azure Stack HCI in a nested environment on HyperV running on a Windows 11 laptop. The versions of software used are:

a.      Windows Server 2019 Standard running as Domain Controller and DNS server

b.      Windows Admin Center 2110.2 running on Windows Server 2019 Standard

c.      Azure Stack HCI 21H2

**Pre-requisites**:

a.      Download [Azure Stack HCI evaluation](https://azure.microsoft.com/en-us/contact/azure-stack-hci/), [Windows 2019 Server evaluation](https://info.microsoft.com/ww-landing-windows-server-2019.html) and [Windows Admin Center](https://www.microsoft.com/en-us/evalcenter/download-windows-admin-center)

b.      After the Azure Stack HCI VM is installed, it has to join the domain managed by the Windows 2019 domain controller

c.      Similarly after the Windows 2019 containing Windows Admin Center is installed, it has to join the domain managed by the Windows 2019 domain controller

I followed the instructions at [Deploy Azure Stack HCI on a single server](https://learn.microsoft.com/en-us/azure-stack/hci/deploy/single-server) to perform a single-node Azure Stack HCI installation.

### Gotcha #1 – need to enable nested virtualization on the Azure Stack HCI VM

### Gotcha #2 – need to have SCSI controller with at least 2 disks on the Azure Stack HCI VM in order to install Storage Space Direct or S2D

### Gotcha #3 – sometime using SCONFIG to change the IP address of the Azure Stack HCI VM interface IP address does not always work

### Gotcha #4 – Windows Admin Center does not show the option to register the Azure Stack HCI cluster if S2D is not enabled and configured

### Gotcha #5 – Unable to register Azure Stack HCI cluster to Azure from Windows Admin Center

### Gotcha #6 – Using PowerShell to register Azure Stack HCI cluster to Azure results in “unable to generate self-signed certificate errors” on the Azure Stack HCI cluster VM

### Gotcha #7 – Need to manually configure NAT for HyperV switch

### Gotcha #8 – Need to enable mac spoofing on HyperV vswitch

### Gotcha #9 – Disable checkpointing on HyperV to save disk space

### Gotcha #10 – transferring HyperV vhdx to Azure Blob as vhd for managed disk creation so that similar VMs can be created in Azure

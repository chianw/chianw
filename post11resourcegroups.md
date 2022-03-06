# Overview of management and resource groups

This diagram shows the relationship between management groups, subscriptions, resource groups and resources. A tenant can have multiple management groups and before a management group can be used it has to be tied to a subscription.

![azureresourcegrp](https://github.com/2cloudyskies/azure/blob/main/azureresource.png)

More information is available [here](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-setup-guide/organize-resources)

A management group can nest other management groups. 

A resource group cannot nest another resource group. A resource group is defined in a location, but resources in it do not have to be in the same location.

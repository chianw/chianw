# This post aims to explain how NSG and route tables apply to private endpoints in a virtual network subnet

By default, a NSG applied to a subnet that contains private endpoints will not have effect on traffic for the private endpoint. 

To have NSG control **inbound** traffic to the private endpoints in the subnet, you have to enable NSG in the Network Policy for the subnet as show below. Note that you cannot use NSG to control outbound traffic from private endpoint, only inbound traffic since PAAS service does not originate traffic - see this [article](https://learn.microsoft.com/en-us/azure/private-link/private-endpoint-overview#nsg-more-considerations)

![peppolicy1.png](https://github.com/chianw/chianw/blob/main/peppolicy1.png)



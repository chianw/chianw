## Using a custom route table to force traffic in a vnet to a network virtual appliance

This post describes a simple example of how you can use a custom route table to force traffic going out of the vnet to Internet to first traverse a network virtual appliance. By default every subnet within a virtual network has a default route to the Internet and workloads on the subnet can access the Internet directly. This is not a desired behaviour in many cases since most organisations want to control Internet access for their workloads in the cloud. This example shows how this can be done using a custom route table. The network virtual appliance deployed here is VYOS router that performs basic NAT for the egress traffic to Internet. In actual deployments organizations will want to use a true Firewall.

**Overview of lab setup**

Notice that the VYOS router is deployed in 2-arm mode: 1 interface facing Internet and 1 interface facing internal

![azure1rt](https://github.com/chianw/chianw/blob/main/azure1rt.png)  




**Custom Route table**

The custom route table is associated with vnet1-subnet1 and has a default route pointing to VYOS interface IP in the same subnet.

![azure2rt](https://github.com/chianw/chianw/blob/main/azure2rt.png)

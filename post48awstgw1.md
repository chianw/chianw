## Using AWS Transit Gateway with Network Firewall and NAT Gateway for EW and NS traffic

In many organizations using cloud services, a common network topology is to have a hub and spoke network topology, where the hub provides security services to enable inspection of spoke to spoke traffic and spoke to Internet. In Azure, this can be enabled via secured VWAN hub. In this example for AWS, I will be using Transit Gateway along with AWS Network Firewall and NAT Gateway to achieve the functionality of a secured hub.

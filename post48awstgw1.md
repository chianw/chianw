## Using AWS Transit Gateway with Network Firewall and NAT Gateway for EW and NS traffic

In many organizations using cloud services, a common network topology is to have a hub and spoke network topology, where the hub provides security services to enable inspection of spoke to spoke traffic and spoke to Internet. In Azure, this can be enabled via secured VWAN hub. In this example for AWS, I will be using Transit Gateway along with AWS Network Firewall and NAT Gateway to achieve the functionality of a secured hub.


### Resources involved in this setup:
- AWS account called 'Dev'. This contains the transit gateway, as well as a spoke VPC with 2 private subnets each containing 1 EC2 instance. The transit gateway is shared to the other 2 AWS accounts via Resource Access Manager
- AWS account called 'Sandbox'. This contains a spoke VPC with a private subnet and EC2 instance in it.
- AWS account called 'Prod'. This contains a VPC with 2 private subnets and 1 public subnet. The public subnet contains NAT Gateway, while 1 of the private subnet contains AWS network firewall and its Gateway LB, and the other private subnet servces for transit gateway attachment
- All the 3 AWS accounts are in the same organization. Resource Access Manager is configured to allow the Transit Gateway in 'Dev' account to be shared to 'Sandbox' and 'Prod' accounts.
- All resources created in the same region of ***ap-southeast-1*** 

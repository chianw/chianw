## Using AWS Transit Gateway with Network Firewall and NAT Gateway for EW and NS traffic

In many organizations using cloud services, a common network topology is to have a hub and spoke network topology, where the hub provides security services to enable inspection of spoke to spoke traffic and spoke to Internet. In Azure, this can be enabled via secured VWAN hub. In this example for AWS, I will be using Transit Gateway along with AWS Network Firewall and NAT Gateway to achieve the functionality of a secured hub.

<img width="1272" height="950" alt="image" src="https://github.com/user-attachments/assets/e9a0e01b-c684-4945-8e42-a07b35ac53ba" />



### Resources involved in this setup:
- AWS account called 'Dev'. This contains the transit gateway, as well as a spoke VPC with 2 private subnets each containing 1 EC2 instance. The transit gateway is shared to the other 2 AWS accounts via Resource Access Manager
- AWS account called 'Sandbox'. This contains a spoke VPC with a private subnet and EC2 instance in it.
- AWS account called 'Prod'. This contains a VPC with 2 private subnets and 1 public subnet. The public subnet contains NAT Gateway, while 1 of the private subnet contains AWS network firewall and its Gateway LB, and the other private subnet servces for transit gateway attachment
- The AWS Network Firewall is configured with a rule to allow ALL IP traffic and is configured to send logs to CloudWatch
- All the 3 AWS accounts are in the same organization. Resource Access Manager is configured to allow the Transit Gateway in 'Dev' account to be shared to 'Sandbox' and 'Prod' accounts.
- All resources created in the same region of ***ap-southeast-1*** 

### Important observations about this test:
- AWS Network Firewall does not perform NAT, unlike Azure Firewall. To support NAT there is a need for a separate NAT Gateway
- When AWS Network Firewall is created, it will be fronted by a Gateway Load Balancer by default. When configuring routes in route tables to send traffic to the Network Firewall, choose the next-hope as ***vpce-xxx*** which represents the Gateway Load Balancer. You will not be able to see the Gateway Load Balancer in the Load Balancer page
- When you attach a VPC to a transit gateway, you must specify one subnet from each Availability Zone to be used by the transit gateway to route traffic. Specifying one subnet from an Availability Zone enables traffic to reach resources in every subnet in that Availability Zone. Any resources in Availability Zones where there is no transit gateway attachment cannot reach the transit gateway
- The transit GW can have multiple route tables to help control the routing of traffic across the attachments. It has the concept of **association** and **propagation** . When an attachment is associated with a route table, it means it can use the routes the route table. When an attachment is propagated to a route table, it means the prefixes from the attachment are injected into the route table.
- By default, a transit GW will have 1 main default route that to which all attachments are associated with and propagated into
- Here we have 2 route tables for the transit GW. One route table has a default route to the attachment going to Firewall and is associated with all other attachments other than the one going to Firewall, and this is for pre-firewall inspection, and no attachments propagate to this route table. The other is a post-inspection route table has routes for all other spokes VPCs via their respective attachments, and is associated with the attachment going to the Firewall, and no attachments propagate to this route table as well

### Accessing EC2 instances without public IP in private subnets
- To access EC2 instances in a private subnet with only private IP, you can use EC2 Instance Connect Endpoint
- With 1 EC2 instance connect endpoint in a subnet of a VPC, you can use it to access EC2 instances in any subnet of the VPC
- During creation of the EC2 instance connect endpoint, you can associate it with a NSG that has 0 inbound rules and default allow all outbound rules. Then when apply NSGs to the EC2 instances, you can specify the source as the NSG of the EC2 instance connect endpoint and destination on TCP 22 to allow SSH to Linux EC2 instances


For example, the EC2 instance endpoint for the 'SEA' VPC has the following details. Even though it is in 1 subnet in 1 AZ, it can provide secure access to EC2 instances in other subnets and AZ in the same VPC.

<img width="1904" height="657" alt="image" src="https://github.com/user-attachments/assets/cdddef96-9a46-43f0-a2cb-c96709c0105a" />

The inbound and outbound rules of the NSG associated with this EC2 are shown below:

<img width="1905" height="597" alt="image" src="https://github.com/user-attachments/assets/0ead35f4-f453-44a9-982b-250659a22d57" />

<img width="1905" height="569" alt="image" src="https://github.com/user-attachments/assets/42ce1f3e-d6b8-4e09-be8d-26947d574745" />


The NSG for EC2 instances has an inbound rule specifying source as NSG of the EC2 instance endpoint and destination port 22.

<img width="1905" height="655" alt="image" src="https://github.com/user-attachments/assets/033173c9-4e49-4923-a9e3-71afa5ebf7a5" />


### Sharing of Transit Gateway from Dev to Prod and Sandbox accounts via Resource Access Manager
The transit gateway in Dev account is shared to Prod and Sandbox account via Resource Access Manager configuration in Dev account. Before Resource Access Manager can be used, it has to be first enabled in the RAM console via the management account of the organization. 

Enable sharing of resources in organization

<img width="1897" height="575" alt="image" src="https://github.com/user-attachments/assets/d470bc4b-881b-47f5-985e-1b052cdcbda8" />


<img width="1910" height="804" alt="image" src="https://github.com/user-attachments/assets/902394e3-a94c-4b0c-812b-3f383d44ba46" />


Sharing is via the 12 digit account numbers of Prod and Sandbox accounts


<img width="1892" height="926" alt="image" src="https://github.com/user-attachments/assets/ccd82e88-b449-4388-abb3-70ce017110ed" />

### Route table at spoke VPCs
The route table associated to the subnets at spoke VPCs will need a default route to the TGW attachment. As an example, the route table for SEA VPC is shown below:

<img width="1905" height="562" alt="image" src="https://github.com/user-attachments/assets/828f8a38-f848-4e7d-ae08-31960e2e60af" />

The route table is associated to both subnets across 2 AZs in SEA VPC.

<img width="1900" height="813" alt="image" src="https://github.com/user-attachments/assets/65ef2697-0599-4735-8f7e-75a456d08245" />

### Transit GW attachments and route tables
The TGW will have 3 attachments as shown below. The **SEA** attachment is to the SEA VPC, the **FIRE** attachment is the to Network Firewall VPC and the **SKY** attachment is to the Sandbox VPC. Note that the attachment request from Prod and Sandbox accounts to the TGW in Dev account need to be approved at the Dev account TGW before the attachment can be completed.
 
<img width="1905" height="574" alt="image" src="https://github.com/user-attachments/assets/4fbb956d-765b-4f26-a649-c83e2e3d8d7a" />


There are 2 route tables for the TGW. The **main** route table is associated with the attachments to spoke VPCs and the **firewall** route table is associated with the attachment to Network Firewall

<img width="1904" height="765" alt="image" src="https://github.com/user-attachments/assets/0d5c4d6a-4118-41b5-b824-86f4c9ef47af" />

The **main** TGW route table is the **pre-inspection** table that defines routes to send to the attachment for Network firewall. It is associated to the attachments to Dev and Sandbox.

<img width="2" height="1" alt="image" src="https://github.com/user-attachments/assets/716b10b9-78ee-46b7-b342-e7ff24d92bd6" />

No propagations to the **main** route table
<img width="1904" height="764" alt="image" src="https://github.com/user-attachments/assets/79bdd0ac-7d05-4b76-aaad-539ab1efb5c1" />

Default route on **main** route table to the attachment going to Network Firewall
<img width="1903" height="766" alt="image" src="https://github.com/user-attachments/assets/3c841c00-6fd2-42c5-9062-ec57ec21dd37" />

The **firewall** TGW route table contains routes for **post-inspection** by Network Firewall, it is used to send traffic back to the spoke VPCs via their attachments.

It is associated to the attachment to Network Firewall VPC
<img width="1905" height="768" alt="image" src="https://github.com/user-attachments/assets/9648e9e9-fc47-498b-9bed-b2c1ff8675bc" />

No propagation to the **firewall** TGW route table
<img width="1901" height="768" alt="image" src="https://github.com/user-attachments/assets/222f58c3-9d6c-4697-ab4b-533563c95e7a" />

Routes in the **firewall** route table to route traffic back to spoke VPCs via their attachments
<img width="1903" height="765" alt="image" src="https://github.com/user-attachments/assets/4e94f6e0-9c75-48da-a2b0-cc02d5a16549" />





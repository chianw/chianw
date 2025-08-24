## Differences between AWS S3 interface and gateway endpoints

This article explains the differences between AWS S3 interface and gateway endpoints, along with example configuration that show the differences. 

This table below summarizes the differences between the 2 types of endpoints for S3:

<img width="1441" height="560" alt="image" src="https://github.com/user-attachments/assets/05270054-0498-4c31-aa70-c2b32d0ce7e0" />


S3 Gateway endpoints are like service endpoints in Azure which allow an EC2 to use its private IP to access the public IP address of S3 bucket by routing it through a Gateway endpoint in the subnet. This means the EC2 instance can be in a private subnet without egress Internet access at all. It works by adding a managed prefix-list of S3 public IP addresses to the VPC and then adding a route for this managed prefix-list with next hop as the S3 gateway endpoint.

S3 Interface endpoints are like private endpoints in Azure which installs an Elastic Network Interface ENI with private IP address on the subnet, and modifies the DNS resolution for S3 to resolve to the private IP of the ENI. It also allows EC2 in private subnets without Internet access to securely connect to S3 via the private IP of the ENI.

### Interface endpoints

As a pre-requisite to use interface endpoints, the VPC must support DNS resolution

<img width="1817" height="683" alt="image" src="https://github.com/user-attachments/assets/f440f728-c7e8-40d3-8648-8fbd30283226" />

Select interface endpoint during creation

<img width="1895" height="872" alt="image" src="https://github.com/user-attachments/assets/160750ee-7115-46c4-973b-4f7544f6d621" />


Details of the interface endpoint

<img width="1909" height="882" alt="image" src="https://github.com/user-attachments/assets/871d483f-432a-421b-a9a6-3b1504ddca9c" />

DNS resolution for S3 from an EC2 in the subnet with interface endpoint shows that private IP addresses are returned for S3

<img width="763" height="403" alt="image" src="https://github.com/user-attachments/assets/6722235d-1680-41e1-b8f3-654c3b76c735" />



## Differences between AWS S3 interface and gateway endpoints

This article explains the differences between AWS S3 interface and gateway endpoints, along with example configuration that show the differences. 

This table below summarizes the differences between the 2 types of endpoints for S3:

<img width="1441" height="560" alt="image" src="https://github.com/user-attachments/assets/05270054-0498-4c31-aa70-c2b32d0ce7e0" />

S3 Gateway endpoints are like service endpoints in Azure which allow an EC2 to use its private IP to access the public IP address of S3 bucket by routing it through a Gateway endpoint in the subnet. This means the EC2 instance can be in a private subnet without egress Internet access at all. It works by adding a managed prefix-list of S3 public IP addresses to the VPC and then adding a route for this managed prefix-list with next hop as the S3 gateway endpoint.

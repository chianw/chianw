# Comparing Internet Gateway and NAT Gateway in AWS

## Internet Gateway
- One IGW per VPC
- serves as conduit to allow egress and ingress Internet traffic for the VPC
- no IGW = no Internet for the VPC regardless of whether NAT GW is deployed or if instances are assigned public IP
- a public subnet in the VPC is one that is associated with a route table that has 0.0.0.0/0 pointing to IGW
- IGW does not perform NAT, it's only a conduit to Internet for the VPC, resources in the VPC need public IP for Internet access

## NAT Gateway
- one per AZ
- there can be multiple NAT GW in a VPC if there are multiple AZ in the VPC
- comes into use when traffic egresses from VPC to Internet
- NAT GW has to be deployed in public subnet which is associated with route table having 0.0.0.0/0 pointing to IGW, otherwise it will not be able to perform SNAT for resources routing Internet-bound traffic to it

## Comparing Network ACLs and Security Group
The following link from AWS documentation does a [good comparison of NACL and SG](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html#VPC_Security_Comparison)

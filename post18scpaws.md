## Copying ssh private key to AWS Bastion Host

When you have a bastion EC2 host in a public subnet in AWS and you need to use it for SSH login to EC2 hosts in the private subnets, you may need to provide the SSH private key that was used during the creation of the EC2 instances as part of the SSH login. You can either copy and paste the private key file in the EC2 Bastion host, or you can scp the file to it.

```
scp -i myprivatekey.pem /Users/superman/Downloads/myprivatekey.pem ec2-user@X.X.X.X:/home/ec2-user

```

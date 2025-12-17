## This post compares the differences in using AWS Session Manager and EC2 instance connect endpoint to connect to EC2 instances with only private IP in private subnets

| Feature | **SSM Session Manager** | **EC2 Instance Connect (EIC) Endpoint** |
| :--- | :--- | :--- |
| **Network Placement** | Private subnet<br>(Private IP only) | Private subnet<br>(Private IP only) |
| **Outbound Internet Access** | **Required**<br>(via NAT Gateway or VPC Endpoints)<br>to reach SSM service | **Not Required**<br>(works entirely within the AWS backbone) |
| **Inbound Security Group Rules** | **None**<br>(No ports need to be opened) | **Required**<br>(SSH/RDP allowed from the EIC Endpoint SG) |
| **IAM Requirements** | Role with SSM,<br>CloudWatch and S3 logging permissions attached to EC2.<br>CloudWatch and S3 permissions needed if session recording is enabled | No additional IAM role required |
| **Agent Requirements** | `ssm-agent` must be installed<br>and running | No agent required<br>(standard SSH/RDP) |
| **Logging** | Native support for<br>CloudWatch and S3 logging | IAM-based logging;<br>records connections in CloudTrail |

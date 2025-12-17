## This post compares the differences in using AWS Session Manager and EC2 instance connect endpoint to connect to EC2 instances with only private IP in private subnets

| Feature | **SSM Session Manager** | **EC2 Instance Connect (EIC) Endpoint** |
| :--- | :--- | :--- |
| **Network Placement** | Private subnet (Private IP only) | Private subnet (Private IP only) |
| **Outbound Internet Access** | **Required** (via NAT Gateway or VPC Endpoints) to reach SSM service | **Not Required** (works entirely within the AWS backbone) |
| **Inbound Security Group Rules** | **None** (No ports need to be opened) | **Required** (SSH/RDP allowed from the EIC Endpoint SG) |
| **IAM Requirements** | Role with SSM, Cloudwatch and S3 Logging permissions must be attached to EC2. Cloudwatch and S3 permissions are needed if session recording is enabled | No additional IAM role required on the EC2 instance |
| **Agent Requirements** | `ssm-agent` must be installed and running | No agent required (standard SSH/RDP) |
| **Logging** | Native support for CloudWatch and S3 logging | IAM-based logging; records connections in CloudTrail |

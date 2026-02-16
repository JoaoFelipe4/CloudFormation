
The instance is publicly addressable (auto-assign public IP enabled) but has no open inbound ports – administration is intended through AWS Systems Manager Session Manager.

## Resources Created

| Resource                | Description                                      |
|-------------------------|--------------------------------------------------|
| VPC                     | 10.0.0.0/16 with DNS support                     |
| Internet Gateway        | Attached to the VPC                              |
| Public Subnet           | 10.0.1.0/24 in the first availability zone       |
| Route Table             | Routes 0.0.0.0/0 to the Internet Gateway         |
| Subnet Route Table Assoc| Associates the public subnet with the route table|
| Security Group          | Allows all outbound traffic; no inbound rules    |
| IAM Role                | Grants `AmazonSSMManagedInstanceCore` policy     |
| Instance Profile        | Links the IAM role to the EC2 instance           |
| EC2 Instance            | t4g.nano with Amazon Linux 2023 ARM64 AMI        |

## Prerequisites

- An AWS account with appropriate permissions (CloudFormation, EC2, IAM, VPC).
- AWS CLI installed and configured (optional, for CLI deployment).
- Sufficient service limits for EC2 instances and VPC components.

## Deployment

### Using AWS Management Console

1. Navigate to **CloudFormation** → **Create stack** → **With new resources (standard)**.
2. Upload the `demo.yaml` file.
3. Provide a stack name (e.g., `arm64-demo`).
4. Acknowledge IAM capabilities (the template creates IAM roles).
5. Click **Create stack**.

### Using AWS CLI

```bash
aws cloudformation create-stack \
  --stack-name arm64-demo \
  --template-body file://demo.yaml \
  --capabilities CAPABILITY_IAM"
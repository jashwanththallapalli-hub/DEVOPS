

## Introduction

This phase covers essential AWS services managed through Terraform, combining Infrastructure as Code (IaC) principles with AWS cloud architecture. You'll learn both the theoretical concepts behind each AWS service and the practical Terraform implementations.

### Learning Objectives

By the end of this phase, you will:
- Understand AWS core services and their architectural roles
- Configure Terraform to work with AWS securely
- Design and implement production-ready cloud infrastructure
- Apply security best practices across all resources
- Build scalable, highly available architectures

### Prerequisites

- AWS account with appropriate permissions
- Basic understanding of networking concepts
- Familiarity with Linux command line

---

## 1. AWS Provider Setup

### Theory: Understanding the AWS Provider

The **AWS Provider** is Terraform's interface to AWS APIs. It acts as a translator between your Terraform configuration and AWS's REST APIs, handling authentication, request formatting, and response parsing.

#### Key Concepts

| Concept | Description | Importance |
|---------|-------------|------------|
| **Provider Versioning** | Controls which version of the provider plugin to use | Ensures reproducibility and compatibility |
| **Authentication** | How Terraform proves its identity to AWS | Critical for security |
| **Region Selection** | Which AWS datacenter to deploy resources | Affects latency, compliance, and availability |
| **Default Tags** | Tags automatically applied to all resources | Enables cost tracking and resource management |

#### Authentication Methods Comparison

| Method | Security Level | Use Case | Pros | Cons |
|--------|---------------|----------|------|------|
| **Environment Variables** | Medium | CI/CD pipelines, local dev | Easy to rotate, no files | Can leak in logs |
| **AWS CLI Profile** | Medium | Multiple accounts, local dev | Named profiles, easy switching | Stored on disk |
| **IAM Instance Role** | High | EC2/ECS workloads | No credentials to manage | Only works on AWS |
| **Assume Role** | High | Cross-account access | Temporary credentials | More complex setup |

#### How Provider Authentication Works

```
┌─────────────────────────────────────────────────────────────┐
│                    Terraform Execution                       │
│                                                              │
│  1. Terraform reads provider configuration                   │
│  2. Provider checks for credentials in order:                │
│     a. Explicit credentials in config (NOT recommended)      │
│     b. Environment variables (AWS_ACCESS_KEY_ID, etc.)       │
│     c. Shared credentials file (~/.aws/credentials)          │
│     d. IAM instance profile (if running on AWS)              │
│  3. Provider authenticates with AWS STS                      │
│  4. AWS returns temporary session credentials                │
│  5. Provider uses credentials for all API calls              │
└─────────────────────────────────────────────────────────────┘
```

### Code Implementation

#### Basic Provider Configuration

```hcl
# terraform.tf - Provider requirements
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"      # Official HashiCorp AWS provider
      version = "~> 5.0"              # Allow 5.x versions (5.0, 5.1, etc.)
    }
  }
}

# provider.tf - AWS provider configuration
provider "aws" {
  region = "us-east-1"
  
  # Optional: Use named profile from ~/.aws/credentials
  profile = "default"
  
  # Optional: Assume a different role for elevated permissions
  assume_role {
    role_arn     = "arn:aws:iam::123456789012:role/TerraformRole"
    session_name = "TerraformSession"
  }
  
  # Default tags applied to ALL resources created by this provider
  # This is a best practice for cost allocation and resource management
  default_tags {
    tags = {
      ManagedBy   = "Terraform"
      Environment = var.environment
      Project     = var.project
      Owner       = var.owner
    }
  }
}
```

#### Authentication Methods

**Method 1: Environment Variables (Recommended for CI/CD)**

```bash
# Set these before running Terraform
export AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
export AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
export AWS_DEFAULT_REGION="us-east-1"

# Optional: For assuming roles
export AWS_ROLE_ARN="arn:aws:iam::123456789012:role/TerraformRole"
```

**Method 2: AWS CLI Profile (Recommended for Local Development)**

```bash
# Configure a named profile
aws configure --profile terraform-dev

# The provider configuration
provider "aws" {
  region  = "us-east-1"
  profile = "terraform-dev"  # References ~/.aws/credentials
}
```

**Method 3: IAM Instance Role (Recommended for AWS Workloads)**

```hcl
# When running on EC2/ECS, the provider automatically uses the instance role
provider "aws" {
  region = "us-east-1"
  # No explicit credentials needed - uses instance metadata service
}
```

### Best Practices

1. **Never hardcode credentials** in Terraform files
2. **Use IAM roles** when running on AWS infrastructure
3. **Enable MFA** for human users accessing AWS
4. **Rotate credentials** regularly (every 90 days minimum)
5. **Use separate accounts** for dev/staging/production

---

## 2. IAM Roles & Policies

### Theory: Understanding AWS IAM

**Identity and Access Management (IAM)** is AWS's security service that controls who can do what in your AWS account. It's the foundation of AWS security.

#### Core IAM Concepts

| Concept | Definition | Analogy |
|---------|------------|---------|
| **User** | Human identity with long-term credentials | Employee badge |
| **Group** | Collection of users with shared permissions | Department |
| **Role** | Identity for services/applications with temporary credentials | Job title |
| **Policy** | JSON document defining permissions | Job description |
| **Permission** | Specific action on a specific resource | "Can open door X" |

#### The Principle of Least Privilege

This is the most important security concept in IAM:

> Grant only the minimum permissions necessary to perform a task.

**Why it matters:**
- Limits blast radius of compromised credentials
- Reduces accidental damage
- Simplifies auditing
- Required for compliance (SOC2, HIPAA, etc.)

#### IAM Policy Structure

Every IAM policy follows this structure:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "UniqueStatementIdentifier",
      "Effect": "Allow | Deny",
      "Action": ["service:action", "service:action2"],
      "Resource": ["arn:aws:service:region:account:resource"],
      "Condition": {
        "StringEquals": {
          "aws:RequestTag/Environment": "Production"
        }
      }
    }
  ]
}
```

#### Users vs Roles: When to Use Each

```
┌─────────────────────────────────────────────────────────────┐
│                    IAM Identity Selection                    │
│                                                              │
│  Use IAM USER when:                                          │
│  ├── Human needs AWS Console access                          │
│  ├── Human needs programmatic access from their machine      │
│  └── Long-term credentials are acceptable                    │
│                                                              │
│  Use IAM ROLE when:                                          │
│  ├── AWS service needs permissions (EC2, Lambda, ECS)        │
│  ├── Cross-account access is needed                          │
│  ├── Federation with external identity provider              │
│  └── Temporary credentials are preferred (always!)           │
└─────────────────────────────────────────────────────────────┘
```

### Code Implementation

#### Creating an IAM User

```hcl
# iam_user.tf - Create a developer user

# The IAM user resource
resource "aws_iam_user" "developer" {
  name = "developer"
  path = "/users/"  # Organizational path for grouping users
  
  # Force destroy allows deletion even if user has non-Terraform resources
  force_destroy = true
  
  tags = {
    Role       = "Developer"
    Department = "Engineering"
  }
}

# Console login profile (for AWS Console access)
resource "aws_iam_user_login_profile" "developer" {
  user                    = aws_iam_user.developer.name
  password_length         = 20
  password_reset_required = true  # Force password change on first login
}

# Programmatic access keys (for CLI/API access)
resource "aws_iam_access_key" "developer" {
  user = aws_iam_user.developer.name
}

# Outputs (marked sensitive to prevent accidental exposure)
output "developer_access_key" {
  description = "Access key ID for the developer user"
  value       = aws_iam_access_key.developer.id
  sensitive   = true
}

output "developer_secret_key" {
  description = "Secret access key for the developer user"
  value       = aws_iam_access_key.developer.secret
  sensitive   = true
}
```

#### Creating IAM Groups

```hcl
# iam_groups.tf - Organize users into groups

# Create a developers group
resource "aws_iam_group" "developers" {
  name = "developers"
  path = "/groups/"
}

# Add users to the group
resource "aws_iam_group_membership" "developers" {
  name  = "developers-membership"
  group = aws_iam_group.developers.name
  
  users = [
    aws_iam_user.developer.name,
    # Add more users as needed
  ]
}

# Attach AWS managed policy to group
resource "aws_iam_group_policy_attachment" "developers_readonly" {
  group      = aws_iam_group.developers.name
  policy_arn = "arn:aws:iam::aws:policy/ReadOnlyAccess"
}

# Attach custom policy to group
resource "aws_iam_group_policy_attachment" "developers_s3" {
  group      = aws_iam_group.developers.name
  policy_arn = aws_iam_policy.developer_s3_access.arn
}
```

#### Creating IAM Roles (Recommended Approach)

```hcl
# iam_roles.tf - Create roles for AWS services

# IAM Role for EC2 instances
resource "aws_iam_role" "ec2_role" {
  name        = "ec2-application-role"
  description = "Role for EC2 instances to access AWS services"
  
  # Trust policy - defines WHO can assume this role
  # This is the "assume_role_policy" - it's about identity, not permissions
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Sid    = "AllowEC2AssumeRole"
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"  # Only EC2 service can assume this role
      }
    }]
  })
  
  # Maximum session duration (1-12 hours)
  max_session_duration = 3600
  
  tags = {
    Purpose = "EC2 Application Access"
  }
}

# Custom IAM Policy - defines WHAT the role can do
resource "aws_iam_policy" "s3_access" {
  name        = "s3-access-policy"
  description = "Allows read/write access to specific S3 bucket"
  path        = "/application/"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid    = "ListBucket"
        Effect = "Allow"
        Action = [
          "s3:ListBucket",
          "s3:GetBucketLocation"
        ]
        Resource = "arn:aws:s3:::my-application-bucket"
      },
      {
        Sid    = "ReadWriteObjects"
        Effect = "Allow"
        Action = [
          "s3:GetObject",
          "s3:PutObject",
          "s3:DeleteObject"
        ]
        Resource = "arn:aws:s3:::my-application-bucket/*"
      }
    ]
  })
}

# Attach policy to role
resource "aws_iam_role_policy_attachment" "ec2_s3_access" {
  role       = aws_iam_role.ec2_role.name
  policy_arn = aws_iam_policy.s3_access.arn
}

# Instance profile - required for EC2 to use IAM roles
# Think of it as a "container" that holds the role for EC2
resource "aws_iam_instance_profile" "ec2_profile" {
  name = "ec2-application-profile"
  role = aws_iam_role.ec2_role.name
}
```

#### Using the Role with EC2

```hcl
# ec2_with_role.tf - EC2 instance using the IAM role

resource "aws_instance" "app" {
  ami                  = "ami-0c55b159cbfafe1f0"
  instance_type        = "t3.micro"
  iam_instance_profile = aws_iam_instance_profile.ec2_profile.name
  
  # The instance can now access S3 without any credentials!
  # AWS SDK automatically retrieves temporary credentials from instance metadata
  
  tags = {
    Name = "app-server"
  }
}
```

### Best Practices

1. **Use roles instead of users** for applications
2. **Apply least privilege** - start with no permissions, add as needed
3. **Use groups** to manage user permissions
4. **Enable MFA** for all human users
5. **Audit regularly** with IAM Access Analyzer
6. **Use policy conditions** to add extra security layers

---

## 3. VPC Creation

### Theory: Understanding AWS VPC

A **Virtual Private Cloud (VPC)** is your isolated network in AWS. Think of it as your own private datacenter in the cloud, where you have complete control over IP addressing, subnets, routing, and security.

#### Why VPCs Matter

| Without VPC | With VPC |
|-------------|----------|
| All resources on public internet | Resources isolated in private network |
| No network segmentation | Separate subnets for different tiers |
| Limited security controls | Full control over traffic flow |
| Shared network with other AWS customers | Dedicated network space |

#### VPC Components Explained

| Component | Purpose | Key Points |
|-----------|---------|------------|
| **VPC** | The overall network container | CIDR block defines IP range |
| **Subnet** | Network segment within VPC | Tied to single Availability Zone |
| **Internet Gateway** | Connects VPC to public internet | One per VPC |
| **NAT Gateway** | Allows private subnets to reach internet | Outbound only |
| **Route Table** | Determines where traffic goes | Each subnet has one |
| **NACL** | Subnet-level firewall | Stateless, allow/deny rules |
| **Security Group** | Instance-level firewall | Stateful, allow rules only |

#### Network Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              AWS Region (us-east-1)                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                         VPC (10.0.0.0/16)                              │  │
│  │                         65,536 IP addresses                            │  │
│  │                                                                        │  │
│  │  ┌─────────────────────────────────────────────────────────────────┐  │  │
│  │  │                      PUBLIC SUBNETS                              │  │  │
│  │  │  (Resources here get public IPs and direct internet access)      │  │  │
│  │  │                                                                  │  │  │
│  │  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │  │  │
│  │  │  │ 10.0.0.0/24 │  │ 10.0.1.0/24 │  │ 10.0.2.0/24 │              │  │  │
│  │  │  │   AZ-1a     │  │   AZ-1b     │  │   AZ-1c     │              │  │  │
│  │  │  │ 256 IPs each│  │ 256 IPs each│  │ 256 IPs each│              │  │  │
│  │  │  │             │  │             │  │             │              │  │  │
│  │  │  │  [ALB]      │  │  [NAT GW]   │  │  [Bastion]  │              │  │  │
│  │  │  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘              │  │  │
│  │  └─────────┼────────────────┼────────────────┼─────────────────────┘  │  │
│  │            │                │                │                         │  │
│  │            └────────────────┼────────────────┘                         │  │
│  │                             │                                          │  │
│  │                    ┌────────┴────────┐                                 │  │
│  │                    │ Internet Gateway │◄──── Public Internet           │  │
│  │                    └─────────────────┘                                 │  │
│  │                                                                        │  │
│  │  ┌─────────────────────────────────────────────────────────────────┐  │  │
│  │  │                     PRIVATE SUBNETS                              │  │  │
│  │  │  (Resources here have NO direct internet access)                 │  │  │
│  │  │  (Outbound traffic goes through NAT Gateway)                     │  │  │
│  │  │                                                                  │  │  │
│  │  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │  │  │
│  │  │  │10.0.10.0/24 │  │10.0.11.0/24 │  │10.0.12.0/24 │              │  │  │
│  │  │  │   AZ-1a     │  │   AZ-1b     │  │   AZ-1c     │              │  │  │
│  │  │  │             │  │             │  │             │              │  │  │
│  │  │  │ [App Server]│  │ [App Server]│  │ [App Server]│              │  │  │
│  │  │  │ [Database]  │  │ [Database]  │  │ [Cache]     │              │  │  │
│  │  │  └─────────────┘  └─────────────┘  └─────────────┘              │  │  │
│  │  └─────────────────────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### CIDR Block Planning

| CIDR | IP Range | # of IPs | Use Case |
|------|----------|----------|----------|
| /16 | 10.0.0.0 - 10.0.255.255 | 65,536 | Large VPC |
| /20 | 10.0.0.0 - 10.0.15.255 | 4,096 | Medium VPC |
| /24 | 10.0.0.0 - 10.0.0.255 | 256 | Single subnet |
| /28 | 10.0.0.0 - 10.0.0.15 | 16 | Small subnet |

**Note:** AWS reserves 5 IPs per subnet (first 4 and last 1).

### Code Implementation

#### Complete VPC Setup

```hcl
# vpc.tf - Complete VPC configuration

# Main VPC
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"  # 65,536 IP addresses
  
  # Enable DNS support for internal name resolution
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name = "main-vpc"
  }
}

# Data source to get available AZs in the region
data "aws_availability_zones" "available" {
  state = "available"
  
  # Exclude Local Zones
  filter {
    name   = "opt-in-status"
    values = ["opt-in-not-required"]
  }
}

# Public Subnets - One per AZ for high availability
resource "aws_subnet" "public" {
  count = 3
  
  vpc_id = aws_vpc.main.id
  
  # cidrsubnet calculates subnet CIDR from VPC CIDR
  # cidrsubnet("10.0.0.0/16", 8, 0) = "10.0.0.0/24"
  # cidrsubnet("10.0.0.0/16", 8, 1) = "10.0.1.0/24"
  cidr_block = cidrsubnet(aws_vpc.main.cidr_block, 8, count.index)
  
  # Spread across availability zones
  availability_zone = data.aws_availability_zones.available.names[count.index]
  
  # Auto-assign public IPs to instances launched here
  map_public_ip_on_launch = true
  
  tags = {
    Name = "public-subnet-${count.index + 1}"
    Type = "Public"
    # Tags for Kubernetes if using EKS
    "kubernetes.io/role/elb" = "1"
  }
}

# Private Subnets - For application and database tiers
resource "aws_subnet" "private" {
  count = 3
  
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet(aws_vpc.main.cidr_block, 8, count.index + 10)
  availability_zone = data.aws_availability_zones.available.names[count.index]
  
  # No public IPs for private subnets
  map_public_ip_on_launch = false
  
  tags = {
    Name = "private-subnet-${count.index + 1}"
    Type = "Private"
    "kubernetes.io/role/internal-elb" = "1"
  }
}
```

#### Internet Gateway and NAT Gateway

```hcl
# gateways.tf - Internet and NAT gateways

# Internet Gateway - Allows public subnets to reach internet
resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  
  tags = {
    Name = "main-igw"
  }
}

# Elastic IPs for NAT Gateways
# Each NAT Gateway needs a static public IP
resource "aws_eip" "nat" {
  count = 3  # One per AZ for high availability
  
  domain = "vpc"  # Allocate in VPC (not EC2-Classic)
  
  tags = {
    Name = "nat-eip-${count.index + 1}"
  }
  
  # EIP may require IGW to exist
  depends_on = [aws_internet_gateway.main]
}

# NAT Gateways - Allow private subnets to reach internet (outbound only)
resource "aws_nat_gateway" "main" {
  count = 3  # One per AZ for high availability
  
  allocation_id = aws_eip.nat[count.index].id
  
  # NAT Gateway must be in a PUBLIC subnet
  subnet_id = aws_subnet.public[count.index].id
  
  tags = {
    Name = "nat-gateway-${count.index + 1}"
  }
  
  depends_on = [aws_internet_gateway.main]
}
```

#### Route Tables

```hcl
# routes.tf - Route table configuration

# Public Route Table - Routes to Internet Gateway
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
  
  # Route all non-local traffic to Internet Gateway
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }
  
  tags = {
    Name = "public-route-table"
  }
}

# Associate public subnets with public route table
resource "aws_route_table_association" "public" {
  count = 3
  
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}

# Private Route Tables - Routes to NAT Gateway
# One per AZ for fault isolation
resource "aws_route_table" "private" {
  count = 3
  
  vpc_id = aws_vpc.main.id
  
  # Route all non-local traffic to NAT Gateway in same AZ
  route {
    cidr_block     = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.main[count.index].id
  }
  
  tags = {
    Name = "private-route-table-${count.index + 1}"
  }
}

# Associate private subnets with their route tables
resource "aws_route_table_association" "private" {
  count = 3
  
  subnet_id      = aws_subnet.private[count.index].id
  route_table_id = aws_route_table.private[count.index].id
}
```

#### VPC Flow Logs (Security Best Practice)

```hcl
# flow_logs.tf - VPC Flow Logs for network monitoring

# CloudWatch Log Group for flow logs
resource "aws_cloudwatch_log_group" "flow_log" {
  name              = "/aws/vpc/flow-logs"
  retention_in_days = 7  # Adjust based on compliance requirements
  
  tags = {
    Purpose = "VPC Flow Logs"
  }
}

# IAM Role for VPC Flow Logs
resource "aws_iam_role" "flow_log" {
  name = "vpc-flow-log-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "vpc-flow-logs.amazonaws.com"
      }
    }]
  })
}

# Policy allowing flow logs to write to CloudWatch
resource "aws_iam_role_policy" "flow_log" {
  name = "vpc-flow-log-policy"
  role = aws_iam_role.flow_log.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams"
      ]
      Effect   = "Allow"
      Resource = "*"
    }]
  })
}

# VPC Flow Log
resource "aws_flow_log" "main" {
  iam_role_arn    = aws_iam_role.flow_log.arn
  log_destination = aws_cloudwatch_log_group.flow_log.arn
  traffic_type    = "ALL"  # ACCEPT, REJECT, or ALL
  vpc_id          = aws_vpc.main.id
  
  tags = {
    Name = "main-vpc-flow-log"
  }
}
```

#### VPC Outputs

```hcl
# outputs.tf - Export VPC information for other modules

output "vpc_id" {
  description = "The ID of the VPC"
  value       = aws_vpc.main.id
}

output "vpc_cidr_block" {
  description = "The CIDR block of the VPC"
  value       = aws_vpc.main.cidr_block
}

output "public_subnet_ids" {
  description = "List of public subnet IDs"
  value       = aws_subnet.public[*].id
}

output "private_subnet_ids" {
  description = "List of private subnet IDs"
  value       = aws_subnet.private[*].id
}

output "nat_gateway_ips" {
  description = "List of NAT Gateway public IPs"
  value       = aws_eip.nat[*].public_ip
}
```

### Best Practices

1. **Use multiple AZs** for high availability (minimum 2, ideally 3)
2. **Separate public and private subnets** - never put databases in public subnets
3. **Use NAT Gateways per AZ** for fault tolerance
4. **Enable VPC Flow Logs** for security monitoring
5. **Plan CIDR blocks carefully** - they cannot be changed after creation
6. **Use consistent tagging** for cost allocation

---

## 4. Security Groups

### Theory: Understanding Security Groups

**Security Groups** act as virtual firewalls for your AWS resources. They control inbound and outbound traffic at the instance level (technically, at the Elastic Network Interface level).

#### Security Groups vs NACLs

| Feature | Security Group | Network ACL |
|---------|---------------|-------------|
| **Level** | Instance/ENI | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Evaluation** | All rules evaluated | Rules evaluated in order |
| **Default** | Deny all inbound, allow all outbound | Allow all |

#### Stateful vs Stateless

```
STATEFUL (Security Groups):
┌─────────────────────────────────────────────────────────────┐
│  Request:  Client ──────────────────────────► Server        │
│            (Allowed by inbound rule)                        │
│                                                             │
│  Response: Client ◄────────────────────────── Server        │
│            (Automatically allowed - no rule needed!)        │
└─────────────────────────────────────────────────────────────┘

STATELESS (NACLs):
┌─────────────────────────────────────────────────────────────┐
│  Request:  Client ──────────────────────────► Server        │
│            (Needs inbound rule)                             │
│                                                             │
│  Response: Client ◄────────────────────────── Server        │
│            (Needs SEPARATE outbound rule!)                  │
└─────────────────────────────────────────────────────────────┘
```

#### Security Group Chaining Pattern

This is a best practice for multi-tier architectures:

```
┌─────────────────────────────────────────────────────────────┐
│                Security Group Chaining                       │
│                                                              │
│   Internet                                                   │
│      │                                                       │
│      ▼                                                       │
│  ┌────────────┐                                              │
│  │  ALB SG    │  Allows: 80, 443 from 0.0.0.0/0             │
│  └─────┬──────┘                                              │
│        │                                                     │
│        ▼                                                     │
│  ┌────────────┐                                              │
│  │  App SG    │  Allows: 8080 from ALB SG (not CIDR!)       │
│  └─────┬──────┘                                              │
│        │                                                     │
│        ▼                                                     │
│  ┌────────────┐                                              │
│  │  DB SG     │  Allows: 5432 from App SG (not CIDR!)       │
│  └────────────┘                                              │
│                                                              │
│  Benefits:                                                   │
│  - No need to update rules when IPs change                   │
│  - Self-documenting architecture                             │
│  - Automatic scaling support                                 │
└─────────────────────────────────────────────────────────────┘
```

### Code Implementation

```hcl
# security_groups.tf - Security group definitions

# ALB/Web Security Group - Public facing
resource "aws_security_group" "web" {
  name        = "web-sg"
  description = "Security group for web servers and load balancers"
  vpc_id      = aws_vpc.main.id
  
  # HTTP from anywhere
  ingress {
    description = "HTTP from internet"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # HTTPS from anywhere
  ingress {
    description = "HTTPS from internet"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # SSH from specific IP range only (office/VPN)
  ingress {
    description = "SSH from office network"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["203.0.113.0/24"]  # Replace with your IP range
  }
  
  # Allow all outbound traffic
  egress {
    description = "Allow all outbound"
    from_port   = 0
    to_port     = 0
    protocol    = "-1"  # -1 means all protocols
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = "web-sg"
    Tier = "Public"
  }
}

# Application Security Group - Only accessible from web tier
resource "aws_security_group" "app" {
  name        = "app-sg"
  description = "Security group for application servers"
  vpc_id      = aws_vpc.main.id
  
  # Application port from web tier only
  # Note: Using security_groups instead of cidr_blocks!
  ingress {
    description     = "Application traffic from web tier"
    from_port       = 8080
    to_port         = 8080
    protocol        = "tcp"
    security_groups = [aws_security_group.web.id]
  }
  
  # Health check port
  ingress {
    description     = "Health check from web tier"
    from_port       = 8081
    to_port         = 8081
    protocol        = "tcp"
    security_groups = [aws_security_group.web.id]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = "app-sg"
    Tier = "Application"
  }
}

# Database Security Group - Only accessible from app tier
resource "aws_security_group" "db" {
  name        = "db-sg"
  description = "Security group for database servers"
  vpc_id      = aws_vpc.main.id
  
  # PostgreSQL from app tier only
  ingress {
    description     = "PostgreSQL from application tier"
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [aws_security_group.app.id]
  }
  
  # MySQL alternative
  ingress {
    description     = "MySQL from application tier"
    from_port       = 3306
    to_port         = 3306
    protocol        = "tcp"
    security_groups = [aws_security_group.app.id]
  }
  
  # No egress rules needed for database
  # (Databases typically don't initiate outbound connections)
  
  tags = {
    Name = "db-sg"
    Tier = "Database"
  }
}

# Alternative: Using separate rule resources
# Useful when you have circular dependencies
resource "aws_security_group_rule" "app_from_web" {
  type                     = "ingress"
  from_port                = 8080
  to_port                  = 8080
  protocol                 = "tcp"
  source_security_group_id = aws_security_group.web.id
  security_group_id        = aws_security_group.app.id
  description              = "Allow traffic from web tier"
}
```

### Best Practices

1. **Use security group references** instead of CIDR blocks when possible
2. **Follow least privilege** - only open necessary ports
3. **Use descriptive names** and descriptions
4. **Separate by tier** - web, app, database
5. **Avoid 0.0.0.0/0** for SSH - use bastion hosts or VPN
6. **Document all rules** with descriptions

---

## 5. EC2 Instances

### Theory: Understanding EC2

**Elastic Compute Cloud (EC2)** provides resizable compute capacity in the cloud. It's the foundation of AWS compute services.

#### EC2 Instance Lifecycle

```
┌─────────────────────────────────────────────────────────────┐
│                    EC2 Instance Lifecycle                    │
│                                                              │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐  │
│  │ Pending │───►│ Running │───►│ Stopping│───►│ Stopped │  │
│  └─────────┘    └────┬────┘    └─────────┘    └────┬────┘  │
│       │              │                             │        │
│       │              │         ┌─────────────┐     │        │
│       │              └────────►│ Terminating │◄────┘        │
│       │                        └──────┬──────┘              │
│       │                               │                     │
│       │                               ▼                     │
│       │                        ┌─────────────┐              │
│       └───────────────────────►│ Terminated  │              │
│                                └─────────────┘              │
│                                                              │
│  Billing:                                                    │
│  - Running: Billed per second (minimum 60 seconds)          │
│  - Stopped: No compute charges (EBS charges still apply)    │
│  - Terminated: No charges                                    │
└─────────────────────────────────────────────────────────────┘
```

#### Instance Type Naming Convention

```
Instance Type: m5.xlarge

  m    5    .    xlarge
  │    │         │
  │    │         └── Size: nano, micro, small, medium, large, xlarge, 2xlarge...
  │    │
  │    └──────────── Generation: Higher = newer, better price/performance
  │
  └───────────────── Family:
                     t = Burstable (good for variable workloads)
                     m = General purpose (balanced)
                     c = Compute optimized (CPU intensive)
                     r = Memory optimized (RAM intensive)
                     i = Storage optimized (high I/O)
                     g/p = GPU instances
```

#### Instance Family Selection Guide

| Family | Use Case | Example Workloads |
|--------|----------|-------------------|
| **t3/t3a** | Burstable, variable load | Dev/test, small websites |
| **m5/m6i** | General purpose | Web servers, small databases |
| **c5/c6i** | Compute intensive | Batch processing, gaming |
| **r5/r6i** | Memory intensive | In-memory caching, big data |
| **i3/i4i** | Storage intensive | Data warehousing, Elasticsearch |

#### User Data: Bootstrap Scripts

User data runs once at instance launch (by default). It's used to:
- Install software
- Configure services
- Download application code
- Join clusters

### Code Implementation

#### Basic EC2 Instance

```hcl
# ec2.tf - EC2 instance configuration

# Data source: Get latest Ubuntu AMI dynamically
# This ensures you always use the latest patched image
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]  # Canonical's AWS account ID
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
  }
  
  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
  
  filter {
    name   = "architecture"
    values = ["x86_64"]
  }
}

# Key Pair for SSH access
resource "aws_key_pair" "deployer" {
  key_name   = "deployer-key"
  public_key = file("~/.ssh/id_rsa.pub")
  
  tags = {
    Name = "deployer-key"
  }
}

# EC2 Instance
resource "aws_instance" "web" {
  # AMI and instance type
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  # SSH key
  key_name = aws_key_pair.deployer.key_name
  
  # Network configuration
  subnet_id                   = aws_subnet.public[0].id
  vpc_security_group_ids      = [aws_security_group.web.id]
  associate_public_ip_address = true
  
  # IAM role for AWS API access
  iam_instance_profile = aws_iam_instance_profile.ec2_profile.name
  
  # Root volume configuration
  root_block_device {
    volume_type           = "gp3"      # General Purpose SSD v3
    volume_size           = 20         # Size in GB
    iops                  = 3000       # gp3 allows custom IOPS
    throughput            = 125        # MB/s
    delete_on_termination = true
    encrypted             = true       # Always encrypt!
  }
  
  # User data script - runs at first boot
  user_data = <<-EOF
    #!/bin/bash
    set -e  # Exit on error
    
    # Update system
    apt-get update
    apt-get upgrade -y
    
    # Install nginx
    apt-get install -y nginx
    
    # Configure nginx
    cat > /var/www/html/index.html <<HTML
    <!DOCTYPE html>
    <html>
    <head><title>Welcome</title></head>
    <body><h1>Hello from $(hostname)!</h1></body>
    </html>
    HTML
    
    # Start nginx
    systemctl start nginx
    systemctl enable nginx
  EOF
  
  # Metadata options (IMDSv2 for security)
  metadata_options {
    http_endpoint               = "enabled"
    http_tokens                 = "required"  # Require IMDSv2
    http_put_response_hop_limit = 1
  }
  
  tags = {
    Name = "web-server"
  }
}

# Output the public IP
output "instance_public_ip" {
  description = "Public IP of the web server"
  value       = aws_instance.web.public_ip
}

output "instance_public_dns" {
  description = "Public DNS of the web server"
  value       = aws_instance.web.public_dns
}
```

#### Multiple Instances with Count

```hcl
# ec2_multiple.tf - Multiple instances using count

resource "aws_instance" "app" {
  count = 3  # Create 3 instances
  
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  # Distribute across private subnets
  # count.index % 3 gives: 0, 1, 2, 0, 1, 2...
  subnet_id              = aws_subnet.private[count.index % length(aws_subnet.private)].id
  vpc_security_group_ids = [aws_security_group.app.id]
  
  iam_instance_profile = aws_iam_instance_profile.ec2_profile.name
  
  # Use templatefile for dynamic user data
  user_data = templatefile("${path.module}/scripts/user_data.sh", {
    instance_name = "app-${count.index + 1}"
    environment   = var.environment
  })
  
  root_block_device {
    volume_type = "gp3"
    volume_size = 20
    encrypted   = true
  }
  
  tags = {
    Name  = "app-server-${count.index + 1}"
    Index = count.index
  }
}

# Output all instance IDs
output "app_instance_ids" {
  description = "IDs of all app instances"
  value       = aws_instance.app[*].id
}
```

### Best Practices

1. **Use data sources for AMIs** - always get the latest patched image
2. **Enable encryption** on all EBS volumes
3. **Use IAM roles** instead of access keys on instances
4. **Require IMDSv2** for metadata security
5. **Use private subnets** for application servers
6. **Tag everything** consistently

---

## 6. Load Balancers (ALB/NLB)

### Theory: Understanding Elastic Load Balancing

**Elastic Load Balancing (ELB)** automatically distributes incoming traffic across multiple targets. It increases availability and fault tolerance.

#### Load Balancer Types

| Type | Layer | Use Case | Key Features |
|------|-------|----------|--------------|
| **ALB** | 7 (Application) | HTTP/HTTPS traffic | Path routing, host routing, WebSocket |
| **NLB** | 4 (Transport) | TCP/UDP traffic | Ultra-low latency, static IP |
| **GLB** | 3 (Gateway) | Third-party appliances | Transparent network gateway |
| **CLB** | 4 & 7 | Legacy | Being phased out |

#### ALB Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        Application Load Balancer                             │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                           LISTENERS                                    │  │
│  │                                                                        │  │
│  │   Port 80 (HTTP)                    Port 443 (HTTPS)                  │  │
│  │        │                                  │                            │  │
│  │        │ Redirect to HTTPS               │ SSL Termination            │  │
│  │        └──────────────────────────────────┤                            │  │
│  │                                           │                            │  │
│  └───────────────────────────────────────────┼────────────────────────────┘  │
│                                              │                               │
│  ┌───────────────────────────────────────────┼────────────────────────────┐  │
│  │                        LISTENER RULES     │                            │  │
│  │                                           ▼                            │  │
│  │   ┌─────────────────────────────────────────────────────────────────┐ │  │
│  │   │ Rule 1: IF path = /api/*     THEN forward to API Target Group   │ │  │
│  │   │ Rule 2: IF host = admin.*    THEN forward to Admin Target Group │ │  │
│  │   │ Rule 3: IF header X-Debug    THEN forward to Debug Target Group │ │  │
│  │   │ Default: forward to App Target Group                            │ │  │
│  │   └─────────────────────────────────────────────────────────────────┘ │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                        TARGET GROUPS                                   │  │
│  │                                                                        │  │
│  │   ┌─────────────┐  ┌─────────────┐  ┌─────────────┐                   │  │
│  │   │ App TG      │  │ API TG      │  │ Admin TG    │                   │  │
│  │   │             │  │             │  │             │                   │  │
│  │   │ ┌───┐ ┌───┐│  │ ┌───┐ ┌───┐│  │ ┌───┐      │                   │  │
│  │   │ │EC2│ │EC2││  │ │EC2│ │EC2││  │ │EC2│      │                   │  │
│  │   │ └───┘ └───┘│  │ └───┘ └───┘│  │ └───┘      │                   │  │
│  │   │             │  │             │  │             │                   │  │
│  │   │ Health: /   │  │ Health: /hc │  │ Health: /   │                   │  │
│  │   └─────────────┘  └─────────────┘  └─────────────┘                   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Health Check Configuration

Health checks determine if targets are healthy enough to receive traffic:

| Parameter | Description | Recommended Value |
|-----------|-------------|-------------------|
| **Path** | URL path to check | /health or /healthz |
| **Interval** | Time between checks | 30 seconds |
| **Timeout** | Time to wait for response | 5 seconds |
| **Healthy Threshold** | Consecutive successes needed | 2 |
| **Unhealthy Threshold** | Consecutive failures needed | 2 |
| **Matcher** | Expected HTTP status codes | 200 or 200-299 |

### Code Implementation

#### Application Load Balancer

```hcl
# alb.tf - Application Load Balancer configuration

# ALB Security Group
resource "aws_security_group" "alb" {
  name        = "alb-sg"
  description = "Security group for Application Load Balancer"
  vpc_id      = aws_vpc.main.id
  
  ingress {
    description = "HTTP from internet"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    description = "HTTPS from internet"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = "alb-sg"
  }
}

# Application Load Balancer
resource "aws_lb" "main" {
  name               = "main-alb"
  internal           = false  # Internet-facing
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb.id]
  subnets            = aws_subnet.public[*].id
  
  # Deletion protection (enable in production)
  enable_deletion_protection = false
  
  # Enable HTTP/2
  enable_http2 = true
  
  # Access logs (recommended for production)
  # access_logs {
  #   bucket  = aws_s3_bucket.alb_logs.id
  #   prefix  = "alb"
  #   enabled = true
  # }
  
  tags = {
    Name = "main-alb"
  }
}

# Target Group
resource "aws_lb_target_group" "app" {
  name     = "app-tg"
  port     = 80
  protocol = "HTTP"
  vpc_id   = aws_vpc.main.id
  
  # Target type: instance, ip, or lambda
  target_type = "instance"
  
  # Health check configuration
  health_check {
    enabled             = true
    healthy_threshold   = 2
    unhealthy_threshold = 2
    timeout             = 5
    interval            = 30
    path                = "/health"
    port                = "traffic-port"
    protocol            = "HTTP"
    matcher             = "200"
  }
  
  # Stickiness (session affinity)
  stickiness {
    type            = "lb_cookie"
    cookie_duration = 86400  # 1 day
    enabled         = false  # Enable if needed
  }
  
  # Deregistration delay
  deregistration_delay = 300  # 5 minutes
  
  tags = {
    Name = "app-target-group"
  }
}

# Register targets (EC2 instances)
resource "aws_lb_target_group_attachment" "app" {
  count = length(aws_instance.app)
  
  target_group_arn = aws_lb_target_group.app.arn
  target_id        = aws_instance.app[count.index].id
  port             = 80
}

# HTTP Listener (redirect to HTTPS)
resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.main.arn
  port              = 80
  protocol          = "HTTP"
  
  default_action {
    type = "redirect"
    
    redirect {
      port        = "443"
      protocol    = "HTTPS"
      status_code = "HTTP_301"
    }
  }
}

# HTTPS Listener
resource "aws_lb_listener" "https" {
  load_balancer_arn = aws_lb.main.arn
  port              = 443
  protocol          = "HTTPS"
  ssl_policy        = "ELBSecurityPolicy-TLS13-1-2-2021-06"
  certificate_arn   = aws_acm_certificate.main.arn
  
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.app.arn
  }
}

# Listener Rule for API routing
resource "aws_lb_listener_rule" "api" {
  listener_arn = aws_lb_listener.https.arn
  priority     = 100
  
  action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.api.arn
  }
  
  condition {
    path_pattern {
      values = ["/api/*"]
    }
  }
}

# Output ALB DNS name
output "alb_dns_name" {
  description = "DNS name of the load balancer"
  value       = aws_lb.main.dns_name
}
```

#### Network Load Balancer

```hcl
# nlb.tf - Network Load Balancer for TCP/UDP traffic

resource "aws_lb" "network" {
  name               = "main-nlb"
  internal           = false
  load_balancer_type = "network"
  subnets            = aws_subnet.public[*].id
  
  # NLB doesn't use security groups
  # It passes through client IP to targets
  
  enable_deletion_protection       = false
  enable_cross_zone_load_balancing = true
  
  tags = {
    Name = "main-nlb"
  }
}

resource "aws_lb_target_group" "tcp" {
  name     = "tcp-tg"
  port     = 8080
  protocol = "TCP"
  vpc_id   = aws_vpc.main.id
  
  health_check {
    enabled  = true
    protocol = "TCP"
    port     = 8080
    interval = 30
  }
}

resource "aws_lb_listener" "tcp" {
  load_balancer_arn = aws_lb.network.arn
  port              = 80
  protocol          = "TCP"
  
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.tcp.arn
  }
}
```

### Best Practices

1. **Use HTTPS** - always terminate SSL at the load balancer
2. **Configure health checks** properly - they determine traffic routing
3. **Enable access logs** for troubleshooting and compliance
4. **Use connection draining** - allow in-flight requests to complete
5. **Consider cross-zone load balancing** for even distribution

---

## 7. Auto Scaling Groups

### Theory: Understanding Auto Scaling

**Auto Scaling** automatically adjusts the number of EC2 instances based on demand. It ensures you have the right amount of compute capacity at all times.

#### Auto Scaling Components

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         Auto Scaling Architecture                            │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                      LAUNCH TEMPLATE                                   │  │
│  │  (Blueprint for instances)                                            │  │
│  │                                                                        │  │
│  │  • AMI ID                    • Security Groups                        │  │
│  │  • Instance Type             • IAM Role                               │  │
│  │  • Key Pair                  • User Data                              │  │
│  │  • EBS Configuration         • Network Settings                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                    │                                         │
│                                    ▼                                         │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                    AUTO SCALING GROUP                                  │  │
│  │  (Manages the fleet)                                                  │  │
│  │                                                                        │  │
│  │  Capacity Settings:                                                   │  │
│  │  • Minimum: 2    (never go below this)                                │  │
│  │  • Maximum: 10   (never exceed this)                                  │  │
│  │  • Desired: 4    (target capacity)                                    │  │
│  │                                                                        │  │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐                     │  │
│  │  │ EC2-1   │ │ EC2-2   │ │ EC2-3   │ │ EC2-4   │                     │  │
│  │  │  AZ-a   │ │  AZ-b   │ │  AZ-c   │ │  AZ-a   │                     │  │
│  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘                     │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                    │                                         │
│                                    ▼                                         │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                      SCALING POLICIES                                  │  │
│  │  (When and how to scale)                                              │  │
│  │                                                                        │  │
│  │  Target Tracking:  "Keep CPU at 70%"                                  │  │
│  │  Step Scaling:     "If CPU > 80%, add 2 instances"                    │  │
│  │  Scheduled:        "Scale to 10 at 9 AM on weekdays"                  │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Scaling Policy Types

| Policy Type | Description | Use Case |
|-------------|-------------|----------|
| **Target Tracking** | Maintain metric at target value | Keep CPU at 70% |
| **Step Scaling** | Scale in steps based on alarm severity | Add 1 if CPU > 70%, add 3 if > 90% |
| **Simple Scaling** | Single scaling adjustment | Add 1 instance (legacy) |
| **Scheduled** | Scale at specific times | Scale up before traffic spike |
| **Predictive** | ML-based prediction | Recurring traffic patterns |

### Code Implementation

```hcl
# autoscaling.tf - Auto Scaling Group configuration

# Launch Template (replaces Launch Configuration)
resource "aws_launch_template" "app" {
  name_prefix   = "app-"
  description   = "Launch template for application servers"
  
  image_id      = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  # Network configuration
  vpc_security_group_ids = [aws_security_group.app.id]
  
  # IAM role
  iam_instance_profile {
    name = aws_iam_instance_profile.ec2_profile.name
  }
  
  # EBS configuration
  block_device_mappings {
    device_name = "/dev/sda1"
    
    ebs {
      volume_size           = 20
      volume_type           = "gp3"
      delete_on_termination = true
      encrypted             = true
    }
  }
  
  # User data (base64 encoded)
  user_data = base64encode(templatefile("${path.module}/scripts/user_data.sh", {
    environment = var.environment
  }))
  
  # Metadata options
  metadata_options {
    http_endpoint               = "enabled"
    http_tokens                 = "required"
    http_put_response_hop_limit = 1
  }
  
  # Instance tags
  tag_specifications {
    resource_type = "instance"
    tags = {
      Name = "app-server"
    }
  }
  
  tag_specifications {
    resource_type = "volume"
    tags = {
      Name = "app-server-volume"
    }
  }
  
  # Enable detailed monitoring
  monitoring {
    enabled = true
  }
  
  lifecycle {
    create_before_destroy = true
  }
}

# Auto Scaling Group
resource "aws_autoscaling_group" "app" {
  name                = "app-asg"
  vpc_zone_identifier = aws_subnet.private[*].id
  
  # Capacity settings
  min_size         = 2   # Minimum instances (for HA)
  max_size         = 10  # Maximum instances (cost control)
  desired_capacity = 3   # Starting capacity
  
  # Launch template
  launch_template {
    id      = aws_launch_template.app.id
    version = "$Latest"  # Or specific version number
  }
  
  # Load balancer integration
  target_group_arns = [aws_lb_target_group.app.arn]
  
  # Health check configuration
  health_check_type         = "ELB"  # Use ALB health checks
  health_check_grace_period = 300    # Wait 5 min before checking
  
  # Instance refresh (for rolling updates)
  instance_refresh {
    strategy = "Rolling"
    preferences {
      min_healthy_percentage = 50
    }
  }
  
  # Termination policies
  termination_policies = ["OldestInstance", "Default"]
  
  # Tags propagated to instances
  tag {
    key                 = "Name"
    value               = "app-server"
    propagate_at_launch = true
  }
  
  tag {
    key                 = "Environment"
    value               = var.environment
    propagate_at_launch = true
  }
  
  lifecycle {
    create_before_destroy = true
  }
}

# Target Tracking Scaling Policy
resource "aws_autoscaling_policy" "cpu_target" {
  name                   = "cpu-target-tracking"
  autoscaling_group_name = aws_autoscaling_group.app.name
  policy_type            = "TargetTrackingScaling"
  
  target_tracking_configuration {
    predefined_metric_specification {
      predefined_metric_type = "ASGAverageCPUUtilization"
    }
    target_value = 70.0
    
    # Disable scale-in to prevent thrashing (optional)
    # disable_scale_in = true
  }
}

# Step Scaling Policy (for more control)
resource "aws_autoscaling_policy" "scale_up" {
  name                   = "scale-up"
  autoscaling_group_name = aws_autoscaling_group.app.name
  adjustment_type        = "ChangeInCapacity"
  policy_type            = "StepScaling"
  
  step_adjustment {
    scaling_adjustment          = 1
    metric_interval_lower_bound = 0
    metric_interval_upper_bound = 10
  }
  
  step_adjustment {
    scaling_adjustment          = 2
    metric_interval_lower_bound = 10
  }
}

# CloudWatch Alarm to trigger scaling
resource "aws_cloudwatch_metric_alarm" "high_cpu" {
  alarm_name          = "high-cpu-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 120
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "Scale up when CPU exceeds 80%"
  
  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.app.name
  }
  
  alarm_actions = [aws_autoscaling_policy.scale_up.arn]
}
```

### Best Practices

1. **Use Launch Templates** instead of Launch Configurations (more features)
2. **Set appropriate min/max** - min for HA, max for cost control
3. **Use ELB health checks** for better accuracy
4. **Configure health check grace period** to avoid premature termination
5. **Use target tracking** for simple, effective scaling
6. **Test scaling policies** before production

---

## 8. RDS (Relational Database Service)

### Theory: Understanding RDS

**Amazon RDS** is a managed relational database service that handles routine database tasks like provisioning, patching, backup, recovery, and scaling.

#### RDS vs Self-Managed Database

| Aspect | RDS (Managed) | Self-Managed on EC2 |
|--------|---------------|---------------------|
| **Patching** | Automatic | Manual |
| **Backups** | Automatic, point-in-time | Manual setup |
| **High Availability** | Multi-AZ with one click | Complex setup |
| **Scaling** | Easy vertical/horizontal | Manual |
| **Monitoring** | Built-in CloudWatch | Manual setup |
| **Cost** | Higher per-hour | Lower but more ops work |

#### RDS Architecture Options

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        RDS Deployment Options                                │
│                                                                              │
│  SINGLE-AZ (Development/Test)                                               │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  AZ-a                                                                │    │
│  │  ┌─────────────┐                                                    │    │
│  │  │   Primary   │                                                    │    │
│  │  │   Database  │                                                    │    │
│  │  └─────────────┘                                                    │    │
│  │  Single point of failure!                                           │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  MULTI-AZ (Production - High Availability)                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  AZ-a                           AZ-b                                │    │
│  │  ┌─────────────┐                ┌─────────────┐                     │    │
│  │  │   Primary   │  ═══════════►  │   Standby   │                     │    │
│  │  │   Database  │  Synchronous   │   Database  │                     │    │
│  │  │  (Active)   │  Replication   │  (Passive)  │                     │    │
│  │  └─────────────┘                └─────────────┘                     │    │
│  │  Automatic failover in ~60 seconds                                  │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  READ REPLICAS (Read Scaling)                                               │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  AZ-a              AZ-b              AZ-c                           │    │
│  │  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐                │    │
│  │  │   Primary   │──►│ Read Replica│──►│ Read Replica│                │    │
│  │  │  (R/W)      │   │  (R only)   │   │  (R only)   │                │    │
│  │  └─────────────┘   └─────────────┘   └─────────────┘                │    │
│  │  Asynchronous replication (slight lag)                              │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Supported Database Engines

| Engine | Use Case | Notes |
|--------|----------|-------|
| **PostgreSQL** | General purpose, GIS | Open source, feature-rich |
| **MySQL** | Web applications | Most popular open source |
| **MariaDB** | MySQL alternative | Community-driven |
| **Oracle** | Enterprise applications | License required |
| **SQL Server** | Windows/.NET apps | License included or BYOL |
| **Aurora** | High performance | AWS-native, MySQL/PostgreSQL compatible |

### Code Implementation

```hcl
# rds.tf - RDS PostgreSQL configuration

# DB Subnet Group - defines which subnets RDS can use
resource "aws_db_subnet_group" "main" {
  name        = "main-db-subnet-group"
  description = "Database subnet group for main VPC"
  subnet_ids  = aws_subnet.private[*].id
  
  tags = {
    Name = "main-db-subnet-group"
  }
}

# DB Parameter Group - database configuration settings
resource "aws_db_parameter_group" "postgres" {
  name        = "postgres-params"
  family      = "postgres14"
  description = "Custom parameter group for PostgreSQL 14"
  
  # Enable connection logging
  parameter {
    name  = "log_connections"
    value = "1"
  }
  
  parameter {
    name  = "log_disconnections"
    value = "1"
  }
  
  # Performance settings
  parameter {
    name  = "shared_preload_libraries"
    value = "pg_stat_statements"
  }
  
  tags = {
    Name = "postgres-params"
  }
}

# Generate secure random password
resource "random_password" "db_password" {
  length           = 16
  special          = true
  override_special = "!#$%&*()-_=+[]{}<>:?"
}

# RDS Instance
resource "aws_db_instance" "main" {
  identifier = "main-db"
  
  # Engine configuration
  engine               = "postgres"
  engine_version       = "14.7"
  instance_class       = "db.t3.micro"
  
  # Storage configuration
  allocated_storage     = 20
  max_allocated_storage = 100  # Enable storage autoscaling
  storage_type          = "gp3"
  storage_encrypted     = true
  
  # Database configuration
  db_name  = "myapp"
  username = "admin"
  password = random_password.db_password.result
  port     = 5432
  
  # Network configuration
  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [aws_security_group.db.id]
  publicly_accessible    = false  # Never expose DB to internet!
  
  # Parameter and option groups
  parameter_group_name = aws_db_parameter_group.postgres.name
  
  # High availability
  multi_az = true  # Enable for production
  
  # Backup configuration
  backup_retention_period = 7
  backup_window           = "03:00-04:00"  # UTC
  
  # Maintenance window
  maintenance_window = "mon:04:00-mon:05:00"  # UTC
  
  # Monitoring
  enabled_cloudwatch_logs_exports = ["postgresql", "upgrade"]
  performance_insights_enabled    = true
  
  # Deletion protection
  deletion_protection       = false  # Enable in production!
  skip_final_snapshot       = false
  final_snapshot_identifier = "main-db-final-${formatdate("YYYY-MM-DD-hhmm", timestamp())}"
  
  tags = {
    Name = "main-db"
  }
}

# Store credentials in Secrets Manager
resource "aws_secretsmanager_secret" "db_credentials" {
  name        = "main-db-credentials"
  description = "Database credentials for main RDS instance"
  
  tags = {
    Name = "main-db-credentials"
  }
}

resource "aws_secretsmanager_secret_version" "db_credentials" {
  secret_id = aws_secretsmanager_secret.db_credentials.id
  
  secret_string = jsonencode({
    username = aws_db_instance.main.username
    password = random_password.db_password.result
    engine   = "postgres"
    host     = aws_db_instance.main.endpoint
    port     = aws_db_instance.main.port
    dbname   = aws_db_instance.main.db_name
  })
}

# Outputs
output "db_endpoint" {
  description = "RDS instance endpoint"
  value       = aws_db_instance.main.endpoint
}

output "db_secret_arn" {
  description = "ARN of the database credentials secret"
  value       = aws_secretsmanager_secret.db_credentials.arn
}
```

### Best Practices

1. **Enable Multi-AZ** for production workloads
2. **Use Secrets Manager** for credential management
3. **Enable encryption** at rest and in transit
4. **Configure automated backups** with appropriate retention
5. **Use parameter groups** for custom configurations
6. **Monitor with Performance Insights** for query optimization

---

## 9. S3 Buckets

### Theory: Understanding S3

**Amazon S3 (Simple Storage Service)** is object storage with virtually unlimited scalability. It's designed for 99.999999999% (11 9's) durability.

#### S3 Concepts

| Concept | Description |
|---------|-------------|
| **Bucket** | Container for objects (like a folder) |
| **Object** | File + metadata (up to 5TB) |
| **Key** | Unique identifier for object within bucket |
| **Version ID** | Identifier for object version (if versioning enabled) |

#### S3 Storage Classes

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         S3 Storage Classes                                   │
│                                                                              │
│  HOT DATA (Frequently Accessed)                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  S3 Standard                                                        │    │
│  │  • 99.99% availability                                              │    │
│  │  • Millisecond access                                               │    │
│  │  • Highest cost per GB                                              │    │
│  │  • Use for: Active data, frequently accessed files                  │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                    │                                         │
│                                    ▼ (After 30 days)                         │
│  WARM DATA (Infrequently Accessed)                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  S3 Standard-IA (Infrequent Access)                                 │    │
│  │  • 99.9% availability                                               │    │
│  │  • Millisecond access                                               │    │
│  │  • Lower storage cost, retrieval fee                                │    │
│  │  • Use for: Backups, disaster recovery, older data                  │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                    │                                         │
│                                    ▼ (After 90 days)                         │
│  COLD DATA (Archive)                                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  S3 Glacier Instant Retrieval                                       │    │
│  │  • Millisecond access                                               │    │
│  │  • 68% cheaper than Standard-IA                                     │    │
│  │                                                                      │    │
│  │  S3 Glacier Flexible Retrieval                                      │    │
│  │  • Minutes to hours retrieval                                       │    │
│  │  • Very low storage cost                                            │    │
│  │                                                                      │    │
│  │  S3 Glacier Deep Archive                                            │    │
│  │  • 12-48 hours retrieval                                            │    │
│  │  • Lowest cost (compliance archives)                                │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### S3 Security Layers

| Layer | Purpose | Scope |
|-------|---------|-------|
| **IAM Policies** | User/role permissions | AWS account level |
| **Bucket Policies** | Bucket-level access control | Bucket level |
| **ACLs** | Legacy object-level control | Object level |
| **Block Public Access** | Prevent accidental exposure | Account/bucket level |
| **Encryption** | Data protection | Object level |

### Code Implementation

```hcl
# s3.tf - S3 bucket with security best practices

# Random suffix for globally unique bucket name
resource "random_id" "bucket_suffix" {
  byte_length = 4
}

# S3 Bucket
resource "aws_s3_bucket" "data" {
  bucket = "my-app-data-${random_id.bucket_suffix.hex}"
  
  # Prevent accidental deletion
  force_destroy = false
  
  tags = {
    Name        = "data-bucket"
    Environment = var.environment
  }
}

# Enable versioning
resource "aws_s3_bucket_versioning" "data" {
  bucket = aws_s3_bucket.data.id
  
  versioning_configuration {
    status = "Enabled"
  }
}

# Enable server-side encryption
resource "aws_s3_bucket_server_side_encryption_configuration" "data" {
  bucket = aws_s3_bucket.data.id
  
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"  # Or "aws:kms" for KMS
    }
    bucket_key_enabled = true  # Reduces KMS costs
  }
}

# Block all public access (CRITICAL!)
resource "aws_s3_bucket_public_access_block" "data" {
  bucket = aws_s3_bucket.data.id
  
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

# Lifecycle policy for cost optimization
resource "aws_s3_bucket_lifecycle_configuration" "data" {
  bucket = aws_s3_bucket.data.id
  
  # Rule 1: Transition old objects to cheaper storage
  rule {
    id     = "archive-old-objects"
    status = "Enabled"
    
    filter {
      prefix = ""  # Apply to all objects
    }
    
    # Move to Standard-IA after 30 days
    transition {
      days          = 30
      storage_class = "STANDARD_IA"
    }
    
    # Move to Glacier after 90 days
    transition {
      days          = 90
      storage_class = "GLACIER"
    }
    
    # Delete after 365 days
    expiration {
      days = 365
    }
  }
  
  # Rule 2: Clean up old versions
  rule {
    id     = "delete-old-versions"
    status = "Enabled"
    
    filter {
      prefix = ""
    }
    
    noncurrent_version_expiration {
      noncurrent_days = 90
    }
    
    # Clean up incomplete multipart uploads
    abort_incomplete_multipart_upload {
      days_after_initiation = 7
    }
  }
  
  # Rule 3: Special handling for logs
  rule {
    id     = "logs-lifecycle"
    status = "Enabled"
    
    filter {
      prefix = "logs/"
    }
    
    expiration {
      days = 30
    }
  }
}

# Bucket policy for EC2 access
resource "aws_s3_bucket_policy" "data" {
  bucket = aws_s3_bucket.data.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid    = "AllowEC2RoleAccess"
        Effect = "Allow"
        Principal = {
          AWS = aws_iam_role.ec2_role.arn
        }
        Action = [
          "s3:GetObject",
          "s3:PutObject",
          "s3:DeleteObject"
        ]
        Resource = "${aws_s3_bucket.data.arn}/*"
      },
      {
        Sid    = "AllowEC2ListBucket"
        Effect = "Allow"
        Principal = {
          AWS = aws_iam_role.ec2_role.arn
        }
        Action   = "s3:ListBucket"
        Resource = aws_s3_bucket.data.arn
      },
      {
        Sid    = "DenyInsecureTransport"
        Effect = "Deny"
        Principal = "*"
        Action   = "s3:*"
        Resource = [
          aws_s3_bucket.data.arn,
          "${aws_s3_bucket.data.arn}/*"
        ]
        Condition = {
          Bool = {
            "aws:SecureTransport" = "false"
          }
        }
      }
    ]
  })
}

# Outputs
output "bucket_name" {
  description = "Name of the S3 bucket"
  value       = aws_s3_bucket.data.id
}

output "bucket_arn" {
  description = "ARN of the S3 bucket"
  value       = aws_s3_bucket.data.arn
}
```

### Best Practices

1. **Block public access** by default
2. **Enable versioning** for important data
3. **Enable encryption** (SSE-S3 or SSE-KMS)
4. **Use lifecycle policies** for cost optimization
5. **Require HTTPS** via bucket policy
6. **Enable access logging** for audit trails

---

## 10. CloudWatch

### Theory: Understanding CloudWatch

**Amazon CloudWatch** is AWS's monitoring and observability service. It collects and tracks metrics, collects and monitors log files, and sets alarms.

#### CloudWatch Components

| Component | Purpose | Example |
|-----------|---------|---------|
| **Metrics** | Numerical data points over time | CPU utilization, request count |
| **Logs** | Text-based log data | Application logs, access logs |
| **Alarms** | Automated responses to metrics | Alert when CPU > 80% |
| **Dashboards** | Visual representations | Ops dashboard |
| **Events/EventBridge** | React to AWS events | Instance terminated |

#### Metric Dimensions

Metrics are identified by:
- **Namespace**: Category (e.g., AWS/EC2, AWS/RDS)
- **Metric Name**: What's being measured (e.g., CPUUtilization)
- **Dimensions**: Key-value pairs to filter (e.g., InstanceId=i-123)

#### Common AWS Metrics

| Service | Metric | Description |
|---------|--------|-------------|
| **EC2** | CPUUtilization | CPU usage percentage |
| **EC2** | NetworkIn/Out | Network traffic bytes |
| **RDS** | DatabaseConnections | Active DB connections |
| **RDS** | FreeStorageSpace | Available storage |
| **ALB** | RequestCount | Number of requests |
| **ALB** | TargetResponseTime | Response latency |
| **S3** | BucketSizeBytes | Total bucket size |

### Code Implementation

```hcl
# cloudwatch.tf - CloudWatch monitoring configuration

# CloudWatch Log Group for application logs
resource "aws_cloudwatch_log_group" "app" {
  name              = "/aws/app/logs"
  retention_in_days = 7  # Adjust based on requirements
  
  tags = {
    Name        = "app-logs"
    Application = "my-app"
  }
}

# CloudWatch Log Group for VPC Flow Logs
resource "aws_cloudwatch_log_group" "vpc_flow" {
  name              = "/aws/vpc/flow-logs"
  retention_in_days = 14
  
  tags = {
    Name = "vpc-flow-logs"
  }
}

# SNS Topic for alerts
resource "aws_sns_topic" "alerts" {
  name         = "cloudwatch-alerts"
  display_name = "CloudWatch Alerts"
  
  tags = {
    Name = "cloudwatch-alerts"
  }
}

# SNS Email subscription
resource "aws_sns_topic_subscription" "email" {
  topic_arn = aws_sns_topic.alerts.arn
  protocol  = "email"
  endpoint  = var.alert_email  # e.g., "admin@example.com"
}

# High CPU Alarm for EC2
resource "aws_cloudwatch_metric_alarm" "ec2_high_cpu" {
  alarm_name          = "ec2-high-cpu"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300  # 5 minutes
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "Triggers when EC2 CPU exceeds 80% for 10 minutes"
  
  dimensions = {
    InstanceId = aws_instance.web.id
  }
  
  alarm_actions = [aws_sns_topic.alerts.arn]
  ok_actions    = [aws_sns_topic.alerts.arn]
  
  tags = {
    Name = "ec2-high-cpu-alarm"
  }
}

# High CPU Alarm for ASG
resource "aws_cloudwatch_metric_alarm" "asg_high_cpu" {
  alarm_name          = "asg-high-cpu"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 120
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "Triggers when ASG average CPU exceeds 80%"
  
  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.app.name
  }
  
  alarm_actions = [
    aws_sns_topic.alerts.arn,
    aws_autoscaling_policy.scale_up.arn
  ]
  
  tags = {
    Name = "asg-high-cpu-alarm"
  }
}

# RDS Connection Alarm
resource "aws_cloudwatch_metric_alarm" "rds_connections" {
  alarm_name          = "rds-high-connections"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "DatabaseConnections"
  namespace           = "AWS/RDS"
  period              = 300
  statistic           = "Average"
  threshold           = 50
  alarm_description   = "Triggers when RDS connections exceed 50"
  
  dimensions = {
    DBInstanceIdentifier = aws_db_instance.main.identifier
  }
  
  alarm_actions = [aws_sns_topic.alerts.arn]
  
  tags = {
    Name = "rds-connections-alarm"
  }
}

# RDS Storage Alarm
resource "aws_cloudwatch_metric_alarm" "rds_storage" {
  alarm_name          = "rds-low-storage"
  comparison_operator = "LessThanThreshold"
  evaluation_periods  = 1
  metric_name         = "FreeStorageSpace"
  namespace           = "AWS/RDS"
  period              = 300
  statistic           = "Average"
  threshold           = 5368709120  # 5 GB in bytes
  alarm_description   = "Triggers when RDS free storage falls below 5GB"
  
  dimensions = {
    DBInstanceIdentifier = aws_db_instance.main.identifier
  }
  
  alarm_actions = [aws_sns_topic.alerts.arn]
  
  tags = {
    Name = "rds-storage-alarm"
  }
}

# ALB 5xx Error Alarm
resource "aws_cloudwatch_metric_alarm" "alb_5xx" {
  alarm_name          = "alb-5xx-errors"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "HTTPCode_ELB_5XX_Count"
  namespace           = "AWS/ApplicationELB"
  period              = 300
  statistic           = "Sum"
  threshold           = 10
  alarm_description   = "Triggers when ALB returns more than 10 5xx errors"
  
  dimensions = {
    LoadBalancer = aws_lb.main.arn_suffix
  }
  
  alarm_actions = [aws_sns_topic.alerts.arn]
  
  # Handle missing data (no errors = good)
  treat_missing_data = "notBreaching"
  
  tags = {
    Name = "alb-5xx-alarm"
  }
}

# CloudWatch Dashboard
resource "aws_cloudwatch_dashboard" "main" {
  dashboard_name = "main-dashboard"
  
  dashboard_body = jsonencode({
    widgets = [
      {
        type   = "metric"
        x      = 0
        y      = 0
        width  = 12
        height = 6
        properties = {
          title  = "EC2 CPU Utilization"
          region = var.aws_region
          metrics = [
            ["AWS/EC2", "CPUUtilization", "AutoScalingGroupName", aws_autoscaling_group.app.name, { stat = "Average" }]
          ]
          period = 300
        }
      },
      {
        type   = "metric"
        x      = 12
        y      = 0
        width  = 12
        height = 6
        properties = {
          title  = "ALB Request Count"
          region = var.aws_region
          metrics = [
            ["AWS/ApplicationELB", "RequestCount", "LoadBalancer", aws_lb.main.arn_suffix, { stat = "Sum" }]
          ]
          period = 300
        }
      },
      {
        type   = "metric"
        x      = 0
        y      = 6
        width  = 12
        height = 6
        properties = {
          title  = "RDS Connections"
          region = var.aws_region
          metrics = [
            ["AWS/RDS", "DatabaseConnections", "DBInstanceIdentifier", aws_db_instance.main.identifier, { stat = "Average" }]
          ]
          period = 300
        }
      },
      {
        type   = "metric"
        x      = 12
        y      = 6
        width  = 12
        height = 6
        properties = {
          title  = "ALB Response Time"
          region = var.aws_region
          metrics = [
            ["AWS/ApplicationELB", "TargetResponseTime", "LoadBalancer", aws_lb.main.arn_suffix, { stat = "Average" }]
          ]
          period = 300
        }
      }
    ]
  })
}
```

### Best Practices

1. **Set appropriate thresholds** - avoid alert fatigue
2. **Use multiple evaluation periods** - prevent false positives
3. **Configure OK actions** - know when issues resolve
4. **Create dashboards** - visualize system health
5. **Set log retention** - balance cost and compliance
6. **Use metric math** for complex calculations

---

## Summary: Best Practices Checklist

### Security
- [ ] Use IAM roles instead of access keys
- [ ] Enable encryption everywhere (EBS, S3, RDS)
- [ ] Block S3 public access by default
- [ ] Use security group chaining
- [ ] Store secrets in Secrets Manager
- [ ] Enable VPC Flow Logs

### High Availability
- [ ] Use multiple Availability Zones
- [ ] Enable Multi-AZ for RDS
- [ ] Use Auto Scaling Groups
- [ ] Configure health checks properly
- [ ] Use NAT Gateway per AZ

### Cost Optimization
- [ ] Right-size instances
- [ ] Use S3 lifecycle policies
- [ ] Enable RDS storage autoscaling
- [ ] Set ASG maximum limits
- [ ] Use Spot Instances where appropriate

### Operations
- [ ] Tag all resources consistently
- [ ] Enable CloudWatch monitoring
- [ ] Set up alerting
- [ ] Configure automated backups
- [ ] Document everything in Terraform

---

## Next Steps

After completing this phase, you should:

1. **Practice** by building a complete 3-tier architecture
2. **Explore** advanced services (ECS, EKS, Lambda)
3. **Learn** about AWS Organizations and multi-account strategies
4. **Study** for AWS certifications if interested

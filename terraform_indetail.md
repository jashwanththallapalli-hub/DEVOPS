# Complete Terraform Learning Guide

A comprehensive guide covering all Terraform concepts from basics to advanced, with practical HCL code examples.

## 📚 Table of Contents

### 🟢 [Phase 1: Terraform Basics (Foundation)](./phase-01-basics/README.md)
- Infrastructure as Code (IaC)
- Terraform vs Other Tools
- Terraform Architecture
- Installation & CLI Basics
- HCL Syntax
- Core Workflow

### 🟡 [Phase 2: Variables & Outputs](./phase-02-variables-outputs/README.md)
- Input Variables
- Variable Types
- Output Values
- Locals

### 🟠 [Phase 3: Providers & Resources](./phase-03-providers-resources/README.md)
- Provider Configuration
- Resource Meta-Arguments
- Data Sources

### 🔵 [Phase 4: State Management](./phase-04-state-management/README.md)
- State Files
- Remote Backends
- State Commands
- State Security

### 🟣 [Phase 5: Expressions & Functions](./phase-05-expressions-functions/README.md)
- Terraform Expressions
- Built-in Functions
- Dynamic Blocks
- for_each vs count

### 🟤 [Phase 6: Modules](./phase-06-modules/README.md)
- Module Basics
- Module Structure
- Reusable Modules
- Module Best Practices

### ⚫ [Phase 7: Workspaces & Environments](./phase-07-workspaces/README.md)
- Terraform Workspaces
- Environment Management
- Variable Files

### 🔴 [Phase 8: Provisioners & Templates](./phase-08-provisioners/README.md)
- Provisioners
- Template Files
- User Data

### 🟧 [Phase 9: Terraform with AWS](./phase-09-aws/README.md)
- AWS Provider
- VPC, EC2, RDS
- Load Balancers
- EKS

### 🟦 [Phase 10: Terraform with Kubernetes](./phase-10-kubernetes/README.md)
- Kubernetes Provider
- Deployments & Services
- Helm Integration

### 🟩 [Phase 11: CI/CD & Automation](./phase-11-cicd/README.md)
- Terraform in CI/CD
- GitHub Actions
- Jenkins Integration
- Terraform Cloud

### 🟨 [Phase 12: Security & Best Practices](./phase-12-security/README.md)
- Security Best Practices
- Secrets Management
- Code Quality


### 🟪 [Phase 13: Interview Preparation](./phase-14-interview/README.md)
- Common Interview Questions
- Debugging
- Production Checklist

### 🏁 [Phase 14: Capstone Projects](./phase-15-capstone/README.md)
- Real-world Projects
- Complete Implementations

## 🚀 How to Use This Guide

1. Start with Phase 1 and progress sequentially
2. Practice each code example in your own environment
3. Complete the capstone projects to solidify your knowledge
4. Use this as a reference for interviews and production work

## 📝 Prerequisites

- Basic understanding of cloud computing
- Command line familiarity
- AWS/Azure/GCP account (for practice)

## 🤝 Contributing

Feel free to contribute improvements, corrections, or additional examples!

This guide is for educational purposes.


# Phase 1: Terraform Basics (Foundation)

## 1. What is Infrastructure as Code (IaC)?

**Infrastructure as Code (IaC)** is the practice of managing and provisioning infrastructure through machine-readable definition files, rather than manual processes or interactive configuration tools.

### Benefits of IaC:
- **Version Control**: Track changes to infrastructure over time
- **Consistency**: Same configuration produces same results
- **Automation**: Reduce manual errors
- **Reusability**: Share and reuse infrastructure patterns
- **Documentation**: Code serves as documentation
- **Speed**: Rapidly provision and tear down environments

### Traditional vs IaC Approach:

**Traditional (Manual)**:
```
1. Log into AWS Console
2. Click "Launch Instance"
3. Select AMI, instance type, network settings
4. Configure security groups manually
5. Repeat for each environment
```

**IaC (Terraform)**:
```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}
```

---

## 2. Why Terraform? (Terraform vs CloudFormation vs ARM)

### Comparison Table

| Feature | Terraform | CloudFormation | ARM Templates |
|---------|-----------|----------------|---------------|
| **Cloud Support** | Multi-cloud (AWS, Azure, GCP, etc.) | AWS only | Azure only |
| **Language** | HCL (Human-readable) | JSON/YAML | JSON |
| **State Management** | Explicit state file | Implicit | Implicit |
| **Provider Ecosystem** | 3000+ providers | AWS services only | Azure services only |
| **Learning Curve** | Moderate | Moderate | Steep |
| **Community** | Very large | Large | Moderate |

### Why Choose Terraform?

1. **Multi-Cloud**: Single tool for AWS, Azure, GCP, Kubernetes, etc.
2. **Declarative**: Describe desired state, Terraform figures out how
3. **Immutable Infrastructure**: Encourages replacing rather than updating
4. **Large Ecosystem**: Thousands of providers and modules
5. **Open Source**: Free and community-driven

---

## 3. Terraform Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Terraform CLI                        │
│  (User interacts via: terraform init/plan/apply)       │
└─────────────────┬───────────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────────┐
│                  Terraform Core                         │
│  • Reads configuration files (.tf)                      │
│  • Manages state (terraform.tfstate)                    │
│  • Creates execution plan                               │
│  • Dependency graph                                     │
└─────────────────┬───────────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────────┐
│                    Providers                            │
│  • AWS Provider                                         │
│  • Azure Provider                                       │
│  • GCP Provider                                         │
│  • Kubernetes Provider                                  │
│  • 3000+ more...                                        │
└─────────────────┬───────────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────────┐
│              Cloud/Service APIs                         │
│  (AWS API, Azure API, GCP API, etc.)                   │
└─────────────────────────────────────────────────────────┘
```

### Terraform Core
- **Configuration Parser**: Reads .tf files
- **State Manager**: Tracks current infrastructure state
- **Graph Builder**: Creates dependency graph
- **Plan Generator**: Determines what changes are needed
- **Executor**: Applies changes via providers

### Providers
- Plugins that interact with APIs
- Each cloud/service has its own provider
- Examples: `aws`, `azurerm`, `google`, `kubernetes`

---

## 4. Installing Terraform

### Linux (Ubuntu/Debian)
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

### macOS
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

### Windows (Chocolatey)
```powershell
choco install terraform
```

### Verify Installation
```bash
terraform version
```

---

## 5. Terraform CLI Basics

### terraform version
```bash
# Check Terraform version
terraform version

# Output example:
# Terraform v1.6.0
# on linux_amd64
```

### terraform help
```bash
# General help
terraform help

# Help for specific command
terraform plan -help
terraform apply -help
```

### Common Commands Overview
```bash
terraform init      # Initialize working directory
terraform plan      # Preview changes
terraform apply     # Apply changes
terraform destroy   # Destroy infrastructure
terraform validate  # Validate configuration
terraform fmt       # Format code
terraform show      # Show current state
terraform output    # Show outputs
```

---

## 6. Terraform Configuration Language (HCL)

**HCL (HashiCorp Configuration Language)** is a declarative language designed to be human-readable and machine-friendly.

### Basic Syntax
```hcl
# This is a comment

/* 
  This is a
  multi-line comment
*/

# Block structure
<BLOCK_TYPE> "<BLOCK_LABEL>" "<BLOCK_LABEL>" {
  # Block body
  <IDENTIFIER> = <EXPRESSION>
}
```

### Example
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "ExampleInstance"
  }
}
```

---

## 7. Terraform Block

The `terraform` block configures Terraform itself.

```hcl
terraform {
  # Specify required Terraform version
  required_version = ">= 1.0"
  
  # Specify required providers
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
  
  # Configure backend (where state is stored)
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```

### Version Constraints
```hcl
# Exact version
version = "1.0.0"

# Greater than or equal
version = ">= 1.0"

# Pessimistic constraint (allows patch updates)
version = "~> 1.0"  # Allows 1.0.x, but not 1.1.0

# Range
version = ">= 1.0, < 2.0"
```

---

## 8. Provider Block

Providers are plugins that interact with cloud platforms and services.

### Basic Provider Configuration
```hcl
provider "aws" {
  region = "us-east-1"
}
```

### Provider with Credentials
```hcl
provider "aws" {
  region     = "us-east-1"
  access_key = "YOUR_ACCESS_KEY"  # Not recommended
  secret_key = "YOUR_SECRET_KEY"  # Not recommended
}
```

### Best Practice: Use Environment Variables
```bash
export AWS_ACCESS_KEY_ID="your_access_key"
export AWS_SECRET_ACCESS_KEY="your_secret_key"
export AWS_DEFAULT_REGION="us-east-1"
```

```hcl
provider "aws" {
  region = "us-east-1"
  # Credentials automatically picked from environment
}
```

### Multiple Providers
```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "east"
}

provider "aws" {
  region = "us-west-2"
  alias  = "west"
}

# Use specific provider
resource "aws_instance" "east_server" {
  provider = aws.east
  ami      = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

---

## 9. Resource Block

Resources are the most important element in Terraform. They represent infrastructure objects.

### Basic Resource Syntax
```hcl
resource "PROVIDER_RESOURCE_TYPE" "LOCAL_NAME" {
  argument1 = value1
  argument2 = value2
  
  nested_block {
    nested_argument = value
  }
}
```

### AWS EC2 Instance Example
```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name        = "WebServer"
    Environment = "Production"
  }
}
```

### AWS S3 Bucket Example
```hcl
resource "aws_s3_bucket" "data_bucket" {
  bucket = "my-unique-bucket-name-12345"
  
  tags = {
    Name        = "DataBucket"
    Environment = "Dev"
  }
}

resource "aws_s3_bucket_versioning" "data_bucket_versioning" {
  bucket = aws_s3_bucket.data_bucket.id
  
  versioning_configuration {
    status = "Enabled"
  }
}
```

### Resource References
```hcl
resource "aws_security_group" "web_sg" {
  name = "web-sg"
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  # Reference another resource
  vpc_security_group_ids = [aws_security_group.web_sg.id]
}
```

---

## 10. Terraform Init

`terraform init` initializes a working directory containing Terraform configuration files.

### What it Does:
1. Downloads required providers
2. Initializes backend
3. Downloads modules
4. Creates `.terraform` directory

### Example
```bash
# Initialize directory
terraform init

# Output:
# Initializing the backend...
# Initializing provider plugins...
# - Finding hashicorp/aws versions matching "~> 5.0"...
# - Installing hashicorp/aws v5.23.0...
# Terraform has been successfully initialized!
```

### Init Options
```bash
# Upgrade providers to latest version
terraform init -upgrade

# Reconfigure backend
terraform init -reconfigure

# Skip backend initialization
terraform init -backend=false
```

### Example Configuration
```hcl
# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "my-example-bucket-12345"
}
```

```bash
terraform init
```

---

## 11. Terraform Plan

`terraform plan` creates an execution plan, showing what Terraform will do.

### What it Does:
1. Reads current state
2. Reads configuration files
3. Compares desired vs current state
4. Shows what will be created, updated, or destroyed

### Example
```bash
terraform plan

# Output shows:
# + create
# ~ update in-place
# - destroy
# -/+ destroy and recreate
```

### Plan Output Example
```hcl
# main.tf
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}
```

```bash
terraform plan

# Output:
# Terraform will perform the following actions:
#
#   # aws_instance.web will be created
#   + resource "aws_instance" "web" {
#       + ami                    = "ami-0c55b159cbfafe1f0"
#       + instance_type          = "t2.micro"
#       + tags                   = {
#           + "Name" = "WebServer"
#         }
#       # ... other computed attributes
#     }
#
# Plan: 1 to add, 0 to change, 0 to destroy.
```

### Plan Options
```bash
# Save plan to file
terraform plan -out=tfplan

# Show detailed changes
terraform plan -refresh=true

# Target specific resource
terraform plan -target=aws_instance.web

# Use variable file
terraform plan -var-file="prod.tfvars"
```

---

## 12. Terraform Apply

`terraform apply` executes the planned changes to reach the desired state.

### What it Does:
1. Shows execution plan (unless using saved plan)
2. Asks for confirmation
3. Creates/updates/destroys resources
4. Updates state file

### Example
```bash
terraform apply

# Output:
# ... shows plan ...
# Do you want to perform these actions?
#   Enter a value: yes
#
# aws_instance.web: Creating...
# aws_instance.web: Creation complete after 45s
#
# Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

### Apply Options
```bash
# Auto-approve (skip confirmation)
terraform apply -auto-approve

# Apply saved plan
terraform apply tfplan

# Target specific resource
terraform apply -target=aws_instance.web

# Use variable file
terraform apply -var-file="prod.tfvars"

# Set variable inline
terraform apply -var="instance_type=t2.large"
```

### Complete Example
```hcl
# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "app_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "AppServer"
    Environment = "Development"
  }
}

output "instance_id" {
  value = aws_instance.app_server.id
}

output "public_ip" {
  value = aws_instance.app_server.public_ip
}
```

```bash
terraform init
terraform plan
terraform apply
```

---

## 13. Terraform Destroy

`terraform destroy` destroys all resources managed by Terraform.

### What it Does:
1. Shows destruction plan
2. Asks for confirmation
3. Destroys resources in reverse dependency order
4. Updates state file

### Example
```bash
terraform destroy

# Output:
# ... shows destruction plan ...
# Do you really want to destroy all resources?
#   Enter a value: yes
#
# aws_instance.web: Destroying...
# aws_instance.web: Destruction complete after 30s
#
# Destroy complete! Resources: 1 destroyed.
```

### Destroy Options
```bash
# Auto-approve
terraform destroy -auto-approve

# Destroy specific resource
terraform destroy -target=aws_instance.web

# Use variable file
terraform destroy -var-file="prod.tfvars"
```

### Prevent Accidental Destruction
```hcl
resource "aws_instance" "critical_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  lifecycle {
    prevent_destroy = true
  }
}
```

---

## 14. Terraform State (Introduction)

The **state file** (`terraform.tfstate`) is Terraform's record of your infrastructure.

### What State Contains:
- Resource IDs
- Resource attributes
- Dependencies
- Metadata

### State File Example
```json
{
  "version": 4,
  "terraform_version": "1.6.0",
  "resources": [
    {
      "mode": "managed",
      "type": "aws_instance",
      "name": "web",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "attributes": {
            "id": "i-0123456789abcdef",
            "ami": "ami-0c55b159cbfafe1f0",
            "instance_type": "t2.micro",
            "public_ip": "54.123.45.67"
          }
        }
      ]
    }
  ]
}
```

### Why State is Important:
1. **Mapping**: Maps configuration to real resources
2. **Performance**: Caches resource attributes
3. **Dependencies**: Tracks resource relationships
4. **Metadata**: Stores resource metadata

### State File Location
```hcl
# Local state (default)
# Stored in: terraform.tfstate

# Remote state (recommended for teams)
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```

### ⚠️ Important Notes:
- **Never** manually edit state file
- **Never** commit state file to version control (contains secrets)
- Use remote state for team collaboration
- State file can contain sensitive data

---

## 15. Terraform Workflow (Write → Plan → Apply)

### The Standard Workflow

```
┌──────────────────────────────────────────────────────────┐
│  1. WRITE                                                │
│  Write .tf configuration files                           │
│  Define desired infrastructure                           │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  2. INIT                                                 │
│  terraform init                                          │
│  Download providers and modules                          │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  3. PLAN                                                 │
│  terraform plan                                          │
│  Preview changes before applying                         │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  4. APPLY                                                │
│  terraform apply                                         │
│  Create/update infrastructure                            │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  5. DESTROY (when needed)                                │
│  terraform destroy                                       │
│  Clean up resources                                      │
└──────────────────────────────────────────────────────────┘
```

### Complete Example Workflow

**Step 1: Write Configuration**
```hcl
# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "MainVPC"
  }
}

resource "aws_subnet" "public" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
  
  tags = {
    Name = "PublicSubnet"
  }
}

output "vpc_id" {
  value = aws_vpc.main.id
}
```

**Step 2: Initialize**
```bash
terraform init
```

**Step 3: Plan**
```bash
terraform plan
```

**Step 4: Apply**
```bash
terraform apply
```

**Step 5: Make Changes**
```hcl
# Add another subnet
resource "aws_subnet" "private" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.2.0/24"
  
  tags = {
    Name = "PrivateSubnet"
  }
}
```

**Step 6: Plan and Apply Again**
```bash
terraform plan
terraform apply
```

**Step 7: Destroy (when done)**
```bash
terraform destroy
```

---

## Best Practices for Phase 1

1. **Always run `terraform plan` before `apply`**
2. **Use version control (Git) for .tf files**
3. **Never commit state files or secrets**
4. **Use consistent naming conventions**
5. **Add comments to complex configurations**
6. **Use `terraform fmt` to format code**
7. **Use `terraform validate` to check syntax**

---

## Practice Exercise

Create a simple AWS infrastructure:

```hcl
# exercise.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

# Create an S3 bucket
resource "aws_s3_bucket" "practice_bucket" {
  bucket = "my-practice-bucket-${random_id.bucket_suffix.hex}"
  
  tags = {
    Name        = "PracticeBucket"
    Environment = "Learning"
  }
}

# Generate random suffix for unique bucket name
resource "random_id" "bucket_suffix" {
  byte_length = 4
}

# Output the bucket name
output "bucket_name" {
  value = aws_s3_bucket.practice_bucket.id
}
```

**Tasks:**
1. Save this as `exercise.tf`
2. Run `terraform init`
3. Run `terraform plan`
4. Run `terraform apply`
5. Check AWS console to verify bucket creation
6. Run `terraform destroy` to clean up

# Phase 2: Variables & Outputs

## 1. Input Variables

Variables make your Terraform configurations dynamic and reusable.

### Basic Variable Declaration

```hcl
# variables.tf
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "environment" {
  description = "Environment name"
  type        = string
}
```

### Using Variables

```hcl
# main.tf
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type
  
  tags = {
    Name        = "WebServer"
    Environment = var.environment
  }
}
```

### Ways to Assign Variables

**1. Command Line**
```bash
terraform apply -var="environment=production" -var="instance_type=t2.large"
```

**2. Variable Files (.tfvars)**
```hcl
# terraform.tfvars (automatically loaded)
environment   = "production"
instance_type = "t2.large"
```

```hcl
# prod.tfvars (must specify with -var-file)
environment   = "production"
instance_type = "t2.large"
```

```bash
terraform apply -var-file="prod.tfvars"
```

**3. Environment Variables**
```bash
export TF_VAR_environment="production"
export TF_VAR_instance_type="t2.large"
terraform apply
```

**4. Interactive Prompt**
```bash
# If no value provided, Terraform will prompt
terraform apply
# var.environment
#   Enter a value: production
```

---

## 2. Variable Types

### String
```hcl
variable "region" {
  type    = string
  default = "us-east-1"
}

# Usage
provider "aws" {
  region = var.region
}
```

### Number
```hcl
variable "instance_count" {
  type    = number
  default = 3
}

variable "port" {
  type    = number
  default = 8080
}

# Usage
resource "aws_instance" "web" {
  count         = var.instance_count
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

### Bool
```hcl
variable "enable_monitoring" {
  type    = bool
  default = true
}

variable "is_production" {
  type    = bool
  default = false
}

# Usage
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  monitoring    = var.enable_monitoring
}
```

### List
```hcl
variable "availability_zones" {
  type    = list(string)
  default = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

variable "allowed_ports" {
  type    = list(number)
  default = [80, 443, 22]
}

# Usage
resource "aws_subnet" "public" {
  count             = length(var.availability_zones)
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.${count.index}.0/24"
  availability_zone = var.availability_zones[count.index]
}
```

### Map
```hcl
variable "instance_types" {
  type = map(string)
  default = {
    dev  = "t2.micro"
    qa   = "t2.small"
    prod = "t2.large"
  }
}

variable "ami_ids" {
  type = map(string)
  default = {
    us-east-1 = "ami-0c55b159cbfafe1f0"
    us-west-2 = "ami-0d1cd67c26f5fca19"
  }
}

# Usage
resource "aws_instance" "web" {
  ami           = var.ami_ids[var.region]
  instance_type = var.instance_types[var.environment]
}
```

### Object
```hcl
variable "server_config" {
  type = object({
    instance_type = string
    ami           = string
    disk_size     = number
    enable_public_ip = bool
  })
  
  default = {
    instance_type    = "t2.micro"
    ami              = "ami-0c55b159cbfafe1f0"
    disk_size        = 20
    enable_public_ip = true
  }
}

# Usage
resource "aws_instance" "web" {
  ami                         = var.server_config.ami
  instance_type               = var.server_config.instance_type
  associate_public_ip_address = var.server_config.enable_public_ip
  
  root_block_device {
    volume_size = var.server_config.disk_size
  }
}
```

### Complex Types
```hcl
variable "vpc_config" {
  type = object({
    cidr_block = string
    subnets = list(object({
      cidr_block        = string
      availability_zone = string
      public            = bool
    }))
    tags = map(string)
  })
  
  default = {
    cidr_block = "10.0.0.0/16"
    subnets = [
      {
        cidr_block        = "10.0.1.0/24"
        availability_zone = "us-east-1a"
        public            = true
      },
      {
        cidr_block        = "10.0.2.0/24"
        availability_zone = "us-east-1b"
        public            = false
      }
    ]
    tags = {
      Environment = "Production"
      ManagedBy   = "Terraform"
    }
  }
}
```

---

## 3. Default Values

```hcl
# Variable with default
variable "instance_type" {
  type    = string
  default = "t2.micro"
}

# Variable without default (required)
variable "environment" {
  type        = string
  description = "Environment name (required)"
}

# Variable with null default
variable "backup_retention_days" {
  type    = number
  default = null
}
```

---

## 4. Variable Validation

Add custom validation rules to variables.

```hcl
variable "environment" {
  type        = string
  description = "Environment name"
  
  validation {
    condition     = contains(["dev", "qa", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, qa, staging, or prod."
  }
}

variable "instance_type" {
  type = string
  
  validation {
    condition     = can(regex("^t2\\.", var.instance_type))
    error_message = "Instance type must be a t2 instance."
  }
}

variable "port" {
  type = number
  
  validation {
    condition     = var.port > 0 && var.port < 65536
    error_message = "Port must be between 1 and 65535."
  }
}

variable "cidr_block" {
  type = string
  
  validation {
    condition     = can(cidrhost(var.cidr_block, 0))
    error_message = "Must be a valid IPv4 CIDR block."
  }
}

variable "tags" {
  type = map(string)
  
  validation {
    condition     = contains(keys(var.tags), "Environment")
    error_message = "Tags must include 'Environment' key."
  }
}
```

### Complete Example with Validation
```hcl
# variables.tf
variable "environment" {
  description = "Deployment environment"
  type        = string
  
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

variable "instance_count" {
  description = "Number of instances"
  type        = number
  default     = 1
  
  validation {
    condition     = var.instance_count >= 1 && var.instance_count <= 10
    error_message = "Instance count must be between 1 and 10."
  }
}

# main.tf
resource "aws_instance" "app" {
  count         = var.instance_count
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name        = "AppServer-${count.index + 1}"
    Environment = var.environment
  }
}
```

---

## 5. Environment Variables (TF_VAR_)

Terraform automatically reads environment variables prefixed with `TF_VAR_`.

```bash
# Set environment variables
export TF_VAR_region="us-west-2"
export TF_VAR_instance_type="t2.large"
export TF_VAR_environment="production"

# Run Terraform
terraform plan
terraform apply
```

### Example
```hcl
# variables.tf
variable "region" {
  type = string
}

variable "instance_type" {
  type = string
}

variable "environment" {
  type = string
}

# main.tf
provider "aws" {
  region = var.region
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type
  
  tags = {
    Environment = var.environment
  }
}
```

```bash
# Set via environment
export TF_VAR_region="us-east-1"
export TF_VAR_instance_type="t2.micro"
export TF_VAR_environment="dev"

terraform apply
```

---

## 6. Output Values

Outputs display information about your infrastructure after `terraform apply`.

### Basic Output
```hcl
# outputs.tf
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.web.id
}

output "instance_public_ip" {
  description = "Public IP of the EC2 instance"
  value       = aws_instance.web.public_ip
}

output "instance_private_ip" {
  description = "Private IP of the EC2 instance"
  value       = aws_instance.web.private_ip
}
```

### Output with Multiple Resources
```hcl
output "instance_ids" {
  description = "IDs of all instances"
  value       = aws_instance.web[*].id
}

output "instance_ips" {
  description = "Public IPs of all instances"
  value       = aws_instance.web[*].public_ip
}
```

### Viewing Outputs
```bash
# View all outputs
terraform output

# View specific output
terraform output instance_id

# Output in JSON format
terraform output -json

# Output raw value (no quotes)
terraform output -raw instance_id
```

---

## 7. Sensitive Outputs

Mark outputs as sensitive to hide values from console output.

```hcl
# outputs.tf
output "db_password" {
  description = "Database password"
  value       = aws_db_instance.main.password
  sensitive   = true
}

output "api_key" {
  description = "API key for external service"
  value       = random_password.api_key.result
  sensitive   = true
}

output "instance_id" {
  description = "Instance ID (not sensitive)"
  value       = aws_instance.web.id
}
```

### Viewing Sensitive Outputs
```bash
# Sensitive values are hidden in plan/apply output
terraform apply

# View sensitive output explicitly
terraform output db_password

# View in JSON (shows sensitive values)
terraform output -json
```

### Complete Example
```hcl
# main.tf
resource "random_password" "db_password" {
  length  = 16
  special = true
}

resource "aws_db_instance" "main" {
  identifier        = "mydb"
  engine            = "postgres"
  instance_class    = "db.t3.micro"
  allocated_storage = 20
  username          = "admin"
  password          = random_password.db_password.result
  skip_final_snapshot = true
}

# outputs.tf
output "db_endpoint" {
  description = "Database endpoint"
  value       = aws_db_instance.main.endpoint
}

output "db_password" {
  description = "Database password"
  value       = random_password.db_password.result
  sensitive   = true
}
```

---

## 8. Locals (locals {})

Locals assign names to expressions for reuse within a module.

### Basic Locals
```hcl
locals {
  common_tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
    Project     = "WebApp"
  }
  
  name_prefix = "${var.project}-${var.environment}"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.name_prefix}-web-server"
    }
  )
}

resource "aws_s3_bucket" "data" {
  bucket = "${local.name_prefix}-data-bucket"
  tags   = local.common_tags
}
```

### Complex Locals
```hcl
locals {
  # Conditional logic
  instance_type = var.environment == "prod" ? "t2.large" : "t2.micro"
  
  # List manipulation
  availability_zones = slice(data.aws_availability_zones.available.names, 0, 3)
  
  # Map manipulation
  subnet_cidrs = {
    for idx, az in local.availability_zones :
    az => cidrsubnet(var.vpc_cidr, 8, idx)
  }
  
  # String manipulation
  bucket_name = lower(replace(var.project_name, " ", "-"))
  
  # Computed values
  total_instances = var.instance_count * length(local.availability_zones)
}
```

### Locals vs Variables

**Use Variables when:**
- Value comes from outside (user input)
- Value changes between environments
- Value needs validation

**Use Locals when:**
- Value is computed from other values
- Value is used multiple times
- Value simplifies complex expressions

### Complete Example
```hcl
# variables.tf
variable "project" {
  type = string
}

variable "environment" {
  type = string
}

variable "vpc_cidr" {
  type = string
}

# locals.tf
locals {
  common_tags = {
    Project     = var.project
    Environment = var.environment
    ManagedBy   = "Terraform"
    CreatedAt   = timestamp()
  }
  
  name_prefix = "${var.project}-${var.environment}"
  
  # Compute subnet CIDRs
  public_subnet_cidrs = [
    cidrsubnet(var.vpc_cidr, 8, 1),
    cidrsubnet(var.vpc_cidr, 8, 2),
    cidrsubnet(var.vpc_cidr, 8, 3)
  ]
  
  private_subnet_cidrs = [
    cidrsubnet(var.vpc_cidr, 8, 11),
    cidrsubnet(var.vpc_cidr, 8, 12),
    cidrsubnet(var.vpc_cidr, 8, 13)
  ]
  
  # Conditional instance type
  instance_type = var.environment == "prod" ? "t3.large" : "t3.micro"
}

# main.tf
resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr
  tags       = merge(local.common_tags, { Name = "${local.name_prefix}-vpc" })
}

resource "aws_subnet" "public" {
  count             = length(local.public_subnet_cidrs)
  vpc_id            = aws_vpc.main.id
  cidr_block        = local.public_subnet_cidrs[count.index]
  availability_zone = data.aws_availability_zones.available.names[count.index]
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.name_prefix}-public-${count.index + 1}"
      Type = "Public"
    }
  )
}

resource "aws_instance" "app" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = local.instance_type
  subnet_id     = aws_subnet.public[0].id
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.name_prefix}-app-server"
    }
  )
}
```

---

## Complete Project Example

### Directory Structure
```
project/
├── main.tf
├── variables.tf
├── outputs.tf
├── locals.tf
├── terraform.tfvars
├── dev.tfvars
└── prod.tfvars
```

### variables.tf
```hcl
variable "project" {
  description = "Project name"
  type        = string
}

variable "environment" {
  description = "Environment (dev, staging, prod)"
  type        = string
  
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

variable "region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "vpc_cidr" {
  description = "VPC CIDR block"
  type        = string
  default     = "10.0.0.0/16"
}

variable "instance_count" {
  description = "Number of instances"
  type        = number
  default     = 1
  
  validation {
    condition     = var.instance_count >= 1 && var.instance_count <= 10
    error_message = "Instance count must be between 1 and 10."
  }
}

variable "enable_monitoring" {
  description = "Enable detailed monitoring"
  type        = bool
  default     = false
}

variable "tags" {
  description = "Additional tags"
  type        = map(string)
  default     = {}
}
```

### locals.tf
```hcl
locals {
  common_tags = merge(
    {
      Project     = var.project
      Environment = var.environment
      ManagedBy   = "Terraform"
    },
    var.tags
  )
  
  name_prefix = "${var.project}-${var.environment}"
  
  instance_type = var.environment == "prod" ? "t3.large" : "t3.micro"
}
```

### main.tf
```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.name_prefix}-vpc"
    }
  )
}

resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 8, 1)
  map_public_ip_on_launch = true
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.name_prefix}-public-subnet"
    }
  )
}

resource "aws_instance" "app" {
  count         = var.instance_count
  ami           = data.aws_ami.ubuntu.id
  instance_type = local.instance_type
  subnet_id     = aws_subnet.public.id
  monitoring    = var.enable_monitoring
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.name_prefix}-app-${count.index + 1}"
    }
  )
}

data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}
```

### outputs.tf
```hcl
output "vpc_id" {
  description = "VPC ID"
  value       = aws_vpc.main.id
}

output "subnet_id" {
  description = "Subnet ID"
  value       = aws_subnet.public.id
}

output "instance_ids" {
  description = "Instance IDs"
  value       = aws_instance.app[*].id
}

output "instance_public_ips" {
  description = "Instance public IPs"
  value       = aws_instance.app[*].public_ip
}

output "instance_private_ips" {
  description = "Instance private IPs"
  value       = aws_instance.app[*].private_ip
}
```

### dev.tfvars
```hcl
project          = "myapp"
environment      = "dev"
region           = "us-east-1"
vpc_cidr         = "10.0.0.0/16"
instance_count   = 1
enable_monitoring = false

tags = {
  Team = "DevOps"
  CostCenter = "Engineering"
}
```

### prod.tfvars
```hcl
project          = "myapp"
environment      = "prod"
region           = "us-east-1"
vpc_cidr         = "10.1.0.0/16"
instance_count   = 3
enable_monitoring = true

tags = {
  Team = "DevOps"
  CostCenter = "Production"
}
```

### Usage
```bash
# Development
terraform apply -var-file="dev.tfvars"

# Production
terraform apply -var-file="prod.tfvars"
```

---

## Best Practices

1. **Use descriptive variable names**
2. **Always add descriptions to variables**
3. **Use validation for critical variables**
4. **Group related variables together**
5. **Use locals for computed values**
6. **Mark sensitive outputs appropriately**
7. **Use .tfvars files for different environments**
8. **Never commit .tfvars with secrets to version control**
9. **Use type constraints to catch errors early**
10. **Document complex variable structures**

# Phase 3: Providers & Resources (Deep Dive)

## 1. Providers (AWS, Azure, GCP, Kubernetes)

Providers are plugins that enable Terraform to interact with cloud platforms, SaaS providers, and APIs.

### AWS Provider
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region     = "us-east-1"
  access_key = var.aws_access_key  # Not recommended
  secret_key = var.aws_secret_key  # Not recommended
  
  # Better: Use AWS CLI credentials or IAM roles
  profile = "default"
  
  default_tags {
    tags = {
      ManagedBy = "Terraform"
      Project   = "MyProject"
    }
  }
}
```

### Azure Provider
```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

provider "azurerm" {
  features {}
  
  subscription_id = var.subscription_id
  tenant_id       = var.tenant_id
  
  # Or use Azure CLI authentication
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}
```

### GCP Provider
```hcl
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 5.0"
    }
  }
}

provider "google" {
  project     = "my-project-id"
  region      = "us-central1"
  credentials = file("path/to/credentials.json")
  
  # Or use Application Default Credentials
}

resource "google_compute_instance" "default" {
  name         = "test-instance"
  machine_type = "e2-medium"
  zone         = "us-central1-a"
  
  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }
  
  network_interface {
    network = "default"
  }
}
```

### Kubernetes Provider
```hcl
terraform {
  required_providers {
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "~> 2.0"
    }
  }
}

provider "kubernetes" {
  config_path = "~/.kube/config"
  
  # Or use in-cluster config
  # host                   = var.cluster_endpoint
  # cluster_ca_certificate = base64decode(var.cluster_ca_cert)
  # token                  = var.cluster_token
}

resource "kubernetes_namespace" "example" {
  metadata {
    name = "my-namespace"
  }
}
```

---

## 2. Provider Version Constraints

### Version Constraint Operators

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      
      # Exact version
      version = "5.23.0"
      
      # Greater than or equal
      version = ">= 5.0.0"
      
      # Less than
      version = "< 6.0.0"
      
      # Pessimistic constraint (recommended)
      version = "~> 5.0"  # Allows 5.x but not 6.0
      
      # Range
      version = ">= 5.0, < 6.0"
    }
  }
}
```

### Best Practices
```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"  # Allow patch and minor updates
    }
    
    random = {
      source  = "hashicorp/random"
      version = "~> 3.0"
    }
  }
}
```

---

## 3. Multiple Providers

Use multiple provider configurations for different regions or accounts.

### Multiple AWS Regions
```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "east"
}

provider "aws" {
  region = "us-west-2"
  alias  = "west"
}

# Use default provider (first declared without alias)
resource "aws_s3_bucket" "default" {
  bucket = "my-default-bucket"
}

# Use specific provider
resource "aws_instance" "east_server" {
  provider      = aws.east
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

resource "aws_instance" "west_server" {
  provider      = aws.west
  ami           = "ami-0d1cd67c26f5fca19"
  instance_type = "t2.micro"
}
```

### Multiple AWS Accounts
```hcl
provider "aws" {
  region  = "us-east-1"
  profile = "account-dev"
  alias   = "dev"
}

provider "aws" {
  region  = "us-east-1"
  profile = "account-prod"
  alias   = "prod"
}

resource "aws_s3_bucket" "dev_bucket" {
  provider = aws.dev
  bucket   = "dev-data-bucket"
}

resource "aws_s3_bucket" "prod_bucket" {
  provider = aws.prod
  bucket   = "prod-data-bucket"
}
```

### Cross-Provider Example
```hcl
provider "aws" {
  region = "us-east-1"
}

provider "cloudflare" {
  api_token = var.cloudflare_api_token
}

# AWS S3 bucket
resource "aws_s3_bucket" "website" {
  bucket = "my-website-bucket"
}

# Cloudflare DNS record pointing to S3
resource "cloudflare_record" "website" {
  zone_id = var.cloudflare_zone_id
  name    = "www"
  value   = aws_s3_bucket.website.website_endpoint
  type    = "CNAME"
  proxied = true
}
```

---

## 4. Provider Aliases

Aliases allow multiple configurations of the same provider.

```hcl
# providers.tf
provider "aws" {
  region = "us-east-1"
  alias  = "primary"
  
  default_tags {
    tags = {
      Region = "Primary"
    }
  }
}

provider "aws" {
  region = "us-west-2"
  alias  = "secondary"
  
  default_tags {
    tags = {
      Region = "Secondary"
    }
  }
}

# main.tf
resource "aws_vpc" "primary" {
  provider   = aws.primary
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "Primary VPC"
  }
}

resource "aws_vpc" "secondary" {
  provider   = aws.secondary
  cidr_block = "10.1.0.0/16"
  
  tags = {
    Name = "Secondary VPC"
  }
}

# VPC Peering between regions
resource "aws_vpc_peering_connection" "peer" {
  provider    = aws.primary
  vpc_id      = aws_vpc.primary.id
  peer_vpc_id = aws_vpc.secondary.id
  peer_region = "us-west-2"
  auto_accept = false
}

resource "aws_vpc_peering_connection_accepter" "peer" {
  provider                  = aws.secondary
  vpc_peering_connection_id = aws_vpc_peering_connection.peer.id
  auto_accept               = true
}
```

---

## 5. Resource Meta-Arguments

Meta-arguments change resource behavior.

### depends_on

Explicitly define dependencies between resources.

```hcl
resource "aws_iam_role" "example" {
  name = "example-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"
      }
    }]
  })
}

resource "aws_iam_role_policy" "example" {
  name = "example-policy"
  role = aws_iam_role.example.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action   = ["s3:*"]
      Effect   = "Allow"
      Resource = "*"
    }]
  })
}

resource "aws_iam_instance_profile" "example" {
  name = "example-profile"
  role = aws_iam_role.example.name
  
  # Ensure policy is attached before creating profile
  depends_on = [aws_iam_role_policy.example]
}

resource "aws_instance" "example" {
  ami                  = "ami-0c55b159cbfafe1f0"
  instance_type        = "t2.micro"
  iam_instance_profile = aws_iam_instance_profile.example.name
  
  # Ensure IAM resources are fully created
  depends_on = [aws_iam_instance_profile.example]
}
```

### count

Create multiple similar resources.

```hcl
# Create 3 EC2 instances
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer-${count.index + 1}"
  }
}

# Reference specific instance
output "first_instance_id" {
  value = aws_instance.web[0].id
}

# Reference all instances
output "all_instance_ids" {
  value = aws_instance.web[*].id
}

# Conditional creation
variable "create_instance" {
  type    = bool
  default = true
}

resource "aws_instance" "conditional" {
  count         = var.create_instance ? 1 : 0
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

# Create resources based on list
variable "availability_zones" {
  type    = list(string)
  default = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

resource "aws_subnet" "public" {
  count             = length(var.availability_zones)
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet("10.0.0.0/16", 8, count.index)
  availability_zone = var.availability_zones[count.index]
  
  tags = {
    Name = "Public-${var.availability_zones[count.index]}"
  }
}
```

### for_each

Create resources based on a map or set.

```hcl
# Using set of strings
variable "user_names" {
  type    = set(string)
  default = ["alice", "bob", "charlie"]
}

resource "aws_iam_user" "users" {
  for_each = var.user_names
  name     = each.value
}

# Using map
variable "instances" {
  type = map(object({
    instance_type = string
    ami           = string
  }))
  
  default = {
    web = {
      instance_type = "t2.micro"
      ami           = "ami-0c55b159cbfafe1f0"
    }
    app = {
      instance_type = "t2.small"
      ami           = "ami-0c55b159cbfafe1f0"
    }
    db = {
      instance_type = "t2.medium"
      ami           = "ami-0c55b159cbfafe1f0"
    }
  }
}

resource "aws_instance" "servers" {
  for_each      = var.instances
  ami           = each.value.ami
  instance_type = each.value.instance_type
  
  tags = {
    Name = "${each.key}-server"
    Type = each.key
  }
}

# Reference specific resource
output "web_server_id" {
  value = aws_instance.servers["web"].id
}

# Reference all resources
output "all_server_ids" {
  value = { for k, v in aws_instance.servers : k => v.id }
}
```

### count vs for_each

**Use count when:**
- Creating identical resources
- Number of resources is known
- Resources don't need unique identifiers

**Use for_each when:**
- Each resource has unique configuration
- Resources need stable identifiers
- Adding/removing items shouldn't affect others

```hcl
# BAD: Using count with map (changing order affects all resources)
variable "users" {
  type    = list(string)
  default = ["alice", "bob", "charlie"]
}

resource "aws_iam_user" "bad_example" {
  count = length(var.users)
  name  = var.users[count.index]
  # If "bob" is removed, "charlie" becomes index 1, causing recreation
}

# GOOD: Using for_each with set (stable identifiers)
resource "aws_iam_user" "good_example" {
  for_each = toset(var.users)
  name     = each.value
  # Removing "bob" only affects "bob", not "charlie"
}
```

### lifecycle

Control resource lifecycle behavior.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  lifecycle {
    # Prevent accidental deletion
    prevent_destroy = true
    
    # Create new resource before destroying old
    create_before_destroy = true
    
    # Ignore changes to specific attributes
    ignore_changes = [
      tags,
      user_data,
    ]
    
    # Ignore all changes
    # ignore_changes = all
  }
}

# Example: Database with prevent_destroy
resource "aws_db_instance" "production" {
  identifier        = "prod-db"
  engine            = "postgres"
  instance_class    = "db.t3.micro"
  allocated_storage = 20
  username          = "admin"
  password          = var.db_password
  
  lifecycle {
    prevent_destroy = true
  }
}

# Example: Auto Scaling Group with create_before_destroy
resource "aws_launch_template" "app" {
  name_prefix   = "app-"
  image_id      = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  lifecycle {
    create_before_destroy = true
  }
}

# Example: Ignore tags managed by external systems
resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "AppServer"
  }
  
  lifecycle {
    ignore_changes = [tags]
  }
}
```

---

## 6. Resource Naming Conventions

### Best Practices

```hcl
# GOOD: Descriptive, consistent naming
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "${var.project}-${var.environment}-vpc"
  }
}

resource "aws_subnet" "public_web" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
  
  tags = {
    Name = "${var.project}-${var.environment}-public-web-subnet"
  }
}

resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public_web.id
  
  tags = {
    Name = "${var.project}-${var.environment}-web-server"
  }
}

# BAD: Generic, unclear naming
resource "aws_vpc" "vpc1" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "subnet1" {
  vpc_id     = aws_vpc.vpc1.id
  cidr_block = "10.0.1.0/24"
}
```

### Naming Pattern
```
<resource_type>_<purpose>_<modifier>

Examples:
- aws_vpc_main
- aws_subnet_public_web
- aws_instance_app_server
- aws_security_group_web_lb
- aws_s3_bucket_data_lake
```

---

## 7. Data Sources

Data sources allow Terraform to fetch information from existing resources.

### Basic Data Source
```hcl
# Fetch latest Ubuntu AMI
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
  
  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}

# Use data source
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
}
```

### Common Data Sources

```hcl
# Get availability zones
data "aws_availability_zones" "available" {
  state = "available"
}

# Get current region
data "aws_region" "current" {}

# Get caller identity
data "aws_caller_identity" "current" {}

# Get VPC by tag
data "aws_vpc" "selected" {
  tags = {
    Name = "production-vpc"
  }
}

# Get subnet IDs
data "aws_subnets" "private" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.selected.id]
  }
  
  tags = {
    Type = "Private"
  }
}

# Use data sources
resource "aws_instance" "app" {
  ami               = data.aws_ami.ubuntu.id
  instance_type     = "t2.micro"
  availability_zone = data.aws_availability_zones.available.names[0]
  subnet_id         = data.aws_subnets.private.ids[0]
  
  tags = {
    Region    = data.aws_region.current.name
    AccountID = data.aws_caller_identity.current.account_id
  }
}
```

---

## 8. Data Source vs Resource

### Resource (Managed by Terraform)
```hcl
# Terraform creates and manages this VPC
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "MainVPC"
  }
}

# Terraform can:
# - Create it
# - Update it
# - Destroy it
```

### Data Source (Read-only, Managed Externally)
```hcl
# Terraform only reads this VPC (created manually or by another process)
data "aws_vpc" "existing" {
  filter {
    name   = "tag:Name"
    values = ["ExistingVPC"]
  }
}

# Terraform can:
# - Read its attributes
# - Use it in other resources
# - Cannot modify or destroy it
```

### Combined Example
```hcl
# Data: Existing VPC (not managed by Terraform)
data "aws_vpc" "existing" {
  tags = {
    Name = "legacy-vpc"
  }
}

# Resource: New subnet in existing VPC
resource "aws_subnet" "new_subnet" {
  vpc_id     = data.aws_vpc.existing.id
  cidr_block = "10.0.100.0/24"
  
  tags = {
    Name = "NewSubnet"
  }
}

# Resource: New instance in new subnet
resource "aws_instance" "app" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.new_subnet.id
}
```

---

## Complete Example: Multi-Region Setup

```hcl
# providers.tf
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  alias  = "primary"
}

provider "aws" {
  region = "us-west-2"
  alias  = "secondary"
}

# data.tf
data "aws_availability_zones" "primary" {
  provider = aws.primary
  state    = "available"
}

data "aws_availability_zones" "secondary" {
  provider = aws.secondary
  state    = "available"
}

data "aws_ami" "ubuntu_primary" {
  provider    = aws.primary
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

data "aws_ami" "ubuntu_secondary" {
  provider    = aws.secondary
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

# main.tf
locals {
  common_tags = {
    Project   = "MultiRegion"
    ManagedBy = "Terraform"
  }
}

# Primary Region Resources
resource "aws_vpc" "primary" {
  provider   = aws.primary
  cidr_block = "10.0.0.0/16"
  
  tags = merge(local.common_tags, {
    Name   = "primary-vpc"
    Region = "us-east-1"
  })
}

resource "aws_subnet" "primary" {
  count             = 2
  provider          = aws.primary
  vpc_id            = aws_vpc.primary.id
  cidr_block        = cidrsubnet(aws_vpc.primary.cidr_block, 8, count.index)
  availability_zone = data.aws_availability_zones.primary.names[count.index]
  
  tags = merge(local.common_tags, {
    Name = "primary-subnet-${count.index + 1}"
  })
}

resource "aws_instance" "primary" {
  for_each = toset(["web", "app", "db"])
  
  provider      = aws.primary
  ami           = data.aws_ami.ubuntu_primary.id
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.primary[0].id
  
  lifecycle {
    create_before_destroy = true
  }
  
  tags = merge(local.common_tags, {
    Name = "primary-${each.key}"
    Type = each.key
  })
}

# Secondary Region Resources
resource "aws_vpc" "secondary" {
  provider   = aws.secondary
  cidr_block = "10.1.0.0/16"
  
  tags = merge(local.common_tags, {
    Name   = "secondary-vpc"
    Region = "us-west-2"
  })
}

resource "aws_subnet" "secondary" {
  count             = 2
  provider          = aws.secondary
  vpc_id            = aws_vpc.secondary.id
  cidr_block        = cidrsubnet(aws_vpc.secondary.cidr_block, 8, count.index)
  availability_zone = data.aws_availability_zones.secondary.names[count.index]
  
  tags = merge(local.common_tags, {
    Name = "secondary-subnet-${count.index + 1}"
  })
}

resource "aws_instance" "secondary" {
  for_each = toset(["web", "app"])
  
  provider      = aws.secondary
  ami           = data.aws_ami.ubuntu_secondary.id
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.secondary[0].id
  
  lifecycle {
    create_before_destroy = true
  }
  
  tags = merge(local.common_tags, {
    Name = "secondary-${each.key}"
    Type = each.key
  })
}

# outputs.tf
output "primary_instance_ids" {
  value = { for k, v in aws_instance.primary : k => v.id }
}

output "secondary_instance_ids" {
  value = { for k, v in aws_instance.secondary : k => v.id }
}
```

---

## Best Practices

1. **Use data sources for existing resources**
2. **Prefer for_each over count for maps/sets**
3. **Use lifecycle rules to prevent accidents**
4. **Use provider aliases for multi-region/account**
5. **Follow consistent naming conventions**
6. **Use depends_on sparingly (Terraform usually figures it out)**
7. **Version pin your providers**
8. **Use descriptive resource names**

# Phase 4: State Management (VERY IMPORTANT)

## 1. Terraform State File (terraform.tfstate)

The state file is Terraform's database of your infrastructure.

### What's in the State File?

```json
{
  "version": 4,
  "terraform_version": "1.6.0",
  "serial": 1,
  "lineage": "unique-id-here",
  "outputs": {
    "instance_id": {
      "value": "i-0123456789abcdef",
      "type": "string"
    }
  },
  "resources": [
    {
      "mode": "managed",
      "type": "aws_instance",
      "name": "web",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 1,
          "attributes": {
            "id": "i-0123456789abcdef",
            "ami": "ami-0c55b159cbfafe1f0",
            "instance_type": "t2.micro",
            "public_ip": "54.123.45.67",
            "private_ip": "10.0.1.50",
            "tags": {
              "Name": "WebServer"
            }
          },
          "dependencies": [
            "aws_security_group.web_sg"
          ]
        }
      ]
    }
  ]
}
```

### State File Purpose

1. **Mapping**: Maps Terraform config to real-world resources
2. **Metadata**: Stores resource metadata and dependencies
3. **Performance**: Caches attribute values for large infrastructures
4. **Collaboration**: Enables team collaboration via remote state

---

## 2. Desired State vs Current State

### Desired State
What you define in your `.tf` files

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.small"  # Desired: t2.small
  
  tags = {
    Name = "WebServer"
  }
}
```

### Current State
What actually exists in your cloud provider (tracked in `terraform.tfstate`)

```json
{
  "attributes": {
    "instance_type": "t2.micro",  // Current: t2.micro
    "id": "i-0123456789abcdef"
  }
}
```

### Terraform Workflow

```
┌─────────────────────────────────────────────────────────┐
│  terraform plan                                         │
│  1. Read desired state from .tf files                   │
│  2. Read current state from terraform.tfstate           │
│  3. Query actual state from cloud provider              │
│  4. Compare and show differences                        │
└─────────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│  terraform apply                                        │
│  1. Execute changes to match desired state              │
│  2. Update terraform.tfstate with new current state     │
└─────────────────────────────────────────────────────────┘
```

---

## 3. State Locking

Prevents concurrent modifications to state file.

### Why State Locking?

**Without locking:**
```
User A: terraform apply (starts)
User B: terraform apply (starts at same time)
Result: Corrupted state, conflicting changes
```

**With locking:**
```
User A: terraform apply (acquires lock)
User B: terraform apply (waits for lock)
Result: Safe, sequential execution
```

### Backends with Locking Support

| Backend | Locking Support | Notes |
|---------|----------------|-------|
| S3 + DynamoDB | ✅ Yes | Most common for AWS |
| Azure Blob | ✅ Yes | Built-in |
| GCS | ✅ Yes | Built-in |
| Terraform Cloud | ✅ Yes | Built-in |
| Local | ❌ No | Not recommended for teams |

### S3 + DynamoDB Locking Example

```hcl
# backend.tf
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
  }
}

# Create DynamoDB table for locking
resource "aws_dynamodb_table" "terraform_lock" {
  name           = "terraform-state-lock"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "LockID"
  
  attribute {
    name = "LockID"
    type = "S"
  }
  
  tags = {
    Name = "Terraform State Lock"
  }
}
```

### Force Unlock (Use with Caution)

```bash
# If a lock is stuck (e.g., process crashed)
terraform force-unlock <LOCK_ID>

# Example
terraform force-unlock 12345678-1234-1234-1234-123456789abc
```

---

## 4. Remote Backend

Store state file remotely for team collaboration.

### S3 Backend (AWS)

```hcl
# backend.tf
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
    
    # Optional: Server-side encryption
    kms_key_id = "arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789abc"
  }
}
```

**Setup Steps:**

1. Create S3 bucket:
```bash
aws s3api create-bucket \
  --bucket my-terraform-state \
  --region us-east-1

aws s3api put-bucket-versioning \
  --bucket my-terraform-state \
  --versioning-configuration Status=Enabled

aws s3api put-bucket-encryption \
  --bucket my-terraform-state \
  --server-side-encryption-configuration '{
    "Rules": [{
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "AES256"
      }
    }]
  }'
```

2. Create DynamoDB table:
```bash
aws dynamodb create-table \
  --table-name terraform-state-lock \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST
```

3. Initialize backend:
```bash
terraform init
```

### Azure Blob Backend

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "terraform-state-rg"
    storage_account_name = "terraformstate"
    container_name       = "tfstate"
    key                  = "prod.terraform.tfstate"
  }
}
```

### GCS Backend (Google Cloud)

```hcl
terraform {
  backend "gcs" {
    bucket = "my-terraform-state"
    prefix = "prod"
  }
}
```

### Terraform Cloud Backend

```hcl
terraform {
  cloud {
    organization = "my-org"
    
    workspaces {
      name = "my-workspace"
    }
  }
}
```

---

## 5. Backend Types

### Local Backend (Default)
```hcl
# No backend block = local backend
# State stored in: terraform.tfstate
```

**Pros:**
- Simple, no setup required
- Fast

**Cons:**
- No collaboration
- No locking
- No versioning
- State on local machine only

### S3 Backend
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "path/to/terraform.tfstate"
    region = "us-east-1"
  }
}
```

**Pros:**
- Versioning support
- Encryption support
- Locking (with DynamoDB)
- Cost-effective

**Cons:**
- Requires AWS setup
- Need to manage S3 bucket

### Terraform Cloud
```hcl
terraform {
  cloud {
    organization = "my-org"
    workspaces {
      name = "my-workspace"
    }
  }
}
```

**Pros:**
- Built-in locking
- State versioning
- Access controls
- Remote execution
- Free tier available

**Cons:**
- Requires internet connection
- Vendor lock-in

---

## 6. Backend Configuration

### Partial Configuration

```hcl
# backend.tf
terraform {
  backend "s3" {
    # Static configuration
    region  = "us-east-1"
    encrypt = true
  }
}
```

```bash
# Provide dynamic configuration at init
terraform init \
  -backend-config="bucket=my-terraform-state" \
  -backend-config="key=prod/terraform.tfstate" \
  -backend-config="dynamodb_table=terraform-state-lock"
```

### Backend Configuration File

```hcl
# backend.hcl
bucket         = "my-terraform-state"
key            = "prod/terraform.tfstate"
region         = "us-east-1"
dynamodb_table = "terraform-state-lock"
encrypt        = true
```

```bash
terraform init -backend-config=backend.hcl
```

### Environment-Specific Backends

```hcl
# backend-dev.hcl
bucket = "terraform-state-dev"
key    = "dev/terraform.tfstate"
region = "us-east-1"
```

```hcl
# backend-prod.hcl
bucket = "terraform-state-prod"
key    = "prod/terraform.tfstate"
region = "us-east-1"
```

```bash
# Development
terraform init -backend-config=backend-dev.hcl

# Production
terraform init -backend-config=backend-prod.hcl
```

---

## 7. State File Security

### Security Best Practices

1. **Enable Encryption**
```hcl
terraform {
  backend "s3" {
    bucket  = "my-terraform-state"
    key     = "terraform.tfstate"
    region  = "us-east-1"
    encrypt = true  # Enable encryption at rest
    
    # Use KMS for encryption
    kms_key_id = "arn:aws:kms:us-east-1:123456789012:key/12345678"
  }
}
```

2. **Enable Versioning**
```bash
aws s3api put-bucket-versioning \
  --bucket my-terraform-state \
  --versioning-configuration Status=Enabled
```

3. **Restrict Access**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:role/TerraformRole"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-terraform-state/*"
    }
  ]
}
```

4. **Enable Logging**
```bash
aws s3api put-bucket-logging \
  --bucket my-terraform-state \
  --bucket-logging-status '{
    "LoggingEnabled": {
      "TargetBucket": "my-logs-bucket",
      "TargetPrefix": "terraform-state-logs/"
    }
  }'
```

5. **Never Commit State Files**
```bash
# .gitignore
*.tfstate
*.tfstate.*
.terraform/
```

### Sensitive Data in State

State files contain sensitive data:
- Passwords
- Private keys
- API tokens
- Database connection strings

**Example:**
```hcl
resource "aws_db_instance" "main" {
  identifier = "mydb"
  engine     = "postgres"
  username   = "admin"
  password   = "SuperSecret123!"  # This will be in state file!
}
```

**Better approach:**
```hcl
resource "random_password" "db_password" {
  length  = 16
  special = true
}

resource "aws_db_instance" "main" {
  identifier = "mydb"
  engine     = "postgres"
  username   = "admin"
  password   = random_password.db_password.result
}

# Store password in Secrets Manager
resource "aws_secretsmanager_secret" "db_password" {
  name = "db-password"
}

resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = random_password.db_password.result
}
```

---

## 8. State Drift

State drift occurs when real infrastructure differs from Terraform state.

### Causes of Drift

1. **Manual changes** in cloud console
2. **External automation** (scripts, other tools)
3. **Resource deletion** outside Terraform
4. **Failed applies** that partially complete

### Detecting Drift

```bash
# Refresh state and show differences
terraform plan -refresh-only

# Or
terraform plan
```

**Example Output:**
```
Note: Objects have changed outside of Terraform

Terraform detected the following changes made outside of Terraform since the
last "terraform apply":

  # aws_instance.web has changed
  ~ resource "aws_instance" "web" {
        id            = "i-0123456789abcdef"
      ~ instance_type = "t2.micro" -> "t2.small"
        # (other attributes unchanged)
    }
```

---

## 9. terraform refresh

Updates state file to match real infrastructure.

```bash
# Refresh state (deprecated, use plan -refresh-only)
terraform refresh

# Better: Use plan with refresh-only
terraform plan -refresh-only

# Apply the refresh
terraform apply -refresh-only
```

### Example

**Current state:**
```json
{
  "instance_type": "t2.micro"
}
```

**Real infrastructure:**
```
instance_type: t2.small (changed manually)
```

**After refresh:**
```json
{
  "instance_type": "t2.small"
}
```

---

## 10. terraform state list

List all resources in state file.

```bash
# List all resources
terraform state list

# Output:
# aws_instance.web
# aws_security_group.web_sg
# aws_vpc.main
# aws_subnet.public[0]
# aws_subnet.public[1]
```

```bash
# Filter by resource type
terraform state list | grep aws_instance

# Filter by name pattern
terraform state list | grep web
```

---

## 11. terraform state show

Show detailed information about a specific resource.

```bash
# Show specific resource
terraform state show aws_instance.web

# Output:
# resource "aws_instance" "web" {
#     ami                          = "ami-0c55b159cbfafe1f0"
#     instance_type                = "t2.micro"
#     id                           = "i-0123456789abcdef"
#     public_ip                    = "54.123.45.67"
#     private_ip                   = "10.0.1.50"
#     vpc_security_group_ids       = ["sg-0123456789abcdef"]
#     subnet_id                    = "subnet-0123456789abcdef"
#     tags                         = {
#         "Name" = "WebServer"
#     }
# }
```

---

## 12. terraform state rm

Remove resource from state without destroying it.

```bash
# Remove resource from state
terraform state rm aws_instance.web

# Remove multiple resources
terraform state rm aws_instance.web aws_instance.app

# Remove all instances of a resource with count
terraform state rm 'aws_subnet.public[0]'
terraform state rm 'aws_subnet.public[1]'

# Remove all instances with for_each
terraform state rm 'aws_instance.servers["web"]'
```

### Use Cases

1. **Stop managing a resource**
```bash
# Remove from Terraform management but keep in cloud
terraform state rm aws_instance.legacy_server
```

2. **Before refactoring**
```bash
# Remove old resource before renaming
terraform state rm aws_instance.old_name
# Then rename in code and import as new name
```

---

## 13. terraform import

Import existing infrastructure into Terraform state.

### Basic Import

```bash
# Import EC2 instance
terraform import aws_instance.web i-0123456789abcdef

# Import S3 bucket
terraform import aws_s3_bucket.data my-existing-bucket

# Import VPC
terraform import aws_vpc.main vpc-0123456789abcdef
```

### Import Process

1. **Write configuration**
```hcl
resource "aws_instance" "existing" {
  # Add minimal required arguments
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

2. **Import resource**
```bash
terraform import aws_instance.existing i-0123456789abcdef
```

3. **Run plan to see differences**
```bash
terraform plan
```

4. **Update configuration to match**
```hcl
resource "aws_instance" "existing" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "ExistingServer"
  }
  
  # Add all other attributes shown in plan
}
```

5. **Verify no changes needed**
```bash
terraform plan
# Should show: No changes. Your infrastructure matches the configuration.
```

### Import Examples

```bash
# EC2 Instance
terraform import aws_instance.web i-0123456789abcdef

# Security Group
terraform import aws_security_group.web sg-0123456789abcdef

# VPC
terraform import aws_vpc.main vpc-0123456789abcdef

# Subnet
terraform import aws_subnet.public subnet-0123456789abcdef

# S3 Bucket
terraform import aws_s3_bucket.data my-bucket-name

# RDS Instance
terraform import aws_db_instance.main mydb-instance

# IAM Role
terraform import aws_iam_role.example my-role-name

# Route53 Zone
terraform import aws_route53_zone.main Z1234567890ABC
```

### Import with for_each

```hcl
# Configuration
resource "aws_instance" "servers" {
  for_each = toset(["web", "app", "db"])
  
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "${each.key}-server"
  }
}
```

```bash
# Import each instance
terraform import 'aws_instance.servers["web"]' i-0123456789abcdef0
terraform import 'aws_instance.servers["app"]' i-0123456789abcdef1
terraform import 'aws_instance.servers["db"]' i-0123456789abcdef2
```

---

## Complete State Management Example

### Scenario: Migrating to Remote State

**Step 1: Current Setup (Local State)**
```hcl
# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "data" {
  bucket = "my-data-bucket"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

**Step 2: Create Backend Infrastructure**
```hcl
# backend-setup.tf (separate directory)
provider "aws" {
  region = "us-east-1"
}

# S3 bucket for state
resource "aws_s3_bucket" "terraform_state" {
  bucket = "my-terraform-state-bucket"
  
  lifecycle {
    prevent_destroy = true
  }
}

resource "aws_s3_bucket_versioning" "terraform_state" {
  bucket = aws_s3_bucket.terraform_state.id
  
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "terraform_state" {
  bucket = aws_s3_bucket.terraform_state.id
  
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

# DynamoDB table for locking
resource "aws_dynamodb_table" "terraform_lock" {
  name           = "terraform-state-lock"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "LockID"
  
  attribute {
    name = "LockID"
    type = "S"
  }
}

output "s3_bucket_name" {
  value = aws_s3_bucket.terraform_state.id
}

output "dynamodb_table_name" {
  value = aws_dynamodb_table.terraform_lock.name
}
```

```bash
cd backend-setup
terraform init
terraform apply
cd ..
```

**Step 3: Add Backend Configuration**
```hcl
# main.tf (add backend block)
terraform {
  backend "s3" {
    bucket         = "my-terraform-state-bucket"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
  }
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

**Step 4: Migrate State**
```bash
# Initialize with new backend
terraform init -migrate-state

# Terraform will ask:
# Do you want to copy existing state to the new backend?
# Enter a value: yes
```

**Step 5: Verify**
```bash
# Check state is in S3
aws s3 ls s3://my-terraform-state-bucket/prod/

# Run plan to verify
terraform plan
```

---

## State Management Best Practices

1. **Always use remote state for teams**
2. **Enable state locking**
3. **Enable state file versioning**
4. **Encrypt state files**
5. **Never commit state files to Git**
6. **Use separate state files for different environments**
7. **Regularly backup state files**
8. **Use state file access controls**
9. **Be cautious with state commands (rm, mv)**
10. **Document state management procedures**

---

## Common State Issues and Solutions

### Issue 1: Corrupted State
```bash
# Restore from backup
terraform state pull > backup.tfstate
# Fix manually or restore from S3 version
```

### Issue 2: Stuck Lock
```bash
# Force unlock (use with caution)
terraform force-unlock <LOCK_ID>
```

### Issue 3: State Drift
```bash
# Detect drift
terraform plan -refresh-only

# Fix drift
terraform apply -refresh-only
```

### Issue 4: Lost State File
```bash
# If using S3 with versioning
aws s3api list-object-versions --bucket my-terraform-state --prefix prod/

# Restore previous version
aws s3api get-object \
  --bucket my-terraform-state \
  --key prod/terraform.tfstate \
  --version-id <VERSION_ID> \
  terraform.tfstate
```

# Phase 5: Expressions & Functions

## 1. Terraform Expressions

Expressions are used to compute values in Terraform configurations.

### Types of Expressions

```hcl
# Literal values
string_value  = "hello"
number_value  = 42
bool_value    = true
list_value    = [1, 2, 3]
map_value     = { key = "value" }

# References
var.instance_type
aws_instance.web.id
local.common_tags

# Operators
result = 10 + 5
comparison = var.count > 3
logical = var.enabled && var.approved

# Function calls
upper_name = upper(var.name)
subnet_cidr = cidrsubnet(var.vpc_cidr, 8, 1)

# Conditional expressions
instance_type = var.env == "prod" ? "t3.large" : "t3.micro"

# For expressions
instance_ids = [for i in aws_instance.web : i.id]
```

---

## 2. Conditional Expressions

Syntax: `condition ? true_val : false_val`

### Basic Conditionals

```hcl
variable "environment" {
  type = string
}

# Simple conditional
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.environment == "prod" ? "t3.large" : "t3.micro"
}

# Multiple conditions
locals {
  instance_type = (
    var.environment == "prod" ? "t3.large" :
    var.environment == "staging" ? "t3.medium" :
    "t3.micro"
  )
}

# Boolean conditional
resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  monitoring    = var.enable_monitoring ? true : false
}
```

### Conditional Resource Creation

```hcl
variable "create_database" {
  type    = bool
  default = false
}

# Create resource conditionally
resource "aws_db_instance" "main" {
  count = var.create_database ? 1 : 0
  
  identifier     = "mydb"
  engine         = "postgres"
  instance_class = "db.t3.micro"
  allocated_storage = 20
}

# Reference conditional resource
output "db_endpoint" {
  value = var.create_database ? aws_db_instance.main[0].endpoint : "No database created"
}
```

### Complex Conditionals

```hcl
locals {
  # Nested conditionals
  instance_config = var.environment == "prod" ? {
    instance_type = "t3.large"
    disk_size     = 100
    backup        = true
  } : var.environment == "staging" ? {
    instance_type = "t3.medium"
    disk_size     = 50
    backup        = true
  } : {
    instance_type = "t3.micro"
    disk_size     = 20
    backup        = false
  }
  
  # Conditional with multiple checks
  enable_feature = var.environment == "prod" && var.feature_flag == true
  
  # Conditional list
  availability_zones = var.multi_az ? ["us-east-1a", "us-east-1b", "us-east-1c"] : ["us-east-1a"]
}

resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = local.instance_config.instance_type
  
  root_block_device {
    volume_size = local.instance_config.disk_size
  }
  
  tags = {
    Name   = "AppServer"
    Backup = local.instance_config.backup ? "enabled" : "disabled"
  }
}
```

---

## 3. Built-in Functions

Terraform has 100+ built-in functions. Here are the most important ones:

### Numeric Functions

```hcl
# abs - Absolute value
abs(-10)  # Returns: 10

# ceil - Round up
ceil(5.1)  # Returns: 6

# floor - Round down
floor(5.9)  # Returns: 5

# max - Maximum value
max(5, 12, 9)  # Returns: 12

# min - Minimum value
min(5, 12, 9)  # Returns: 5

# pow - Power
pow(2, 3)  # Returns: 8

# Example usage
locals {
  disk_size = max(var.min_disk_size, 20)
  instances = ceil(var.expected_load / 100)
}
```

### String Functions

```hcl
# upper - Convert to uppercase
upper("hello")  # Returns: "HELLO"

# lower - Convert to lowercase
lower("HELLO")  # Returns: "hello"

# title - Title case
title("hello world")  # Returns: "Hello World"

# trim - Remove whitespace
trim("  hello  ")  # Returns: "hello"

# trimprefix - Remove prefix
trimprefix("hello-world", "hello-")  # Returns: "world"

# trimsuffix - Remove suffix
trimsuffix("hello-world", "-world")  # Returns: "hello"

# split - Split string
split(",", "a,b,c")  # Returns: ["a", "b", "c"]

# join - Join list
join(",", ["a", "b", "c"])  # Returns: "a,b,c"

# replace - Replace substring
replace("hello world", "world", "terraform")  # Returns: "hello terraform"

# substr - Substring
substr("hello", 0, 3)  # Returns: "hel"

# format - Format string
format("Hello, %s!", "World")  # Returns: "Hello, World!"

# Example usage
locals {
  bucket_name = lower(replace(var.project_name, " ", "-"))
  name_prefix = "${var.project}-${var.environment}"
  tags_string = join(",", [for k, v in var.tags : "${k}=${v}"])
}
```

### Collection Functions

#### length
```hcl
# length - Get length of list, map, or string
length([1, 2, 3])              # Returns: 3
length({a = 1, b = 2})         # Returns: 2
length("hello")                # Returns: 5

# Example usage
resource "aws_subnet" "public" {
  count      = length(var.availability_zones)
  vpc_id     = aws_vpc.main.id
  cidr_block = cidrsubnet(var.vpc_cidr, 8, count.index)
}
```

#### join
```hcl
# join - Join list elements with separator
join(", ", ["apple", "banana", "cherry"])  # Returns: "apple, banana, cherry"

# Example usage
locals {
  security_groups = join(",", aws_security_group.web[*].id)
  
  user_data = <<-EOF
    #!/bin/bash
    echo "Servers: ${join(" ", aws_instance.web[*].private_ip)}"
  EOF
}
```

#### lookup
```hcl
# lookup - Get value from map with default
variable "instance_types" {
  type = map(string)
  default = {
    dev  = "t3.micro"
    prod = "t3.large"
  }
}

# lookup(map, key, default)
instance_type = lookup(var.instance_types, var.environment, "t3.small")

# Example: Returns "t3.micro" for dev, "t3.large" for prod, "t3.small" for others
```

#### merge
```hcl
# merge - Merge multiple maps
merge(
  { a = 1, b = 2 },
  { b = 3, c = 4 }
)  # Returns: { a = 1, b = 3, c = 4 }

# Example usage
locals {
  common_tags = {
    ManagedBy = "Terraform"
    Project   = var.project
  }
  
  env_tags = {
    Environment = var.environment
  }
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  tags = merge(
    local.common_tags,
    local.env_tags,
    {
      Name = "WebServer"
    }
  )
}
```

#### element
```hcl
# element - Get element from list (wraps around)
element(["a", "b", "c"], 0)  # Returns: "a"
element(["a", "b", "c"], 3)  # Returns: "a" (wraps around)

# Example usage
resource "aws_instance" "web" {
  count             = 5
  ami               = "ami-0c55b159cbfafe1f0"
  instance_type     = "t3.micro"
  availability_zone = element(var.availability_zones, count.index)
}
```

#### flatten
```hcl
# flatten - Flatten nested lists
flatten([["a", "b"], ["c", "d"]])  # Returns: ["a", "b", "c", "d"]

# Example usage
locals {
  subnet_cidrs = [
    ["10.0.1.0/24", "10.0.2.0/24"],
    ["10.0.3.0/24", "10.0.4.0/24"]
  ]
  
  all_cidrs = flatten(local.subnet_cidrs)
  # Returns: ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24", "10.0.4.0/24"]
}
```

### More Useful Functions

```hcl
# concat - Concatenate lists
concat([1, 2], [3, 4])  # Returns: [1, 2, 3, 4]

# contains - Check if list contains value
contains(["a", "b", "c"], "b")  # Returns: true

# distinct - Remove duplicates
distinct([1, 2, 2, 3, 3, 3])  # Returns: [1, 2, 3]

# keys - Get map keys
keys({a = 1, b = 2})  # Returns: ["a", "b"]

# values - Get map values
values({a = 1, b = 2})  # Returns: [1, 2]

# reverse - Reverse list
reverse([1, 2, 3])  # Returns: [3, 2, 1]

# sort - Sort list
sort(["c", "a", "b"])  # Returns: ["a", "b", "c"]

# slice - Get slice of list
slice([1, 2, 3, 4, 5], 1, 4)  # Returns: [2, 3, 4]

# zipmap - Create map from two lists
zipmap(["a", "b"], [1, 2])  # Returns: {a = 1, b = 2}
```

### File Functions

```hcl
# file - Read file content
file("${path.module}/config.txt")

# filebase64 - Read file as base64
filebase64("${path.module}/image.png")

# templatefile - Render template file
templatefile("${path.module}/user_data.sh", {
  server_name = "web-server"
  port        = 8080
})

# Example usage
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  user_data = templatefile("${path.module}/user_data.sh", {
    environment = var.environment
    app_version = var.app_version
  })
}
```

### Encoding Functions

```hcl
# base64encode - Encode to base64
base64encode("hello")  # Returns: "aGVsbG8="

# base64decode - Decode from base64
base64decode("aGVsbG8=")  # Returns: "hello"

# jsonencode - Encode to JSON
jsonencode({name = "John", age = 30})  # Returns: '{"name":"John","age":30}'

# jsondecode - Decode from JSON
jsondecode('{"name":"John"}')  # Returns: {name = "John"}

# yamlencode - Encode to YAML
yamlencode({name = "John", age = 30})

# yamldecode - Decode from YAML
yamldecode("name: John\nage: 30")

# Example usage
resource "aws_iam_policy" "example" {
  name = "example-policy"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Action = ["s3:*"]
      Resource = "*"
    }]
  })
}
```

### Network Functions

```hcl
# cidrhost - Get IP address from CIDR
cidrhost("10.0.0.0/24", 5)  # Returns: "10.0.0.5"

# cidrnetmask - Get netmask from CIDR
cidrnetmask("10.0.0.0/24")  # Returns: "255.255.255.0"

# cidrsubnet - Calculate subnet
cidrsubnet("10.0.0.0/16", 8, 1)  # Returns: "10.0.1.0/24"

# cidrsubnets - Calculate multiple subnets
cidrsubnets("10.0.0.0/16", 8, 8, 8)  # Returns: ["10.0.0.0/24", "10.0.1.0/24", "10.0.2.0/24"]

# Example usage
locals {
  vpc_cidr = "10.0.0.0/16"
  
  public_subnets = [
    cidrsubnet(local.vpc_cidr, 8, 1),  # 10.0.1.0/24
    cidrsubnet(local.vpc_cidr, 8, 2),  # 10.0.2.0/24
    cidrsubnet(local.vpc_cidr, 8, 3),  # 10.0.3.0/24
  ]
  
  private_subnets = [
    cidrsubnet(local.vpc_cidr, 8, 11),  # 10.0.11.0/24
    cidrsubnet(local.vpc_cidr, 8, 12),  # 10.0.12.0/24
    cidrsubnet(local.vpc_cidr, 8, 13),  # 10.0.13.0/24
  ]
}
```

### Type Conversion Functions

```hcl
# tostring - Convert to string
tostring(42)  # Returns: "42"

# tonumber - Convert to number
tonumber("42")  # Returns: 42

# tobool - Convert to boolean
tobool("true")  # Returns: true

# tolist - Convert to list
tolist(["a", "b"])  # Returns: ["a", "b"]

# toset - Convert to set
toset(["a", "b", "a"])  # Returns: ["a", "b"]

# tomap - Convert to map
tomap({a = 1, b = 2})  # Returns: {a = 1, b = 2}

# Example usage
variable "port" {
  type = string
  default = "8080"
}

resource "aws_security_group_rule" "app" {
  type        = "ingress"
  from_port   = tonumber(var.port)
  to_port     = tonumber(var.port)
  protocol    = "tcp"
  cidr_blocks = ["0.0.0.0/0"]
  security_group_id = aws_security_group.app.id
}
```

---

## 4. Dynamic Blocks

Dynamic blocks allow you to dynamically construct repeatable nested blocks.

### Basic Dynamic Block

```hcl
# Without dynamic block (repetitive)
resource "aws_security_group" "web" {
  name = "web-sg"
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["10.0.0.0/8"]
  }
}

# With dynamic block (cleaner)
variable "ingress_rules" {
  type = list(object({
    from_port   = number
    to_port     = number
    protocol    = string
    cidr_blocks = list(string)
  }))
  
  default = [
    {
      from_port   = 80
      to_port     = 80
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    {
      from_port   = 443
      to_port     = 443
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    {
      from_port   = 22
      to_port     = 22
      protocol    = "tcp"
      cidr_blocks = ["10.0.0.0/8"]
    }
  ]
}

resource "aws_security_group" "web" {
  name = "web-sg"
  
  dynamic "ingress" {
    for_each = var.ingress_rules
    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }
}
```

### Dynamic Block with Map

```hcl
variable "ports" {
  type = map(object({
    port        = number
    description = string
  }))
  
  default = {
    http = {
      port        = 80
      description = "HTTP traffic"
    }
    https = {
      port        = 443
      description = "HTTPS traffic"
    }
    ssh = {
      port        = 22
      description = "SSH access"
    }
  }
}

resource "aws_security_group" "web" {
  name = "web-sg"
  
  dynamic "ingress" {
    for_each = var.ports
    content {
      description = ingress.value.description
      from_port   = ingress.value.port
      to_port     = ingress.value.port
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}
```

### Nested Dynamic Blocks

```hcl
variable "load_balancers" {
  type = map(object({
    port     = number
    protocol = string
    health_checks = list(object({
      path     = string
      interval = number
    }))
  }))
  
  default = {
    web = {
      port     = 80
      protocol = "HTTP"
      health_checks = [
        { path = "/health", interval = 30 },
        { path = "/ready", interval = 60 }
      ]
    }
  }
}

resource "aws_lb_target_group" "app" {
  for_each = var.load_balancers
  
  name     = "${each.key}-tg"
  port     = each.value.port
  protocol = each.value.protocol
  vpc_id   = aws_vpc.main.id
  
  dynamic "health_check" {
    for_each = each.value.health_checks
    content {
      path                = health_check.value.path
      interval            = health_check.value.interval
      healthy_threshold   = 2
      unhealthy_threshold = 2
    }
  }
}
```

### Dynamic Block with Conditional

```hcl
variable "enable_monitoring" {
  type    = bool
  default = false
}

variable "monitoring_config" {
  type = object({
    interval = number
    timeout  = number
  })
  default = {
    interval = 30
    timeout  = 5
  }
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  # Only create monitoring block if enabled
  dynamic "monitoring" {
    for_each = var.enable_monitoring ? [1] : []
    content {
      enabled = true
    }
  }
  
  dynamic "credit_specification" {
    for_each = var.enable_monitoring ? [var.monitoring_config] : []
    content {
      cpu_credits = "unlimited"
    }
  }
}
```

---

## 5. for Expressions

For expressions transform one collection into another.

### List Transformation

```hcl
# Transform list
variable "names" {
  type    = list(string)
  default = ["alice", "bob", "charlie"]
}

# Create uppercase list
locals {
  upper_names = [for name in var.names : upper(name)]
  # Result: ["ALICE", "BOB", "CHARLIE"]
}

# Filter and transform
locals {
  long_names = [for name in var.names : upper(name) if length(name) > 3]
  # Result: ["ALICE", "CHARLIE"]
}

# Create objects from list
locals {
  user_objects = [
    for name in var.names : {
      username = name
      email    = "${name}@example.com"
    }
  ]
  # Result: [
  #   {username = "alice", email = "alice@example.com"},
  #   {username = "bob", email = "bob@example.com"},
  #   ...
  # ]
}
```

### Map Transformation

```hcl
variable "instances" {
  type = map(string)
  default = {
    web = "t3.micro"
    app = "t3.small"
    db  = "t3.medium"
  }
}

# Transform map to map
locals {
  instance_arns = {
    for name, type in var.instances :
    name => "arn:aws:ec2:us-east-1:123456789012:instance/${name}"
  }
}

# Transform map to list
locals {
  instance_list = [
    for name, type in var.instances : {
      name = name
      type = type
    }
  ]
}

# Filter map
locals {
  small_instances = {
    for name, type in var.instances :
    name => type
    if type == "t3.micro" || type == "t3.small"
  }
}
```

### Complex for Expressions

```hcl
# Flatten nested structures
variable "regions" {
  type = map(list(string))
  default = {
    us-east-1 = ["subnet-1", "subnet-2"]
    us-west-2 = ["subnet-3", "subnet-4"]
  }
}

locals {
  all_subnets = flatten([
    for region, subnets in var.regions : [
      for subnet in subnets : {
        region = region
        subnet = subnet
      }
    ]
  ])
  # Result: [
  #   {region = "us-east-1", subnet = "subnet-1"},
  #   {region = "us-east-1", subnet = "subnet-2"},
  #   {region = "us-west-2", subnet = "subnet-3"},
  #   {region = "us-west-2", subnet = "subnet-4"}
  # ]
}

# Group by attribute
locals {
  instances_by_type = {
    for inst in aws_instance.web : inst.instance_type => inst.id...
  }
}
```

### Practical Examples

```hcl
# Create security group rules from list
variable "allowed_ports" {
  type    = list(number)
  default = [80, 443, 22]
}

resource "aws_security_group" "web" {
  name = "web-sg"
  
  dynamic "ingress" {
    for_each = var.allowed_ports
    content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}

# Create tags from map
variable "tags" {
  type = map(string)
  default = {
    Environment = "Production"
    Project     = "WebApp"
    Team        = "DevOps"
  }
}

locals {
  tag_list = [for key, value in var.tags : "${key}=${value}"]
  # Result: ["Environment=Production", "Project=WebApp", "Team=DevOps"]
}

# Create instances in multiple AZs
variable "availability_zones" {
  type    = list(string)
  default = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

resource "aws_instance" "web" {
  for_each = toset(var.availability_zones)
  
  ami               = "ami-0c55b159cbfafe1f0"
  instance_type     = "t3.micro"
  availability_zone = each.value
  
  tags = {
    Name = "web-${each.value}"
  }
}

output "instance_ips" {
  value = {
    for az, instance in aws_instance.web :
    az => instance.private_ip
  }
}
```

---

## 6. count vs for_each (Interview Favorite)

### count

**Use when:**
- Creating identical resources
- Number of resources is known
- Simple indexing is sufficient

```hcl
# Good use of count
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  tags = {
    Name = "web-${count.index + 1}"
  }
}

# Conditional creation with count
resource "aws_instance" "optional" {
  count         = var.create_instance ? 1 : 0
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
}
```

**Problems with count:**
```hcl
# BAD: Order-dependent
variable "users" {
  type    = list(string)
  default = ["alice", "bob", "charlie"]
}

resource "aws_iam_user" "users" {
  count = length(var.users)
  name  = var.users[count.index]
}

# If you remove "bob", "charlie" becomes index 1
# This causes Terraform to:
# - Destroy charlie (was index 2, now doesn't exist)
# - Recreate charlie at index 1
# This is NOT what you want!
```

### for_each

**Use when:**
- Each resource has unique configuration
- Resources need stable identifiers
- Adding/removing shouldn't affect others

```hcl
# Good use of for_each
variable "users" {
  type    = set(string)
  default = ["alice", "bob", "charlie"]
}

resource "aws_iam_user" "users" {
  for_each = var.users
  name     = each.value
}

# Removing "bob" only affects "bob"
# "alice" and "charlie" are unaffected
```

### Comparison Example

```hcl
# Using count (BAD for this use case)
variable "instances_count" {
  type = map(string)
  default = {
    web = "t3.micro"
    app = "t3.small"
    db  = "t3.medium"
  }
}

locals {
  instance_names = keys(var.instances_count)
  instance_types = values(var.instances_count)
}

resource "aws_instance" "bad_example" {
  count         = length(var.instances_count)
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = local.instance_types[count.index]
  
  tags = {
    Name = local.instance_names[count.index]
  }
}

# Using for_each (GOOD)
resource "aws_instance" "good_example" {
  for_each      = var.instances_count
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = each.value
  
  tags = {
    Name = each.key
  }
}
```

### When to Use Each

| Scenario | Use count | Use for_each |
|----------|-----------|--------------|
| Creating N identical resources | ✅ | ❌ |
| Conditional resource creation | ✅ | ❌ |
| Resources with unique configs | ❌ | ✅ |
| Resources from map/set | ❌ | ✅ |
| Order matters | ✅ | ❌ |
| Stable identifiers needed | ❌ | ✅ |

---

## Complete Example: Advanced Expressions

```hcl
# variables.tf
variable "environment" {
  type = string
}

variable "regions" {
  type = map(object({
    vpc_cidr = string
    azs      = list(string)
  }))
  
  default = {
    us-east-1 = {
      vpc_cidr = "10.0.0.0/16"
      azs      = ["us-east-1a", "us-east-1b", "us-east-1c"]
    }
    us-west-2 = {
      vpc_cidr = "10.1.0.0/16"
      azs      = ["us-west-2a", "us-west-2b"]
    }
  }
}

variable "instance_configs" {
  type = map(object({
    instance_type = string
    count         = number
    ports         = list(number)
  }))
  
  default = {
    web = {
      instance_type = "t3.micro"
      count         = 2
      ports         = [80, 443]
    }
    app = {
      instance_type = "t3.small"
      count         = 3
      ports         = [8080, 8443]
    }
  }
}

# locals.tf
locals {
  # Conditional instance type based on environment
  instance_types = {
    for name, config in var.instance_configs :
    name => var.environment == "prod" ? "t3.large" : config.instance_type
  }
  
  # Calculate all subnet CIDRs
  subnets = flatten([
    for region, config in var.regions : [
      for idx, az in config.azs : {
        region    = region
        az        = az
        cidr      = cidrsubnet(config.vpc_cidr, 8, idx)
        is_public = idx < 2  # First 2 are public
      }
    ]
  ])
  
  # Group subnets by region and type
  public_subnets = {
    for subnet in local.subnets :
    "${subnet.region}-${subnet.az}" => subnet
    if subnet.is_public
  }
  
  private_subnets = {
    for subnet in local.subnets :
    "${subnet.region}-${subnet.az}" => subnet
    if !subnet.is_public
  }
  
  # Create security group rules
  all_sg_rules = flatten([
    for name, config in var.instance_configs : [
      for port in config.ports : {
        name      = name
        port      = port
        rule_name = "${name}-${port}"
      }
    ]
  ])
  
  # Common tags
  common_tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
    CreatedAt   = formatdate("YYYY-MM-DD", timestamp())
  }
}

# main.tf
resource "aws_vpc" "main" {
  for_each = var.regions
  
  cidr_block           = each.value.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = merge(
    local.common_tags,
    {
      Name   = "${var.environment}-${each.key}-vpc"
      Region = each.key
    }
  )
}

resource "aws_subnet" "public" {
  for_each = local.public_subnets
  
  vpc_id                  = aws_vpc.main[each.value.region].id
  cidr_block              = each.value.cidr
  availability_zone       = each.value.az
  map_public_ip_on_launch = true
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-${each.key}-public"
      Type = "Public"
    }
  )
}

resource "aws_security_group" "app" {
  for_each = var.instance_configs
  
  name        = "${var.environment}-${each.key}-sg"
  description = "Security group for ${each.key}"
  vpc_id      = aws_vpc.main["us-east-1"].id
  
  dynamic "ingress" {
    for_each = each.value.ports
    content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
      description = "Allow ${ingress.value}"
    }
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-${each.key}-sg"
      Type = each.key
    }
  )
}

# outputs.tf
output "vpc_cidrs" {
  value = {
    for region, vpc in aws_vpc.main :
    region => vpc.cidr_block
  }
}

output "public_subnet_ids" {
  value = [for subnet in aws_subnet.public : subnet.id]
}

output "security_group_ids" {
  value = {
    for name, sg in aws_security_group.app :
    name => sg.id
  }
}

output "all_rules" {
  value = [
    for rule in local.all_sg_rules :
    "${rule.name}: ${rule.port}"
  ]
}
```

---

## Best Practices

1. **Use for_each over count for maps/sets**
2. **Use dynamic blocks for repeatable nested blocks**
3. **Use locals for complex expressions**
4. **Use built-in functions instead of external scripts**
5. **Use conditional expressions for simple logic**
6. **Keep expressions readable (split complex ones)**
7. **Use type conversion functions explicitly**
8. **Test expressions with terraform console**

---

## Testing Expressions

```bash
# Start Terraform console
terraform console

# Test expressions
> upper("hello")
"HELLO"

> cidrsubnet("10.0.0.0/16", 8, 1)
"10.0.1.0/24"

> [for i in [1, 2, 3] : i * 2]
[2, 4, 6]

> merge({a = 1}, {b = 2})
{
  "a" = 1
  "b" = 2
}
```

# Phase 6: Modules (MOST IMPORTANT)

## 1. What are Modules?

Modules are containers for multiple resources that are used together. Every Terraform configuration has at least one module, called the **root module**.

### Benefits of Modules

- **Reusability**: Write once, use many times
- **Organization**: Group related resources
- **Encapsulation**: Hide complexity
- **Versioning**: Track changes over time
- **Collaboration**: Share with team/community

---

## 2. Root Module

The root module is the working directory where you run Terraform commands.

```
my-project/
├── main.tf          # Root module
├── variables.tf     # Root module
├── outputs.tf       # Root module
└── terraform.tfvars # Root module
```

---

## 3. Child Modules

Child modules are modules called by the root module or other modules.

### Calling a Module

```hcl
# Root module: main.tf
module "vpc" {
  source = "./modules/vpc"
  
  # Input variables
  vpc_cidr    = "10.0.0.0/16"
  environment = "production"
}

# Use module outputs
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  subnet_id     = module.vpc.public_subnet_ids[0]
}
```

---

## 4. Module Structure Best Practices

### Standard Module Structure

```
modules/
└── vpc/
    ├── main.tf          # Main resources
    ├── variables.tf     # Input variables
    ├── outputs.tf       # Output values
    ├── README.md        # Documentation
    ├── versions.tf      # Provider requirements
    └── examples/        # Usage examples
        └── basic/
            ├── main.tf
            └── variables.tf
```

### Complete Example: VPC Module

**modules/vpc/variables.tf**
```hcl
variable "vpc_cidr" {
  description = "CIDR block for VPC"
  type        = string
}

variable "environment" {
  description = "Environment name"
  type        = string
}

variable "availability_zones" {
  description = "List of availability zones"
  type        = list(string)
  default     = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

variable "public_subnet_count" {
  description = "Number of public subnets"
  type        = number
  default     = 3
}

variable "private_subnet_count" {
  description = "Number of private subnets"
  type        = number
  default     = 3
}

variable "enable_nat_gateway" {
  description = "Enable NAT Gateway for private subnets"
  type        = bool
  default     = true
}

variable "tags" {
  description = "Additional tags"
  type        = map(string)
  default     = {}
}
```

**modules/vpc/main.tf**
```hcl
locals {
  common_tags = merge(
    {
      Environment = var.environment
      ManagedBy   = "Terraform"
      Module      = "vpc"
    },
    var.tags
  )
}

# VPC
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-vpc"
    }
  )
}

# Internet Gateway
resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-igw"
    }
  )
}

# Public Subnets
resource "aws_subnet" "public" {
  count = var.public_subnet_count
  
  vpc_id                  = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 8, count.index)
  availability_zone       = var.availability_zones[count.index % length(var.availability_zones)]
  map_public_ip_on_launch = true
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-public-${count.index + 1}"
      Type = "Public"
    }
  )
}

# Private Subnets
resource "aws_subnet" "private" {
  count = var.private_subnet_count
  
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet(var.vpc_cidr, 8, count.index + 10)
  availability_zone = var.availability_zones[count.index % length(var.availability_zones)]
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-private-${count.index + 1}"
      Type = "Private"
    }
  )
}

# Elastic IPs for NAT Gateways
resource "aws_eip" "nat" {
  count = var.enable_nat_gateway ? var.public_subnet_count : 0
  
  domain = "vpc"
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-nat-eip-${count.index + 1}"
    }
  )
  
  depends_on = [aws_internet_gateway.main]
}

# NAT Gateways
resource "aws_nat_gateway" "main" {
  count = var.enable_nat_gateway ? var.public_subnet_count : 0
  
  allocation_id = aws_eip.nat[count.index].id
  subnet_id     = aws_subnet.public[count.index].id
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-nat-${count.index + 1}"
    }
  )
}

# Public Route Table
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
  
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-public-rt"
    }
  )
}

# Public Route Table Association
resource "aws_route_table_association" "public" {
  count = var.public_subnet_count
  
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}

# Private Route Tables
resource "aws_route_table" "private" {
  count = var.enable_nat_gateway ? var.private_subnet_count : 1
  
  vpc_id = aws_vpc.main.id
  
  dynamic "route" {
    for_each = var.enable_nat_gateway ? [1] : []
    content {
      cidr_block     = "0.0.0.0/0"
      nat_gateway_id = aws_nat_gateway.main[count.index % length(aws_nat_gateway.main)].id
    }
  }
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.environment}-private-rt-${count.index + 1}"
    }
  )
}

# Private Route Table Association
resource "aws_route_table_association" "private" {
  count = var.private_subnet_count
  
  subnet_id      = aws_subnet.private[count.index].id
  route_table_id = aws_route_table.private[var.enable_nat_gateway ? count.index % length(aws_route_table.private) : 0].id
}
```

**modules/vpc/outputs.tf**
```hcl
output "vpc_id" {
  description = "VPC ID"
  value       = aws_vpc.main.id
}

output "vpc_cidr" {
  description = "VPC CIDR block"
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

output "public_subnet_cidrs" {
  description = "List of public subnet CIDR blocks"
  value       = aws_subnet.public[*].cidr_block
}

output "private_subnet_cidrs" {
  description = "List of private subnet CIDR blocks"
  value       = aws_subnet.private[*].cidr_block
}

output "internet_gateway_id" {
  description = "Internet Gateway ID"
  value       = aws_internet_gateway.main.id
}

output "nat_gateway_ids" {
  description = "List of NAT Gateway IDs"
  value       = aws_nat_gateway.main[*].id
}

output "nat_gateway_ips" {
  description = "List of NAT Gateway public IPs"
  value       = aws_eip.nat[*].public_ip
}
```

**modules/vpc/versions.tf**
```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = ">= 5.0"
    }
  }
}
```

**modules/vpc/README.md**
```markdown
# VPC Module

This module creates a VPC with public and private subnets across multiple availability zones.

## Features

- VPC with customizable CIDR
- Public and private subnets
- Internet Gateway
- NAT Gateways (optional)
- Route tables and associations

## Usage

```hcl
module "vpc" {
  source = "./modules/vpc"
  
  vpc_cidr             = "10.0.0.0/16"
  environment          = "production"
  availability_zones   = ["us-east-1a", "us-east-1b", "us-east-1c"]
  public_subnet_count  = 3
  private_subnet_count = 3
  enable_nat_gateway   = true
  
  tags = {
    Project = "MyApp"
  }
}
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|----------|
| vpc_cidr | CIDR block for VPC | string | n/a | yes |
| environment | Environment name | string | n/a | yes |
| availability_zones | List of AZs | list(string) | ["us-east-1a", "us-east-1b", "us-east-1c"] | no |
| public_subnet_count | Number of public subnets | number | 3 | no |
| private_subnet_count | Number of private subnets | number | 3 | no |
| enable_nat_gateway | Enable NAT Gateway | bool | true | no |
| tags | Additional tags | map(string) | {} | no |

## Outputs

| Name | Description |
|------|-------------|
| vpc_id | VPC ID |
| public_subnet_ids | List of public subnet IDs |
| private_subnet_ids | List of private subnet IDs |
| nat_gateway_ids | List of NAT Gateway IDs |
```

---

## 5. Module Inputs

Input variables are parameters for modules.

```hcl
# Module definition
module "vpc" {
  source = "./modules/vpc"
  
  # Required inputs
  vpc_cidr    = "10.0.0.0/16"
  environment = "production"
  
  # Optional inputs (have defaults)
  enable_nat_gateway = true
  
  # Complex inputs
  tags = {
    Project = "MyApp"
    Team    = "DevOps"
  }
}
```

---

## 6. Module Outputs

Output values expose resource attributes from modules.

```hcl
# In module: outputs.tf
output "vpc_id" {
  description = "VPC ID"
  value       = aws_vpc.main.id
}

# In root module: use module outputs
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  subnet_id     = module.vpc.public_subnet_ids[0]  # Use module output
  
  tags = {
    Name = "WebServer"
    VPC  = module.vpc.vpc_id  # Use module output
  }
}

# Output module outputs
output "vpc_id" {
  value = module.vpc.vpc_id
}
```

---

## 7. Module Versioning

### Local Modules

```hcl
# Relative path
module "vpc" {
  source = "./modules/vpc"
}

# Absolute path
module "vpc" {
  source = "/home/user/terraform/modules/vpc"
}
```

### Git Modules

```hcl
# Git over HTTPS
module "vpc" {
  source = "git::https://github.com/username/terraform-modules.git//vpc"
}

# Git over SSH
module "vpc" {
  source = "git::ssh://git@github.com/username/terraform-modules.git//vpc"
}

# Specific branch
module "vpc" {
  source = "git::https://github.com/username/terraform-modules.git//vpc?ref=main"
}

# Specific tag
module "vpc" {
  source = "git::https://github.com/username/terraform-modules.git//vpc?ref=v1.0.0"
}

# Specific commit
module "vpc" {
  source = "git::https://github.com/username/terraform-modules.git//vpc?ref=abc123"
}
```

### Terraform Registry

```hcl
# Public registry
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.1.0"
  
  name = "my-vpc"
  cidr = "10.0.0.0/16"
}

# Private registry
module "vpc" {
  source  = "app.terraform.io/my-org/vpc/aws"
  version = "1.0.0"
}
```

---

## 8. Reusable Modules

### Creating a Reusable Module

**modules/web-server/variables.tf**
```hcl
variable "name" {
  description = "Name of the web server"
  type        = string
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t3.micro"
}

variable "subnet_id" {
  description = "Subnet ID"
  type        = string
}

variable "vpc_id" {
  description = "VPC ID"
  type        = string
}

variable "allowed_cidr_blocks" {
  description = "CIDR blocks allowed to access"
  type        = list(string)
  default     = ["0.0.0.0/0"]
}

variable "user_data" {
  description = "User data script"
  type        = string
  default     = ""
}

variable "tags" {
  description = "Additional tags"
  type        = map(string)
  default     = {}
}
```

**modules/web-server/main.tf**
```hcl
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

resource "aws_security_group" "web" {
  name        = "${var.name}-sg"
  description = "Security group for ${var.name}"
  vpc_id      = var.vpc_id
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = var.allowed_cidr_blocks
  }
  
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = var.allowed_cidr_blocks
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = merge(
    var.tags,
    {
      Name = "${var.name}-sg"
    }
  )
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  subnet_id     = var.subnet_id
  
  vpc_security_group_ids = [aws_security_group.web.id]
  
  user_data = var.user_data != "" ? var.user_data : <<-EOF
    #!/bin/bash
    apt-get update
    apt-get install -y nginx
    systemctl start nginx
    systemctl enable nginx
  EOF
  
  tags = merge(
    var.tags,
    {
      Name = var.name
    }
  )
}
```

**modules/web-server/outputs.tf**
```hcl
output "instance_id" {
  description = "EC2 instance ID"
  value       = aws_instance.web.id
}

output "public_ip" {
  description = "Public IP address"
  value       = aws_instance.web.public_ip
}

output "private_ip" {
  description = "Private IP address"
  value       = aws_instance.web.private_ip
}

output "security_group_id" {
  description = "Security group ID"
  value       = aws_security_group.web.id
}
```

### Using the Reusable Module

```hcl
# main.tf
module "vpc" {
  source = "./modules/vpc"
  
  vpc_cidr    = "10.0.0.0/16"
  environment = "production"
}

module "web_server_1" {
  source = "./modules/web-server"
  
  name        = "web-server-1"
  instance_type = "t3.micro"
  subnet_id   = module.vpc.public_subnet_ids[0]
  vpc_id      = module.vpc.vpc_id
  
  tags = {
    Environment = "production"
    Role        = "web"
  }
}

module "web_server_2" {
  source = "./modules/web-server"
  
  name        = "web-server-2"
  instance_type = "t3.small"
  subnet_id   = module.vpc.public_subnet_ids[1]
  vpc_id      = module.vpc.vpc_id
  
  tags = {
    Environment = "production"
    Role        = "web"
  }
}

output "web_server_1_ip" {
  value = module.web_server_1.public_ip
}

output "web_server_2_ip" {
  value = module.web_server_2.public_ip
}
```

---

## 9. Public Modules (Terraform Registry)

### Using Public Modules

```hcl
# AWS VPC Module
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.1.0"
  
  name = "my-vpc"
  cidr = "10.0.0.0/16"
  
  azs             = ["us-east-1a", "us-east-1b", "us-east-1c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]
  
  enable_nat_gateway = true
  enable_vpn_gateway = false
  
  tags = {
    Environment = "production"
  }
}

# AWS EKS Module
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "19.16.0"
  
  cluster_name    = "my-cluster"
  cluster_version = "1.27"
  
  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets
  
  eks_managed_node_groups = {
    general = {
      desired_size = 2
      min_size     = 1
      max_size     = 3
      
      instance_types = ["t3.medium"]
    }
  }
}

# AWS RDS Module
module "db" {
  source  = "terraform-aws-modules/rds/aws"
  version = "6.1.0"
  
  identifier = "mydb"
  
  engine            = "postgres"
  engine_version    = "14.7"
  instance_class    = "db.t3.micro"
  allocated_storage = 20
  
  db_name  = "myapp"
  username = "admin"
  password = var.db_password
  
  vpc_security_group_ids = [aws_security_group.db.id]
  
  subnet_ids = module.vpc.private_subnets
}
```

---

## 10. Private Modules

### Terraform Cloud/Enterprise

```hcl
module "vpc" {
  source  = "app.terraform.io/my-org/vpc/aws"
  version = "1.0.0"
  
  vpc_cidr = "10.0.0.0/16"
}
```

### Private Git Repository

```hcl
module "vpc" {
  source = "git::ssh://git@github.com/my-org/terraform-modules.git//vpc?ref=v1.0.0"
  
  vpc_cidr = "10.0.0.0/16"
}
```

---

## 11. Module Composition

Modules can call other modules.

```hcl
# modules/app-stack/main.tf
module "vpc" {
  source = "../vpc"
  
  vpc_cidr    = var.vpc_cidr
  environment = var.environment
}

module "web_servers" {
  source = "../web-server"
  count  = var.web_server_count
  
  name      = "web-${count.index + 1}"
  subnet_id = module.vpc.public_subnet_ids[count.index % length(module.vpc.public_subnet_ids)]
  vpc_id    = module.vpc.vpc_id
}

module "database" {
  source = "../rds"
  
  db_name   = var.db_name
  subnet_ids = module.vpc.private_subnet_ids
  vpc_id    = module.vpc.vpc_id
}
```

---

## 12. Nested Modules

```
modules/
├── app-infrastructure/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── modules/
│       ├── networking/
│       │   ├── main.tf
│       │   ├── variables.tf
│       │   └── outputs.tf
│       ├── compute/
│       │   ├── main.tf
│       │   ├── variables.tf
│       │   └── outputs.tf
│       └── database/
│           ├── main.tf
│           ├── variables.tf
│           └── outputs.tf
```

**modules/app-infrastructure/main.tf**
```hcl
module "networking" {
  source = "./modules/networking"
  
  vpc_cidr    = var.vpc_cidr
  environment = var.environment
}

module "compute" {
  source = "./modules/compute"
  
  vpc_id     = module.networking.vpc_id
  subnet_ids = module.networking.public_subnet_ids
  
  instance_count = var.instance_count
  instance_type  = var.instance_type
}

module "database" {
  source = "./modules/database"
  
  vpc_id     = module.networking.vpc_id
  subnet_ids = module.networking.private_subnet_ids
  
  db_name     = var.db_name
  db_username = var.db_username
  db_password = var.db_password
}
```

---

## Complete Example: Multi-Tier Application

```
project/
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
└── modules/
    ├── vpc/
    ├── web-server/
    ├── app-server/
    └── database/
```

**main.tf**
```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

# VPC Module
module "vpc" {
  source = "./modules/vpc"
  
  vpc_cidr             = var.vpc_cidr
  environment          = var.environment
  availability_zones   = var.availability_zones
  public_subnet_count  = 3
  private_subnet_count = 3
  enable_nat_gateway   = true
  
  tags = local.common_tags
}

# Web Server Module
module "web_servers" {
  source = "./modules/web-server"
  count  = var.web_server_count
  
  name        = "${var.environment}-web-${count.index + 1}"
  instance_type = var.web_instance_type
  subnet_id   = module.vpc.public_subnet_ids[count.index % length(module.vpc.public_subnet_ids)]
  vpc_id      = module.vpc.vpc_id
  
  user_data = templatefile("${path.module}/templates/web_user_data.sh", {
    app_servers = module.app_servers[*].private_ip
  })
  
  tags = merge(
    local.common_tags,
    {
      Tier = "Web"
    }
  )
}

# App Server Module
module "app_servers" {
  source = "./modules/app-server"
  count  = var.app_server_count
  
  name        = "${var.environment}-app-${count.index + 1}"
  instance_type = var.app_instance_type
  subnet_id   = module.vpc.private_subnet_ids[count.index % length(module.vpc.private_subnet_ids)]
  vpc_id      = module.vpc.vpc_id
  
  user_data = templatefile("${path.module}/templates/app_user_data.sh", {
    db_endpoint = module.database.endpoint
  })
  
  tags = merge(
    local.common_tags,
    {
      Tier = "Application"
    }
  )
}

# Database Module
module "database" {
  source = "./modules/database"
  
  identifier     = "${var.environment}-db"
  engine         = "postgres"
  engine_version = "14.7"
  instance_class = var.db_instance_class
  
  db_name     = var.db_name
  db_username = var.db_username
  db_password = var.db_password
  
  subnet_ids = module.vpc.private_subnet_ids
  vpc_id     = module.vpc.vpc_id
  
  allowed_cidr_blocks = module.vpc.private_subnet_cidrs
  
  tags = merge(
    local.common_tags,
    {
      Tier = "Database"
    }
  )
}

locals {
  common_tags = {
    Project     = var.project
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}
```

**variables.tf**
```hcl
variable "region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "project" {
  description = "Project name"
  type        = string
}

variable "environment" {
  description = "Environment name"
  type        = string
}

variable "vpc_cidr" {
  description = "VPC CIDR block"
  type        = string
  default     = "10.0.0.0/16"
}

variable "availability_zones" {
  description = "Availability zones"
  type        = list(string)
  default     = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

variable "web_server_count" {
  description = "Number of web servers"
  type        = number
  default     = 2
}

variable "web_instance_type" {
  description = "Web server instance type"
  type        = string
  default     = "t3.micro"
}

variable "app_server_count" {
  description = "Number of app servers"
  type        = number
  default     = 2
}

variable "app_instance_type" {
  description = "App server instance type"
  type        = string
  default     = "t3.small"
}

variable "db_instance_class" {
  description = "Database instance class"
  type        = string
  default     = "db.t3.micro"
}

variable "db_name" {
  description = "Database name"
  type        = string
}

variable "db_username" {
  description = "Database username"
  type        = string
}

variable "db_password" {
  description = "Database password"
  type        = string
  sensitive   = true
}
```

**outputs.tf**
```hcl
output "vpc_id" {
  description = "VPC ID"
  value       = module.vpc.vpc_id
}

output "web_server_ips" {
  description = "Web server public IPs"
  value       = module.web_servers[*].public_ip
}

output "app_server_ips" {
  description = "App server private IPs"
  value       = module.app_servers[*].private_ip
}

output "database_endpoint" {
  description = "Database endpoint"
  value       = module.database.endpoint
}
```

---

## Best Practices

1. **Keep modules focused** - One responsibility per module
2. **Use semantic versioning** - v1.0.0, v1.1.0, v2.0.0
3. **Document your modules** - README with examples
4. **Use variables for flexibility** - Don't hardcode values
5. **Provide sensible defaults** - Make modules easy to use
6. **Use outputs generously** - Expose useful attributes
7. **Test your modules** - Create example configurations
8. **Version your modules** - Use Git tags or registry versions
9. **Follow naming conventions** - terraform-<PROVIDER>-<NAME>
10. **Keep modules DRY** - Don't repeat yourself

# Phase 7: Workspaces & Environments

## 1. Terraform Workspaces

Workspaces allow you to manage multiple instances of a single configuration.

### What are Workspaces?

- Each workspace has its own state file
- Same configuration, different state
- Useful for managing multiple environments
- Default workspace is called "default"

### Workspace Commands

```bash
# List workspaces
terraform workspace list

# Show current workspace
terraform workspace show

# Create new workspace
terraform workspace new dev
terraform workspace new staging
terraform workspace new prod

# Switch workspace
terraform workspace select dev

# Delete workspace (must not be current)
terraform workspace delete staging
```

---

## 2. Use Cases of Workspaces

### Environment Separation

```hcl
# main.tf
locals {
  environment = terraform.workspace
  
  # Different configs per workspace
  instance_type = {
    default = "t3.micro"
    dev     = "t3.micro"
    staging = "t3.small"
    prod    = "t3.large"
  }
  
  instance_count = {
    default = 1
    dev     = 1
    staging = 2
    prod    = 5
  }
}

resource "aws_instance" "app" {
  count         = local.instance_count[local.environment]
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = local.instance_type[local.environment]
  
  tags = {
    Name        = "${local.environment}-app-${count.index + 1}"
    Environment = local.environment
  }
}

output "environment" {
  value = local.environment
}

output "instance_ids" {
  value = aws_instance.app[*].id
}
```

### Usage

```bash
# Create dev environment
terraform workspace new dev
terraform apply

# Create prod environment
terraform workspace new prod
terraform apply

# Switch between environments
terraform workspace select dev
terraform plan

terraform workspace select prod
terraform plan
```

---

## 3. Managing Multiple Environments

### Method 1: Workspaces (Simple)

```hcl
# variables.tf
variable "region" {
  type    = string
  default = "us-east-1"
}

# main.tf
locals {
  env = terraform.workspace
  
  config = {
    dev = {
      instance_type = "t3.micro"
      instance_count = 1
      db_instance_class = "db.t3.micro"
    }
    staging = {
      instance_type = "t3.small"
      instance_count = 2
      db_instance_class = "db.t3.small"
    }
    prod = {
      instance_type = "t3.large"
      instance_count = 5
      db_instance_class = "db.t3.large"
    }
  }
  
  current_config = local.config[local.env]
}

provider "aws" {
  region = var.region
}

resource "aws_instance" "app" {
  count         = local.current_config.instance_count
  ami           = data.aws_ami.ubuntu.id
  instance_type = local.current_config.instance_type
  
  tags = {
    Name        = "${local.env}-app-${count.index + 1}"
    Environment = local.env
  }
}

resource "aws_db_instance" "main" {
  identifier     = "${local.env}-db"
  engine         = "postgres"
  instance_class = local.current_config.db_instance_class
  allocated_storage = 20
  
  username = "admin"
  password = var.db_password
  
  skip_final_snapshot = local.env != "prod"
  
  tags = {
    Name        = "${local.env}-db"
    Environment = local.env
  }
}

data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}
```

### Method 2: Separate Directories (Recommended)

```
environments/
├── dev/
│   ├── main.tf
│   ├── variables.tf
│   ├── terraform.tfvars
│   └── backend.tf
├── staging/
│   ├── main.tf
│   ├── variables.tf
│   ├── terraform.tfvars
│   └── backend.tf
└── prod/
    ├── main.tf
    ├── variables.tf
    ├── terraform.tfvars
    └── backend.tf
```

**environments/dev/main.tf**
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "dev/terraform.tfstate"
    region = "us-east-1"
  }
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

module "app" {
  source = "../../modules/app"
  
  environment    = "dev"
  instance_type  = "t3.micro"
  instance_count = 1
  
  vpc_cidr = "10.0.0.0/16"
}
```

**environments/dev/terraform.tfvars**
```hcl
region = "us-east-1"
```

**environments/prod/main.tf**
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

module "app" {
  source = "../../modules/app"
  
  environment    = "prod"
  instance_type  = "t3.large"
  instance_count = 5
  
  vpc_cidr = "10.1.0.0/16"
}
```

---

## 4. dev, qa, prod Environments

### Complete Environment Setup

```
project/
├── modules/
│   └── infrastructure/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
└── environments/
    ├── dev/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── terraform.tfvars
    │   └── backend.tf
    ├── qa/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── terraform.tfvars
    │   └── backend.tf
    └── prod/
        ├── main.tf
        ├── variables.tf
        ├── terraform.tfvars
        └── backend.tf
```

**modules/infrastructure/variables.tf**
```hcl
variable "environment" {
  description = "Environment name"
  type        = string
}

variable "vpc_cidr" {
  description = "VPC CIDR block"
  type        = string
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
}

variable "instance_count" {
  description = "Number of instances"
  type        = number
}

variable "db_instance_class" {
  description = "RDS instance class"
  type        = string
}

variable "enable_monitoring" {
  description = "Enable detailed monitoring"
  type        = bool
  default     = false
}

variable "backup_retention_days" {
  description = "Database backup retention days"
  type        = number
  default     = 7
}
```

**modules/infrastructure/main.tf**
```hcl
# VPC
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name        = "${var.environment}-vpc"
    Environment = var.environment
  }
}

# Subnets
resource "aws_subnet" "public" {
  count = 2
  
  vpc_id                  = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 8, count.index)
  availability_zone       = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true
  
  tags = {
    Name        = "${var.environment}-public-${count.index + 1}"
    Environment = var.environment
  }
}

resource "aws_subnet" "private" {
  count = 2
  
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet(var.vpc_cidr, 8, count.index + 10)
  availability_zone = data.aws_availability_zones.available.names[count.index]
  
  tags = {
    Name        = "${var.environment}-private-${count.index + 1}"
    Environment = var.environment
  }
}

# Internet Gateway
resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  
  tags = {
    Name        = "${var.environment}-igw"
    Environment = var.environment
  }
}

# Route Table
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
  
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }
  
  tags = {
    Name        = "${var.environment}-public-rt"
    Environment = var.environment
  }
}

resource "aws_route_table_association" "public" {
  count = 2
  
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}

# Security Group
resource "aws_security_group" "app" {
  name        = "${var.environment}-app-sg"
  description = "Security group for app servers"
  vpc_id      = aws_vpc.main.id
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
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
    Name        = "${var.environment}-app-sg"
    Environment = var.environment
  }
}

# EC2 Instances
resource "aws_instance" "app" {
  count = var.instance_count
  
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  subnet_id     = aws_subnet.public[count.index % 2].id
  
  vpc_security_group_ids = [aws_security_group.app.id]
  
  monitoring = var.enable_monitoring
  
  tags = {
    Name        = "${var.environment}-app-${count.index + 1}"
    Environment = var.environment
  }
}

# RDS Instance
resource "aws_db_subnet_group" "main" {
  name       = "${var.environment}-db-subnet-group"
  subnet_ids = aws_subnet.private[*].id
  
  tags = {
    Name        = "${var.environment}-db-subnet-group"
    Environment = var.environment
  }
}

resource "aws_security_group" "db" {
  name        = "${var.environment}-db-sg"
  description = "Security group for database"
  vpc_id      = aws_vpc.main.id
  
  ingress {
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [aws_security_group.app.id]
  }
  
  tags = {
    Name        = "${var.environment}-db-sg"
    Environment = var.environment
  }
}

resource "aws_db_instance" "main" {
  identifier     = "${var.environment}-db"
  engine         = "postgres"
  engine_version = "14.7"
  instance_class = var.db_instance_class
  
  allocated_storage = 20
  storage_encrypted = var.environment == "prod"
  
  db_name  = "myapp"
  username = "admin"
  password = random_password.db_password.result
  
  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [aws_security_group.db.id]
  
  backup_retention_period = var.backup_retention_days
  skip_final_snapshot     = var.environment != "prod"
  
  tags = {
    Name        = "${var.environment}-db"
    Environment = var.environment
  }
}

resource "random_password" "db_password" {
  length  = 16
  special = true
}

resource "aws_secretsmanager_secret" "db_password" {
  name = "${var.environment}-db-password"
}

resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = random_password.db_password.result
}

data "aws_availability_zones" "available" {
  state = "available"
}

data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}
```

**modules/infrastructure/outputs.tf**
```hcl
output "vpc_id" {
  value = aws_vpc.main.id
}

output "instance_ids" {
  value = aws_instance.app[*].id
}

output "instance_public_ips" {
  value = aws_instance.app[*].public_ip
}

output "db_endpoint" {
  value = aws_db_instance.main.endpoint
}

output "db_password_secret_arn" {
  value = aws_secretsmanager_secret.db_password.arn
}
```

**environments/dev/main.tf**
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "dev/terraform.tfstate"
    region = "us-east-1"
  }
}

provider "aws" {
  region = "us-east-1"
}

module "infrastructure" {
  source = "../../modules/infrastructure"
  
  environment           = "dev"
  vpc_cidr              = "10.0.0.0/16"
  instance_type         = "t3.micro"
  instance_count        = 1
  db_instance_class     = "db.t3.micro"
  enable_monitoring     = false
  backup_retention_days = 1
}

output "instance_ips" {
  value = module.infrastructure.instance_public_ips
}

output "db_endpoint" {
  value = module.infrastructure.db_endpoint
}
```

**environments/qa/main.tf**
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "qa/terraform.tfstate"
    region = "us-east-1"
  }
}

provider "aws" {
  region = "us-east-1"
}

module "infrastructure" {
  source = "../../modules/infrastructure"
  
  environment           = "qa"
  vpc_cidr              = "10.1.0.0/16"
  instance_type         = "t3.small"
  instance_count        = 2
  db_instance_class     = "db.t3.small"
  enable_monitoring     = false
  backup_retention_days = 3
}

output "instance_ips" {
  value = module.infrastructure.instance_public_ips
}

output "db_endpoint" {
  value = module.infrastructure.db_endpoint
}
```

**environments/prod/main.tf**
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}

provider "aws" {
  region = "us-east-1"
}

module "infrastructure" {
  source = "../../modules/infrastructure"
  
  environment           = "prod"
  vpc_cidr              = "10.2.0.0/16"
  instance_type         = "t3.large"
  instance_count        = 5
  db_instance_class     = "db.t3.large"
  enable_monitoring     = true
  backup_retention_days = 30
}

output "instance_ips" {
  value = module.infrastructure.instance_public_ips
}

output "db_endpoint" {
  value = module.infrastructure.db_endpoint
}
```

### Deploying Each Environment

```bash
# Deploy dev
cd environments/dev
terraform init
terraform plan
terraform apply

# Deploy qa
cd ../qa
terraform init
terraform plan
terraform apply

# Deploy prod
cd ../prod
terraform init
terraform plan
terraform apply
```

---

## 5. Workspaces vs Separate State Files

### Workspaces

**Pros:**
- Single codebase
- Easy to switch
- Less duplication

**Cons:**
- Shared backend configuration
- Easy to accidentally affect wrong environment
- All environments in same state backend

### Separate State Files (Recommended)

**Pros:**
- Complete isolation
- Different backends per environment
- Safer (can't accidentally destroy prod)
- Different configurations per environment

**Cons:**
- More code duplication
- Must cd to switch environments

---

## 6. Environment-based Variable Files (.tfvars)

### Using .tfvars Files

**terraform.tfvars** (default, auto-loaded)
```hcl
region = "us-east-1"
project = "myapp"
```

**dev.tfvars**
```hcl
environment    = "dev"
instance_type  = "t3.micro"
instance_count = 1
db_instance_class = "db.t3.micro"
enable_monitoring = false
```

**staging.tfvars**
```hcl
environment    = "staging"
instance_type  = "t3.small"
instance_count = 2
db_instance_class = "db.t3.small"
enable_monitoring = false
```

**prod.tfvars**
```hcl
environment    = "prod"
instance_type  = "t3.large"
instance_count = 5
db_instance_class = "db.t3.large"
enable_monitoring = true
```

### Usage

```bash
# Apply with specific tfvars file
terraform apply -var-file="dev.tfvars"
terraform apply -var-file="staging.tfvars"
terraform apply -var-file="prod.tfvars"
```

---

## Best Practices

1. **Use separate directories for environments** (most recommended)
2. **Use workspaces for temporary/testing environments**
3. **Never use workspaces for production**
4. **Keep environment configs in version control**
5. **Use different AWS accounts for prod**
6. **Implement approval process for prod changes**
7. **Use remote state with locking**
8. **Tag all resources with environment**
9. **Use consistent naming conventions**
10. **Document environment differences**

# Phase 8: Provisioners & Templates

## 1. Provisioners (Introduction)

Provisioners are used to execute scripts on local or remote machines as part of resource creation or destruction.

### ⚠️ Important Note

**Provisioners should be a last resort.** Terraform recommends using other methods first:
- User data / cloud-init
- Configuration management tools (Ansible, Chef, Puppet)
- Packer for pre-baked images
- Container images

### When to Use Provisioners

- Bootstrapping initial configuration
- Running cleanup scripts
- Triggering external systems
- One-time setup tasks

---

## 2. local-exec Provisioner

Executes commands on the machine running Terraform.

### Basic Usage

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  provisioner "local-exec" {
    command = "echo ${self.public_ip} > ip_address.txt"
  }
}
```

### Advanced local-exec

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  # Run on creation
  provisioner "local-exec" {
    command = "echo 'Instance ${self.id} created at ${timestamp()}' >> creation_log.txt"
  }
  
  # Run on destruction
  provisioner "local-exec" {
    when    = destroy
    command = "echo 'Instance destroyed at ${timestamp()}' >> destruction_log.txt"
  }
  
  # With working directory
  provisioner "local-exec" {
    command     = "./notify.sh ${self.public_ip}"
    working_dir = "${path.module}/scripts"
  }
  
  # With environment variables
  provisioner "local-exec" {
    command = "ansible-playbook -i ${self.public_ip}, playbook.yml"
    
    environment = {
      ANSIBLE_HOST_KEY_CHECKING = "False"
      INSTANCE_ID               = self.id
    }
  }
}
```

### Practical Examples

```hcl
# Update inventory file
resource "aws_instance" "web" {
  count = 3
  
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  provisioner "local-exec" {
    command = "echo '${self.public_ip} ansible_user=ubuntu' >> inventory.ini"
  }
}

# Trigger webhook
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  provisioner "local-exec" {
    command = <<-EOT
      curl -X POST https://api.example.com/notify \
        -H "Content-Type: application/json" \
        -d '{"instance_id": "${self.id}", "ip": "${self.public_ip}"}'
    EOT
  }
}

# Run Python script
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  provisioner "local-exec" {
    command = "python3 scripts/configure.py --instance-id ${self.id} --ip ${self.public_ip}"
  }
}
```

---

## 3. remote-exec Provisioner

Executes commands on the remote resource.

### Basic Usage

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  # Connection block required for remote-exec
  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip
  }
  
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl start nginx",
      "sudo systemctl enable nginx"
    ]
  }
}
```

### Connection Types

**SSH Connection**
```hcl
connection {
  type        = "ssh"
  user        = "ubuntu"
  private_key = file("~/.ssh/id_rsa")
  host        = self.public_ip
  port        = 22
  timeout     = "5m"
  
  # Optional: bastion host
  bastion_host        = "bastion.example.com"
  bastion_user        = "ubuntu"
  bastion_private_key = file("~/.ssh/bastion_key")
}
```

**WinRM Connection (Windows)**
```hcl
connection {
  type     = "winrm"
  user     = "Administrator"
  password = var.admin_password
  host     = self.public_ip
  port     = 5985
  https    = false
  insecure = true
  timeout  = "10m"
}
```

### remote-exec with Script

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip
  }
  
  # Run inline commands
  provisioner "remote-exec" {
    inline = [
      "echo 'Starting setup...'",
      "sudo apt-get update",
      "sudo apt-get install -y docker.io",
      "sudo systemctl start docker",
      "sudo usermod -aG docker ubuntu"
    ]
  }
  
  # Run script file
  provisioner "remote-exec" {
    script = "${path.module}/scripts/setup.sh"
  }
  
  # Run multiple scripts
  provisioner "remote-exec" {
    scripts = [
      "${path.module}/scripts/install_dependencies.sh",
      "${path.module}/scripts/configure_app.sh",
      "${path.module}/scripts/start_services.sh"
    ]
  }
}
```

### Complete Example

```hcl
# Key pair for SSH
resource "aws_key_pair" "deployer" {
  key_name   = "deployer-key"
  public_key = file("~/.ssh/id_rsa.pub")
}

# Security group allowing SSH
resource "aws_security_group" "web" {
  name = "web-sg"
  
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# Instance with provisioners
resource "aws_instance" "web" {
  ami                    = "ami-0c55b159cbfafe1f0"
  instance_type          = "t3.micro"
  key_name               = aws_key_pair.deployer.key_name
  vpc_security_group_ids = [aws_security_group.web.id]
  
  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip
  }
  
  # Install web server
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "echo '<h1>Hello from Terraform!</h1>' | sudo tee /var/www/html/index.html",
      "sudo systemctl start nginx",
      "sudo systemctl enable nginx"
    ]
  }
  
  # Notify on creation
  provisioner "local-exec" {
    command = "echo 'Web server created: ${self.public_ip}' >> deployment_log.txt"
  }
  
  # Cleanup on destruction
  provisioner "local-exec" {
    when    = destroy
    command = "echo 'Web server destroyed' >> deployment_log.txt"
  }
  
  tags = {
    Name = "WebServer"
  }
}

output "web_server_url" {
  value = "http://${aws_instance.web.public_ip}"
}
```

---

## 4. file Provisioner

Copies files or directories from local machine to remote resource.

### Basic Usage

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip
  }
  
  # Copy single file
  provisioner "file" {
    source      = "app.conf"
    destination = "/tmp/app.conf"
  }
  
  # Copy directory
  provisioner "file" {
    source      = "configs/"
    destination = "/tmp/configs"
  }
  
  # Copy with content
  provisioner "file" {
    content     = "Hello, World!"
    destination = "/tmp/hello.txt"
  }
  
  # Copy and set permissions
  provisioner "file" {
    source      = "scripts/deploy.sh"
    destination = "/tmp/deploy.sh"
  }
  
  provisioner "remote-exec" {
    inline = [
      "chmod +x /tmp/deploy.sh",
      "/tmp/deploy.sh"
    ]
  }
}
```

### Complete Application Deployment

```hcl
resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip
  }
  
  # Copy application files
  provisioner "file" {
    source      = "app/"
    destination = "/home/ubuntu/app"
  }
  
  # Copy configuration
  provisioner "file" {
    content = templatefile("${path.module}/templates/app.conf.tpl", {
      db_host = aws_db_instance.main.endpoint
      db_name = "myapp"
    })
    destination = "/home/ubuntu/app/config.conf"
  }
  
  # Copy and run deployment script
  provisioner "file" {
    source      = "scripts/deploy.sh"
    destination = "/tmp/deploy.sh"
  }
  
  provisioner "remote-exec" {
    inline = [
      "chmod +x /tmp/deploy.sh",
      "sudo /tmp/deploy.sh"
    ]
  }
}
```

---

## 5. When NOT to Use Provisioners

### ❌ Bad: Using Provisioners

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl start nginx"
    ]
  }
}
```

### ✅ Good: Using User Data

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  user_data = <<-EOF
    #!/bin/bash
    apt-get update
    apt-get install -y nginx
    systemctl start nginx
  EOF
}
```

### ✅ Better: Using Packer

```hcl
# Build AMI with Packer first
# packer.json
{
  "builders": [{
    "type": "amazon-ebs",
    "ami_name": "web-server-{{timestamp}}",
    "instance_type": "t3.micro",
    "source_ami": "ami-0c55b159cbfafe1f0"
  }],
  "provisioners": [{
    "type": "shell",
    "inline": [
      "sudo apt-get update",
      "sudo apt-get install -y nginx"
    ]
  }]
}

# Then use in Terraform
data "aws_ami" "web_server" {
  most_recent = true
  owners      = ["self"]
  
  filter {
    name   = "name"
    values = ["web-server-*"]
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.web_server.id
  instance_type = "t3.micro"
}
```

---

## 6. Template Files (templatefile)

The `templatefile` function renders templates with variables.

### Basic Template

**templates/user_data.sh**
```bash
#!/bin/bash
echo "Environment: ${environment}"
echo "App Version: ${app_version}"

# Install application
apt-get update
apt-get install -y ${packages}

# Configure application
cat > /etc/app/config.conf <<EOL
DB_HOST=${db_host}
DB_NAME=${db_name}
DB_USER=${db_user}
API_KEY=${api_key}
EOL

# Start application
systemctl start myapp
```

**main.tf**
```hcl
resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  user_data = templatefile("${path.module}/templates/user_data.sh", {
    environment = var.environment
    app_version = var.app_version
    packages    = join(" ", ["docker.io", "nginx", "postgresql-client"])
    db_host     = aws_db_instance.main.endpoint
    db_name     = "myapp"
    db_user     = "admin"
    api_key     = var.api_key
  })
}
```

### Advanced Templates

**templates/nginx.conf.tpl**
```nginx
server {
    listen 80;
    server_name ${server_name};
    
    location / {
        proxy_pass http://${backend_host}:${backend_port};
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    
%{ for path, target in proxy_paths ~}
    location ${path} {
        proxy_pass ${target};
    }
%{ endfor ~}
    
%{ if enable_ssl ~}
    listen 443 ssl;
    ssl_certificate /etc/ssl/certs/cert.pem;
    ssl_certificate_key /etc/ssl/private/key.pem;
%{ endif ~}
}
```

**main.tf**
```hcl
locals {
  nginx_config = templatefile("${path.module}/templates/nginx.conf.tpl", {
    server_name   = "example.com"
    backend_host  = "localhost"
    backend_port  = 8080
    enable_ssl    = var.environment == "prod"
    proxy_paths   = {
      "/api"    = "http://api-server:3000"
      "/static" = "http://static-server:8000"
    }
  })
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  user_data = <<-EOF
    #!/bin/bash
    apt-get update
    apt-get install -y nginx
    
    cat > /etc/nginx/sites-available/default <<'EOL'
    ${local.nginx_config}
    EOL
    
    systemctl restart nginx
  EOF
}
```

### Template with Loops

**templates/hosts.tpl**
```
# Generated by Terraform
127.0.0.1 localhost

# Application servers
%{ for idx, ip in app_servers ~}
app-${idx + 1} ${ip}
%{ endfor ~}

# Database servers
%{ for name, ip in db_servers ~}
${name} ${ip}
%{ endfor ~}
```

**main.tf**
```hcl
locals {
  hosts_file = templatefile("${path.module}/templates/hosts.tpl", {
    app_servers = aws_instance.app[*].private_ip
    db_servers  = {
      db-primary   = aws_db_instance.primary.address
      db-secondary = aws_db_instance.secondary.address
    }
  })
}
```

---

## 7. User Data with Terraform

### Basic User Data

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  user_data = <<-EOF
    #!/bin/bash
    apt-get update
    apt-get install -y nginx
    systemctl start nginx
    systemctl enable nginx
  EOF
  
  tags = {
    Name = "WebServer"
  }
}
```

### User Data with Variables

```hcl
variable "environment" {
  type = string
}

variable "app_version" {
  type = string
}

resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  user_data = <<-EOF
    #!/bin/bash
    set -e
    
    # Environment: ${var.environment}
    # Version: ${var.app_version}
    
    apt-get update
    apt-get install -y docker.io
    
    docker run -d \
      -e ENVIRONMENT=${var.environment} \
      -e VERSION=${var.app_version} \
      -p 80:8080 \
      myapp:${var.app_version}
  EOF
}
```

### User Data with Template File

**templates/user_data.sh**
```bash
#!/bin/bash
set -e

# Configuration
ENVIRONMENT="${environment}"
APP_VERSION="${app_version}"
DB_HOST="${db_host}"
DB_NAME="${db_name}"

# Install dependencies
apt-get update
apt-get install -y \
    docker.io \
    postgresql-client \
    awscli

# Configure application
mkdir -p /etc/myapp
cat > /etc/myapp/config.json <<EOL
{
  "environment": "$ENVIRONMENT",
  "version": "$APP_VERSION",
  "database": {
    "host": "$DB_HOST",
    "name": "$DB_NAME"
  }
}
EOL

# Start application
docker run -d \
  --name myapp \
  -v /etc/myapp:/config \
  -p 80:8080 \
  myapp:$APP_VERSION

# Health check
until curl -f http://localhost/health; do
  echo "Waiting for application to start..."
  sleep 5
done

echo "Application started successfully"
```

**main.tf**
```hcl
resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  user_data = templatefile("${path.module}/templates/user_data.sh", {
    environment = var.environment
    app_version = var.app_version
    db_host     = aws_db_instance.main.endpoint
    db_name     = aws_db_instance.main.db_name
  })
  
  user_data_replace_on_change = true  # Recreate instance if user_data changes
  
  tags = {
    Name        = "AppServer"
    Environment = var.environment
  }
}
```

### Cloud-Init Multi-Part

```hcl
data "cloudinit_config" "app" {
  gzip          = true
  base64_encode = true
  
  # First part: shell script
  part {
    content_type = "text/x-shellscript"
    content      = templatefile("${path.module}/templates/install.sh", {
      app_version = var.app_version
    })
  }
  
  # Second part: cloud-config
  part {
    content_type = "text/cloud-config"
    content = yamlencode({
      packages = ["docker.io", "nginx"]
      runcmd = [
        "systemctl start docker",
        "systemctl enable docker"
      ]
    })
  }
}

resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  user_data     = data.cloudinit_config.app.rendered
}
```

---

## Complete Example: Web Application Deployment

```hcl
# variables.tf
variable "environment" {
  type = string
}

variable "app_version" {
  type = string
}

variable "db_password" {
  type      = string
  sensitive = true
}

# templates/user_data.sh
#!/bin/bash
set -e

# Update system
apt-get update
apt-get upgrade -y

# Install dependencies
apt-get install -y \
    docker.io \
    docker-compose \
    nginx \
    certbot \
    python3-certbot-nginx

# Configure Docker
systemctl start docker
systemctl enable docker
usermod -aG docker ubuntu

# Create application directory
mkdir -p /opt/myapp
cd /opt/myapp

# Create docker-compose.yml
cat > docker-compose.yml <<'EOL'
version: '3.8'
services:
  app:
    image: myapp:${app_version}
    environment:
      - ENVIRONMENT=${environment}
      - DB_HOST=${db_host}
      - DB_NAME=${db_name}
      - DB_USER=${db_user}
      - DB_PASSWORD=${db_password}
    ports:
      - "8080:8080"
    restart: always
EOL

# Start application
docker-compose up -d

# Configure Nginx
cat > /etc/nginx/sites-available/default <<'EOL'
server {
    listen 80;
    server_name _;
    
    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
EOL

systemctl restart nginx

echo "Deployment complete!"

# main.tf
resource "aws_key_pair" "deployer" {
  key_name   = "${var.environment}-deployer"
  public_key = file("~/.ssh/id_rsa.pub")
}

resource "aws_security_group" "app" {
  name = "${var.environment}-app-sg"
  
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
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
}

resource "aws_db_instance" "main" {
  identifier     = "${var.environment}-db"
  engine         = "postgres"
  instance_class = "db.t3.micro"
  allocated_storage = 20
  
  db_name  = "myapp"
  username = "admin"
  password = var.db_password
  
  skip_final_snapshot = true
}

resource "aws_instance" "app" {
  ami                    = "ami-0c55b159cbfafe1f0"
  instance_type          = "t3.micro"
  key_name               = aws_key_pair.deployer.key_name
  vpc_security_group_ids = [aws_security_group.app.id]
  
  user_data = templatefile("${path.module}/templates/user_data.sh", {
    environment = var.environment
    app_version = var.app_version
    db_host     = aws_db_instance.main.endpoint
    db_name     = aws_db_instance.main.db_name
    db_user     = aws_db_instance.main.username
    db_password = var.db_password
  })
  
  tags = {
    Name        = "${var.environment}-app"
    Environment = var.environment
  }
}

output "app_url" {
  value = "http://${aws_instance.app.public_ip}"
}
```

---

## Best Practices

1. **Avoid provisioners when possible** - Use user data, Packer, or configuration management
2. **Use templatefile for complex configurations**
3. **Keep provisioner scripts idempotent**
4. **Handle provisioner failures gracefully**
5. **Use null_resource for standalone provisioners**
6. **Test provisioner scripts independently**
7. **Use proper error handling in scripts**
8. **Log provisioner output for debugging**
9. **Use cloud-init for better reliability**
10. **Consider using Packer for AMI creation**

# Phase 9: Terraform with AWS (Core Cloud Skills)

## 1. AWS Provider Setup

### Basic Provider Configuration

```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  
  # Optional: Use named profile
  profile = "default"
  
  # Optional: Assume role
  assume_role {
    role_arn = "arn:aws:iam::123456789012:role/TerraformRole"
  }
  
  # Default tags for all resources
  default_tags {
    tags = {
      ManagedBy   = "Terraform"
      Environment = var.environment
      Project     = var.project
    }
  }
}
```

### Authentication Methods

**1. Environment Variables (Recommended)**
```bash
export AWS_ACCESS_KEY_ID="your_access_key"
export AWS_SECRET_ACCESS_KEY="your_secret_key"
export AWS_DEFAULT_REGION="us-east-1"
```

**2. AWS CLI Profile**
```bash
# Configure AWS CLI
aws configure --profile terraform

# Use in Terraform
provider "aws" {
  region  = "us-east-1"
  profile = "terraform"
}
```

**3. IAM Role (Best for EC2/ECS)**
```hcl
provider "aws" {
  region = "us-east-1"
  # Automatically uses instance IAM role
}
```

---

## 2. IAM Roles & Policies

### IAM User

```hcl
resource "aws_iam_user" "developer" {
  name = "developer"
  path = "/users/"
  
  tags = {
    Role = "Developer"
  }
}

resource "aws_iam_user_login_profile" "developer" {
  user    = aws_iam_user.developer.name
  password_length = 20
}

resource "aws_iam_access_key" "developer" {
  user = aws_iam_user.developer.name
}

output "developer_access_key" {
  value     = aws_iam_access_key.developer.id
  sensitive = true
}

output "developer_secret_key" {
  value     = aws_iam_access_key.developer.secret
  sensitive = true
}
```

### IAM Group

```hcl
resource "aws_iam_group" "developers" {
  name = "developers"
  path = "/groups/"
}

resource "aws_iam_group_membership" "developers" {
  name = "developers-membership"
  
  users = [
    aws_iam_user.developer.name,
  ]
  
  group = aws_iam_group.developers.name
}

resource "aws_iam_group_policy_attachment" "developers_readonly" {
  group      = aws_iam_group.developers.name
  policy_arn = "arn:aws:iam::aws:policy/ReadOnlyAccess"
}
```

### IAM Role

```hcl
# IAM Role for EC2
resource "aws_iam_role" "ec2_role" {
  name = "ec2-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"
      }
    }]
  })
}

# Custom IAM Policy
resource "aws_iam_policy" "s3_access" {
  name        = "s3-access-policy"
  description = "Policy for S3 access"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Action = [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ]
      Resource = [
        "arn:aws:s3:::my-bucket",
        "arn:aws:s3:::my-bucket/*"
      ]
    }]
  })
}

# Attach policy to role
resource "aws_iam_role_policy_attachment" "ec2_s3_access" {
  role       = aws_iam_role.ec2_role.name
  policy_arn = aws_iam_policy.s3_access.arn
}

# Instance profile
resource "aws_iam_instance_profile" "ec2_profile" {
  name = "ec2-profile"
  role = aws_iam_role.ec2_role.name
}

# Use in EC2 instance
resource "aws_instance" "app" {
  ami                  = "ami-0c55b159cbfafe1f0"
  instance_type        = "t3.micro"
  iam_instance_profile = aws_iam_instance_profile.ec2_profile.name
}
```

---

## 3. VPC Creation

### Complete VPC Setup

```hcl
# VPC
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name = "main-vpc"
  }
}

# Data source for availability zones
data "aws_availability_zones" "available" {
  state = "available"
}

# Public Subnets
resource "aws_subnet" "public" {
  count = 3
  
  vpc_id                  = aws_vpc.main.id
  cidr_block              = cidrsubnet(aws_vpc.main.cidr_block, 8, count.index)
  availability_zone       = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true
  
  tags = {
    Name = "public-subnet-${count.index + 1}"
    Type = "Public"
  }
}

# Private Subnets
resource "aws_subnet" "private" {
  count = 3
  
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet(aws_vpc.main.cidr_block, 8, count.index + 10)
  availability_zone = data.aws_availability_zones.available.names[count.index]
  
  tags = {
    Name = "private-subnet-${count.index + 1}"
    Type = "Private"
  }
}

# Internet Gateway
resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  
  tags = {
    Name = "main-igw"
  }
}

# Elastic IPs for NAT Gateways
resource "aws_eip" "nat" {
  count = 3
  
  domain = "vpc"
  
  tags = {
    Name = "nat-eip-${count.index + 1}"
  }
  
  depends_on = [aws_internet_gateway.main]
}

# NAT Gateways
resource "aws_nat_gateway" "main" {
  count = 3
  
  allocation_id = aws_eip.nat[count.index].id
  subnet_id     = aws_subnet.public[count.index].id
  
  tags = {
    Name = "nat-gateway-${count.index + 1}"
  }
}

# Public Route Table
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
  
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }
  
  tags = {
    Name = "public-route-table"
  }
}

# Public Route Table Association
resource "aws_route_table_association" "public" {
  count = 3
  
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}

# Private Route Tables
resource "aws_route_table" "private" {
  count = 3
  
  vpc_id = aws_vpc.main.id
  
  route {
    cidr_block     = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.main[count.index].id
  }
  
  tags = {
    Name = "private-route-table-${count.index + 1}"
  }
}

# Private Route Table Association
resource "aws_route_table_association" "private" {
  count = 3
  
  subnet_id      = aws_subnet.private[count.index].id
  route_table_id = aws_route_table.private[count.index].id
}

# VPC Flow Logs
resource "aws_flow_log" "main" {
  iam_role_arn    = aws_iam_role.flow_log.arn
  log_destination = aws_cloudwatch_log_group.flow_log.arn
  traffic_type    = "ALL"
  vpc_id          = aws_vpc.main.id
}

resource "aws_cloudwatch_log_group" "flow_log" {
  name              = "/aws/vpc/flow-logs"
  retention_in_days = 7
}

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

# Outputs
output "vpc_id" {
  value = aws_vpc.main.id
}

output "public_subnet_ids" {
  value = aws_subnet.public[*].id
}

output "private_subnet_ids" {
  value = aws_subnet.private[*].id
}
```

---

## 4. Security Groups

```hcl
# Web Server Security Group
resource "aws_security_group" "web" {
  name        = "web-sg"
  description = "Security group for web servers"
  vpc_id      = aws_vpc.main.id
  
  # HTTP
  ingress {
    description = "HTTP from anywhere"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # HTTPS
  ingress {
    description = "HTTPS from anywhere"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # SSH from specific IP
  ingress {
    description = "SSH from office"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["203.0.113.0/24"]
  }
  
  # All outbound traffic
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = "web-sg"
  }
}

# Application Server Security Group
resource "aws_security_group" "app" {
  name        = "app-sg"
  description = "Security group for application servers"
  vpc_id      = aws_vpc.main.id
  
  # Allow traffic from web servers
  ingress {
    description     = "Traffic from web servers"
    from_port       = 8080
    to_port         = 8080
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
  }
}

# Database Security Group
resource "aws_security_group" "db" {
  name        = "db-sg"
  description = "Security group for database"
  vpc_id      = aws_vpc.main.id
  
  # PostgreSQL from app servers
  ingress {
    description     = "PostgreSQL from app servers"
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [aws_security_group.app.id]
  }
  
  tags = {
    Name = "db-sg"
  }
}

# Security Group Rules (Alternative method)
resource "aws_security_group_rule" "web_http" {
  type              = "ingress"
  from_port         = 80
  to_port           = 80
  protocol          = "tcp"
  cidr_blocks       = ["0.0.0.0/0"]
  security_group_id = aws_security_group.web.id
  description       = "Allow HTTP"
}
```

---

## 5. EC2 Instances

### Basic EC2 Instance

```hcl
# Get latest Ubuntu AMI
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
  
  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}

# Key Pair
resource "aws_key_pair" "deployer" {
  key_name   = "deployer-key"
  public_key = file("~/.ssh/id_rsa.pub")
}

# EC2 Instance
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  subnet_id                   = aws_subnet.public[0].id
  vpc_security_group_ids      = [aws_security_group.web.id]
  associate_public_ip_address = true
  
  root_block_device {
    volume_type           = "gp3"
    volume_size           = 20
    delete_on_termination = true
    encrypted             = true
  }
  
  user_data = <<-EOF
    #!/bin/bash
    apt-get update
    apt-get install -y nginx
    systemctl start nginx
    systemctl enable nginx
  EOF
  
  tags = {
    Name = "web-server"
  }
}

output "instance_public_ip" {
  value = aws_instance.web.public_ip
}
```

### EC2 with Multiple Instances

```hcl
resource "aws_instance" "app" {
  count = 3
  
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  subnet_id              = aws_subnet.private[count.index % length(aws_subnet.private)].id
  vpc_security_group_ids = [aws_security_group.app.id]
  
  iam_instance_profile = aws_iam_instance_profile.ec2_profile.name
  
  user_data = templatefile("${path.module}/user_data.sh", {
    instance_name = "app-${count.index + 1}"
  })
  
  tags = {
    Name = "app-server-${count.index + 1}"
  }
}
```

---

## 6. Load Balancer (ALB/NLB)

### Application Load Balancer

```hcl
# ALB
resource "aws_lb" "main" {
  name               = "main-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb.id]
  subnets            = aws_subnet.public[*].id
  
  enable_deletion_protection = false
  enable_http2               = true
  
  tags = {
    Name = "main-alb"
  }
}

# ALB Security Group
resource "aws_security_group" "alb" {
  name        = "alb-sg"
  description = "Security group for ALB"
  vpc_id      = aws_vpc.main.id
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
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

# Target Group
resource "aws_lb_target_group" "app" {
  name     = "app-tg"
  port     = 80
  protocol = "HTTP"
  vpc_id   = aws_vpc.main.id
  
  health_check {
    enabled             = true
    healthy_threshold   = 2
    unhealthy_threshold = 2
    timeout             = 5
    interval            = 30
    path                = "/health"
    matcher             = "200"
  }
  
  tags = {
    Name = "app-target-group"
  }
}

# Target Group Attachment
resource "aws_lb_target_group_attachment" "app" {
  count = length(aws_instance.app)
  
  target_group_arn = aws_lb_target_group.app.arn
  target_id        = aws_instance.app[count.index].id
  port             = 80
}

# Listener (HTTP)
resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.main.arn
  port              = 80
  protocol          = "HTTP"
  
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.app.arn
  }
}

# Listener (HTTPS)
resource "aws_lb_listener" "https" {
  load_balancer_arn = aws_lb.main.arn
  port              = 443
  protocol          = "HTTPS"
  ssl_policy        = "ELBSecurityPolicy-2016-08"
  certificate_arn   = aws_acm_certificate.main.arn
  
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.app.arn
  }
}

# Listener Rule (Path-based routing)
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

output "alb_dns_name" {
  value = aws_lb.main.dns_name
}
```

### Network Load Balancer

```hcl
resource "aws_lb" "network" {
  name               = "main-nlb"
  internal           = false
  load_balancer_type = "network"
  subnets            = aws_subnet.public[*].id
  
  enable_deletion_protection = false
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

---

## 7. Auto Scaling Group

```hcl
# Launch Template
resource "aws_launch_template" "app" {
  name_prefix   = "app-"
  image_id      = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  vpc_security_group_ids = [aws_security_group.app.id]
  
  iam_instance_profile {
    name = aws_iam_instance_profile.ec2_profile.name
  }
  
  block_device_mappings {
    device_name = "/dev/sda1"
    
    ebs {
      volume_size           = 20
      volume_type           = "gp3"
      delete_on_termination = true
      encrypted             = true
    }
  }
  
  user_data = base64encode(templatefile("${path.module}/user_data.sh", {
    environment = var.environment
  }))
  
  tag_specifications {
    resource_type = "instance"
    
    tags = {
      Name = "app-server"
    }
  }
}

# Auto Scaling Group
resource "aws_autoscaling_group" "app" {
  name                = "app-asg"
  vpc_zone_identifier = aws_subnet.private[*].id
  target_group_arns   = [aws_lb_target_group.app.arn]
  health_check_type   = "ELB"
  health_check_grace_period = 300
  
  min_size         = 2
  max_size         = 10
  desired_capacity = 3
  
  launch_template {
    id      = aws_launch_template.app.id
    version = "$Latest"
  }
  
  tag {
    key                 = "Name"
    value               = "app-server"
    propagate_at_launch = true
  }
}

# Auto Scaling Policy (Target Tracking)
resource "aws_autoscaling_policy" "cpu" {
  name                   = "cpu-target-tracking"
  autoscaling_group_name = aws_autoscaling_group.app.name
  policy_type            = "TargetTrackingScaling"
  
  target_tracking_configuration {
    predefined_metric_specification {
      predefined_metric_type = "ASGAverageCPUUtilization"
    }
    target_value = 70.0
  }
}

# Auto Scaling Policy (Step Scaling)
resource "aws_autoscaling_policy" "scale_up" {
  name                   = "scale-up"
  autoscaling_group_name = aws_autoscaling_group.app.name
  adjustment_type        = "ChangeInCapacity"
  scaling_adjustment     = 2
  cooldown               = 300
}

resource "aws_cloudwatch_metric_alarm" "high_cpu" {
  alarm_name          = "high-cpu"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 120
  statistic           = "Average"
  threshold           = 80
  
  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.app.name
  }
  
  alarm_actions = [aws_autoscaling_policy.scale_up.arn]
}
```

---

## 8. RDS (PostgreSQL)

```hcl
# DB Subnet Group
resource "aws_db_subnet_group" "main" {
  name       = "main-db-subnet-group"
  subnet_ids = aws_subnet.private[*].id
  
  tags = {
    Name = "main-db-subnet-group"
  }
}

# DB Parameter Group
resource "aws_db_parameter_group" "postgres" {
  name   = "postgres-params"
  family = "postgres14"
  
  parameter {
    name  = "log_connections"
    value = "1"
  }
  
  parameter {
    name  = "log_disconnections"
    value = "1"
  }
}

# RDS Instance
resource "aws_db_instance" "main" {
  identifier     = "main-db"
  engine         = "postgres"
  engine_version = "14.7"
  instance_class = "db.t3.micro"
  
  allocated_storage     = 20
  max_allocated_storage = 100
  storage_type          = "gp3"
  storage_encrypted     = true
  
  db_name  = "myapp"
  username = "admin"
  password = random_password.db_password.result
  
  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [aws_security_group.db.id]
  parameter_group_name   = aws_db_parameter_group.postgres.name
  
  backup_retention_period = 7
  backup_window           = "03:00-04:00"
  maintenance_window      = "mon:04:00-mon:05:00"
  
  enabled_cloudwatch_logs_exports = ["postgresql", "upgrade"]
  
  skip_final_snapshot       = false
  final_snapshot_identifier = "main-db-final-snapshot-${formatdate("YYYY-MM-DD-hhmm", timestamp())}"
  
  tags = {
    Name = "main-db"
  }
}

# Random password for database
resource "random_password" "db_password" {
  length  = 16
  special = true
}

# Store password in Secrets Manager
resource "aws_secretsmanager_secret" "db_password" {
  name = "main-db-password"
}

resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = jsonencode({
    username = aws_db_instance.main.username
    password = random_password.db_password.result
    engine   = "postgres"
    host     = aws_db_instance.main.endpoint
    port     = 5432
    dbname   = aws_db_instance.main.db_name
  })
}

output "db_endpoint" {
  value = aws_db_instance.main.endpoint
}
```

---

## 9. S3 Buckets

```hcl
# S3 Bucket
resource "aws_s3_bucket" "data" {
  bucket = "my-app-data-${random_id.bucket_suffix.hex}"
  
  tags = {
    Name = "data-bucket"
  }
}

resource "random_id" "bucket_suffix" {
  byte_length = 4
}

# Bucket Versioning
resource "aws_s3_bucket_versioning" "data" {
  bucket = aws_s3_bucket.data.id
  
  versioning_configuration {
    status = "Enabled"
  }
}

# Bucket Encryption
resource "aws_s3_bucket_server_side_encryption_configuration" "data" {
  bucket = aws_s3_bucket.data.id
  
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

# Bucket Public Access Block
resource "aws_s3_bucket_public_access_block" "data" {
  bucket = aws_s3_bucket.data.id
  
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

# Bucket Lifecycle Policy
resource "aws_s3_bucket_lifecycle_configuration" "data" {
  bucket = aws_s3_bucket.data.id
  
  rule {
    id     = "archive-old-objects"
    status = "Enabled"
    
    transition {
      days          = 30
      storage_class = "STANDARD_IA"
    }
    
    transition {
      days          = 90
      storage_class = "GLACIER"
    }
    
    expiration {
      days = 365
    }
  }
  
  rule {
    id     = "delete-old-versions"
    status = "Enabled"
    
    noncurrent_version_expiration {
      noncurrent_days = 90
    }
  }
}

# Bucket Policy
resource "aws_s3_bucket_policy" "data" {
  bucket = aws_s3_bucket.data.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Sid    = "AllowEC2Access"
      Effect = "Allow"
      Principal = {
        AWS = aws_iam_role.ec2_role.arn
      }
      Action = [
        "s3:GetObject",
        "s3:PutObject"
      ]
      Resource = "${aws_s3_bucket.data.arn}/*"
    }]
  })
}

output "bucket_name" {
  value = aws_s3_bucket.data.id
}
```

---

## 10. CloudWatch

```hcl
# CloudWatch Log Group
resource "aws_cloudwatch_log_group" "app" {
  name              = "/aws/app/logs"
  retention_in_days = 7
  
  tags = {
    Name = "app-logs"
  }
}

# CloudWatch Metric Alarm
resource "aws_cloudwatch_metric_alarm" "high_cpu" {
  alarm_name          = "high-cpu-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "This metric monitors ec2 cpu utilization"
  
  dimensions = {
    InstanceId = aws_instance.web.id
  }
  
  alarm_actions = [aws_sns_topic.alerts.arn]
}

# SNS Topic for Alerts
resource "aws_sns_topic" "alerts" {
  name = "cloudwatch-alerts"
}

resource "aws_sns_topic_subscription" "email" {
  topic_arn = aws_sns_topic.alerts.arn
  protocol  = "email"
  endpoint  = "admin@example.com"
}

# CloudWatch Dashboard
resource "aws_cloudwatch_dashboard" "main" {
  dashboard_name = "main-dashboard"
  
  dashboard_body = jsonencode({
    widgets = [
      {
        type = "metric"
        properties = {
          metrics = [
            ["AWS/EC2", "CPUUtilization", { stat = "Average" }]
          ]
          period = 300
          stat   = "Average"
          region = "us-east-1"
          title  = "EC2 CPU Utilization"
        }
      }
    ]
  })
}
```

---

## Complete 3-Tier Architecture Example

This example will be included in Phase 15 (Capstone Projects) as a complete working project.

---

## Best Practices

1. **Use data sources for AMIs** - Always get latest AMI
2. **Enable encryption** - Encrypt EBS volumes and S3 buckets
3. **Use IAM roles** - Don't hardcode credentials
4. **Tag everything** - Use consistent tagging strategy
5. **Use private subnets** - Keep app/db in private subnets
6. **Enable CloudWatch** - Monitor all resources
7. **Use Auto Scaling** - For high availability
8. **Backup regularly** - Enable RDS backups
9. **Use Secrets Manager** - Store sensitive data securely
10. **Follow least privilege** - Minimal IAM permissions


# Phase 10: Terraform with Kubernetes

## 1. Kubernetes Provider

### Provider Configuration

```hcl
terraform {
  required_providers {
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "~> 2.23"
    }
  }
}

# Using kubeconfig file
provider "kubernetes" {
  config_path    = "~/.kube/config"
  config_context = "my-cluster"
}

# Using in-cluster configuration
provider "kubernetes" {
  host                   = var.cluster_endpoint
  cluster_ca_certificate = base64decode(var.cluster_ca_cert)
  token                  = var.cluster_token
}

# Using AWS EKS
data "aws_eks_cluster" "cluster" {
  name = var.cluster_name
}

data "aws_eks_cluster_auth" "cluster" {
  name = var.cluster_name
}

provider "kubernetes" {
  host                   = data.aws_eks_cluster.cluster.endpoint
  cluster_ca_certificate = base64decode(data.aws_eks_cluster.cluster.certificate_authority[0].data)
  token                  = data.aws_eks_cluster_auth.cluster.token
}
```

---

## 2. Creating Namespaces

```hcl
# Simple Namespace
resource "kubernetes_namespace" "dev" {
  metadata {
    name = "dev"
    
    labels = {
      environment = "development"
      managed_by  = "terraform"
    }
    
    annotations = {
      description = "Development environment"
    }
  }
}

# Multiple Namespaces
resource "kubernetes_namespace" "environments" {
  for_each = toset(["dev", "staging", "prod"])
  
  metadata {
    name = each.value
    
    labels = {
      environment = each.value
      managed_by  = "terraform"
    }
  }
}

# Namespace with Resource Quota
resource "kubernetes_namespace" "limited" {
  metadata {
    name = "limited-namespace"
  }
}

resource "kubernetes_resource_quota" "limited" {
  metadata {
    name      = "resource-quota"
    namespace = kubernetes_namespace.limited.metadata[0].name
  }
  
  spec {
    hard = {
      "requests.cpu"    = "10"
      "requests.memory" = "10Gi"
      "limits.cpu"      = "20"
      "limits.memory"   = "20Gi"
      pods              = "50"
    }
  }
}
```

---

## 3. Deployments

### Basic Deployment

```hcl
resource "kubernetes_deployment" "nginx" {
  metadata {
    name      = "nginx-deployment"
    namespace = kubernetes_namespace.dev.metadata[0].name
    
    labels = {
      app = "nginx"
    }
  }
  
  spec {
    replicas = 3
    
    selector {
      match_labels = {
        app = "nginx"
      }
    }
    
    template {
      metadata {
        labels = {
          app = "nginx"
        }
      }
      
      spec {
        container {
          name  = "nginx"
          image = "nginx:1.25"
          
          port {
            container_port = 80
          }
          
          resources {
            requests = {
              cpu    = "100m"
              memory = "128Mi"
            }
            limits = {
              cpu    = "500m"
              memory = "512Mi"
            }
          }
          
          liveness_probe {
            http_get {
              path = "/"
              port = 80
            }
            initial_delay_seconds = 30
            period_seconds        = 10
          }
          
          readiness_probe {
            http_get {
              path = "/"
              port = 80
            }
            initial_delay_seconds = 5
            period_seconds        = 5
          }
        }
      }
    }
  }
}
```

### Advanced Deployment

```hcl
resource "kubernetes_deployment" "app" {
  metadata {
    name      = "app-deployment"
    namespace = kubernetes_namespace.dev.metadata[0].name
    
    labels = {
      app     = "myapp"
      version = var.app_version
    }
  }
  
  spec {
    replicas = var.replicas
    
    strategy {
      type = "RollingUpdate"
      
      rolling_update {
        max_surge       = "25%"
        max_unavailable = "25%"
      }
    }
    
    selector {
      match_labels = {
        app = "myapp"
      }
    }
    
    template {
      metadata {
        labels = {
          app     = "myapp"
          version = var.app_version
        }
        
        annotations = {
          "prometheus.io/scrape" = "true"
          "prometheus.io/port"   = "8080"
        }
      }
      
      spec {
        service_account_name = kubernetes_service_account.app.metadata[0].name
        
        init_container {
          name  = "init-db"
          image = "busybox:1.35"
          
          command = ["sh", "-c", "until nc -z ${var.db_host} 5432; do echo waiting for db; sleep 2; done"]
        }
        
        container {
          name  = "app"
          image = "myapp:${var.app_version}"
          
          port {
            name           = "http"
            container_port = 8080
            protocol       = "TCP"
          }
          
          env {
            name  = "ENVIRONMENT"
            value = var.environment
          }
          
          env {
            name = "DB_PASSWORD"
            value_from {
              secret_key_ref {
                name = kubernetes_secret.db_credentials.metadata[0].name
                key  = "password"
              }
            }
          }
          
          env_from {
            config_map_ref {
              name = kubernetes_config_map.app_config.metadata[0].name
            }
          }
          
          volume_mount {
            name       = "config"
            mount_path = "/etc/config"
            read_only  = true
          }
          
          volume_mount {
            name       = "data"
            mount_path = "/data"
          }
          
          resources {
            requests = {
              cpu    = "200m"
              memory = "256Mi"
            }
            limits = {
              cpu    = "1000m"
              memory = "1Gi"
            }
          }
          
          liveness_probe {
            http_get {
              path = "/health"
              port = 8080
            }
            initial_delay_seconds = 30
            period_seconds        = 10
            timeout_seconds       = 5
            failure_threshold     = 3
          }
          
          readiness_probe {
            http_get {
              path = "/ready"
              port = 8080
            }
            initial_delay_seconds = 10
            period_seconds        = 5
            timeout_seconds       = 3
            failure_threshold     = 3
          }
        }
        
        volume {
          name = "config"
          config_map {
            name = kubernetes_config_map.app_config.metadata[0].name
          }
        }
        
        volume {
          name = "data"
          persistent_volume_claim {
            claim_name = kubernetes_persistent_volume_claim.app_data.metadata[0].name
          }
        }
      }
    }
  }
}
```

---

## 4. Services

### ClusterIP Service

```hcl
resource "kubernetes_service" "app" {
  metadata {
    name      = "app-service"
    namespace = kubernetes_namespace.dev.metadata[0].name
    
    labels = {
      app = "myapp"
    }
  }
  
  spec {
    type = "ClusterIP"
    
    selector = {
      app = "myapp"
    }
    
    port {
      name        = "http"
      port        = 80
      target_port = 8080
      protocol    = "TCP"
    }
    
    session_affinity = "ClientIP"
  }
}
```

### NodePort Service

```hcl
resource "kubernetes_service" "nodeport" {
  metadata {
    name      = "app-nodeport"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  spec {
    type = "NodePort"
    
    selector = {
      app = "myapp"
    }
    
    port {
      port        = 80
      target_port = 8080
      node_port   = 30080
    }
  }
}
```

### LoadBalancer Service

```hcl
resource "kubernetes_service" "loadbalancer" {
  metadata {
    name      = "app-lb"
    namespace = kubernetes_namespace.dev.metadata[0].name
    
    annotations = {
      "service.beta.kubernetes.io/aws-load-balancer-type" = "nlb"
    }
  }
  
  spec {
    type = "LoadBalancer"
    
    selector = {
      app = "myapp"
    }
    
    port {
      port        = 80
      target_port = 8080
    }
    
    load_balancer_source_ranges = ["0.0.0.0/0"]
  }
}

output "loadbalancer_ip" {
  value = kubernetes_service.loadbalancer.status[0].load_balancer[0].ingress[0].ip
}
```

---

## 5. ConfigMaps

```hcl
# Simple ConfigMap
resource "kubernetes_config_map" "app_config" {
  metadata {
    name      = "app-config"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  data = {
    APP_NAME        = "MyApp"
    LOG_LEVEL       = "info"
    MAX_CONNECTIONS = "100"
  }
}

# ConfigMap from file
resource "kubernetes_config_map" "nginx_config" {
  metadata {
    name      = "nginx-config"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  data = {
    "nginx.conf" = file("${path.module}/configs/nginx.conf")
  }
}

# ConfigMap with binary data
resource "kubernetes_config_map" "binary_config" {
  metadata {
    name      = "binary-config"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  binary_data = {
    "config.bin" = filebase64("${path.module}/configs/config.bin")
  }
}

# Using ConfigMap in Deployment
resource "kubernetes_deployment" "app_with_config" {
  metadata {
    name      = "app-with-config"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  spec {
    replicas = 1
    
    selector {
      match_labels = {
        app = "myapp"
      }
    }
    
    template {
      metadata {
        labels = {
          app = "myapp"
        }
      }
      
      spec {
        container {
          name  = "app"
          image = "myapp:latest"
          
          # Load all config as environment variables
          env_from {
            config_map_ref {
              name = kubernetes_config_map.app_config.metadata[0].name
            }
          }
          
          # Load specific config as environment variable
          env {
            name = "SPECIFIC_CONFIG"
            value_from {
              config_map_key_ref {
                name = kubernetes_config_map.app_config.metadata[0].name
                key  = "LOG_LEVEL"
              }
            }
          }
          
          # Mount config as file
          volume_mount {
            name       = "config"
            mount_path = "/etc/config"
          }
        }
        
        volume {
          name = "config"
          config_map {
            name = kubernetes_config_map.nginx_config.metadata[0].name
          }
        }
      }
    }
  }
}
```

---

## 6. Secrets

```hcl
# Basic Secret
resource "kubernetes_secret" "db_credentials" {
  metadata {
    name      = "db-credentials"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  type = "Opaque"
  
  data = {
    username = base64encode("admin")
    password = base64encode(random_password.db_password.result)
  }
}

# TLS Secret
resource "kubernetes_secret" "tls_cert" {
  metadata {
    name      = "tls-secret"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  type = "kubernetes.io/tls"
  
  data = {
    "tls.crt" = filebase64("${path.module}/certs/tls.crt")
    "tls.key" = filebase64("${path.module}/certs/tls.key")
  }
}

# Docker Registry Secret
resource "kubernetes_secret" "docker_registry" {
  metadata {
    name      = "docker-registry-secret"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  type = "kubernetes.io/dockerconfigjson"
  
  data = {
    ".dockerconfigjson" = jsonencode({
      auths = {
        "https://index.docker.io/v1/" = {
          username = var.docker_username
          password = var.docker_password
          email    = var.docker_email
          auth     = base64encode("${var.docker_username}:${var.docker_password}")
        }
      }
    })
  }
}

# Using Secret in Deployment
resource "kubernetes_deployment" "app_with_secrets" {
  metadata {
    name      = "app-with-secrets"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  spec {
    replicas = 1
    
    selector {
      match_labels = {
        app = "myapp"
      }
    }
    
    template {
      metadata {
        labels = {
          app = "myapp"
        }
      }
      
      spec {
        # Use docker registry secret
        image_pull_secrets {
          name = kubernetes_secret.docker_registry.metadata[0].name
        }
        
        container {
          name  = "app"
          image = "myapp:latest"
          
          # Load secret as environment variable
          env {
            name = "DB_USERNAME"
            value_from {
              secret_key_ref {
                name = kubernetes_secret.db_credentials.metadata[0].name
                key  = "username"
              }
            }
          }
          
          env {
            name = "DB_PASSWORD"
            value_from {
              secret_key_ref {
                name = kubernetes_secret.db_credentials.metadata[0].name
                key  = "password"
              }
            }
          }
          
          # Mount secret as file
          volume_mount {
            name       = "tls"
            mount_path = "/etc/tls"
            read_only  = true
          }
        }
        
        volume {
          name = "tls"
          secret {
            secret_name = kubernetes_secret.tls_cert.metadata[0].name
          }
        }
      }
    }
  }
}

resource "random_password" "db_password" {
  length  = 16
  special = true
}
```

---

## 7. Ingress

```hcl
# Ingress Resource
resource "kubernetes_ingress_v1" "app" {
  metadata {
    name      = "app-ingress"
    namespace = kubernetes_namespace.dev.metadata[0].name
    
    annotations = {
      "kubernetes.io/ingress.class"                = "nginx"
      "cert-manager.io/cluster-issuer"             = "letsencrypt-prod"
      "nginx.ingress.kubernetes.io/rewrite-target" = "/"
    }
  }
  
  spec {
    tls {
      hosts = ["app.example.com"]
      secret_name = "app-tls-secret"
    }
    
    rule {
      host = "app.example.com"
      
      http {
        path {
          path      = "/"
          path_type = "Prefix"
          
          backend {
            service {
              name = kubernetes_service.app.metadata[0].name
              port {
                number = 80
              }
            }
          }
        }
        
        path {
          path      = "/api"
          path_type = "Prefix"
          
          backend {
            service {
              name = kubernetes_service.api.metadata[0].name
              port {
                number = 80
              }
            }
          }
        }
      }
    }
  }
}

# Multiple hosts
resource "kubernetes_ingress_v1" "multi_host" {
  metadata {
    name      = "multi-host-ingress"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  spec {
    rule {
      host = "app1.example.com"
      http {
        path {
          path      = "/"
          path_type = "Prefix"
          backend {
            service {
              name = "app1-service"
              port {
                number = 80
              }
            }
          }
        }
      }
    }
    
    rule {
      host = "app2.example.com"
      http {
        path {
          path      = "/"
          path_type = "Prefix"
          backend {
            service {
              name = "app2-service"
              port {
                number = 80
              }
            }
          }
        }
      }
    }
  }
}
```

---

## 8. Persistent Volumes & Claims

```hcl
# Persistent Volume
resource "kubernetes_persistent_volume" "data" {
  metadata {
    name = "data-pv"
    
    labels = {
      type = "local"
    }
  }
  
  spec {
    capacity = {
      storage = "10Gi"
    }
    
    access_modes = ["ReadWriteOnce"]
    
    persistent_volume_source {
      host_path {
        path = "/mnt/data"
      }
    }
    
    storage_class_name = "manual"
  }
}

# Persistent Volume Claim
resource "kubernetes_persistent_volume_claim" "app_data" {
  metadata {
    name      = "app-data-pvc"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  spec {
    access_modes = ["ReadWriteOnce"]
    
    resources {
      requests = {
        storage = "5Gi"
      }
    }
    
    storage_class_name = "manual"
    
    selector {
      match_labels = {
        type = "local"
      }
    }
  }
}

# Storage Class
resource "kubernetes_storage_class" "fast" {
  metadata {
    name = "fast-storage"
  }
  
  storage_provisioner = "kubernetes.io/aws-ebs"
  
  parameters = {
    type      = "gp3"
    iops      = "3000"
    encrypted = "true"
  }
  
  reclaim_policy         = "Retain"
  allow_volume_expansion = true
  volume_binding_mode    = "WaitForFirstConsumer"
}

# Using PVC in Deployment
resource "kubernetes_deployment" "stateful_app" {
  metadata {
    name      = "stateful-app"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  spec {
    replicas = 1
    
    selector {
      match_labels = {
        app = "stateful-app"
      }
    }
    
    template {
      metadata {
        labels = {
          app = "stateful-app"
        }
      }
      
      spec {
        container {
          name  = "app"
          image = "myapp:latest"
          
          volume_mount {
            name       = "data"
            mount_path = "/data"
          }
        }
        
        volume {
          name = "data"
          persistent_volume_claim {
            claim_name = kubernetes_persistent_volume_claim.app_data.metadata[0].name
          }
        }
      }
    }
  }
}
```

---

## 9. StatefulSets

```hcl
resource "kubernetes_stateful_set" "postgres" {
  metadata {
    name      = "postgres"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  spec {
    service_name = "postgres"
    replicas     = 3
    
    selector {
      match_labels = {
        app = "postgres"
      }
    }
    
    template {
      metadata {
        labels = {
          app = "postgres"
        }
      }
      
      spec {
        container {
          name  = "postgres"
          image = "postgres:15"
          
          port {
            container_port = 5432
            name           = "postgres"
          }
          
          env {
            name  = "POSTGRES_PASSWORD"
            value = "password"
          }
          
          env {
            name  = "PGDATA"
            value = "/var/lib/postgresql/data/pgdata"
          }
          
          volume_mount {
            name       = "postgres-data"
            mount_path = "/var/lib/postgresql/data"
          }
        }
      }
    }
    
    volume_claim_template {
      metadata {
        name = "postgres-data"
      }
      
      spec {
        access_modes = ["ReadWriteOnce"]
        
        resources {
          requests = {
            storage = "10Gi"
          }
        }
        
        storage_class_name = "fast-storage"
      }
    }
  }
}

# Headless Service for StatefulSet
resource "kubernetes_service" "postgres_headless" {
  metadata {
    name      = "postgres"
    namespace = kubernetes_namespace.dev.metadata[0].name
  }
  
  spec {
    cluster_ip = "None"
    
    selector = {
      app = "postgres"
    }
    
    port {
      port        = 5432
      target_port = 5432
    }
  }
}
```

---

## 10. Helm Provider

```hcl
terraform {
  required_providers {
    helm = {
      source  = "hashicorp/helm"
      version = "~> 2.11"
    }
  }
}

provider "helm" {
  kubernetes {
    config_path = "~/.kube/config"
  }
}

# Install Helm Chart
resource "helm_release" "nginx_ingress" {
  name       = "nginx-ingress"
  repository = "https://kubernetes.github.io/ingress-nginx"
  chart      = "ingress-nginx"
  version    = "4.8.0"
  namespace  = "ingress-nginx"
  
  create_namespace = true
  
  set {
    name  = "controller.service.type"
    value = "LoadBalancer"
  }
  
  set {
    name  = "controller.metrics.enabled"
    value = "true"
  }
  
  values = [
    file("${path.module}/helm-values/nginx-ingress.yaml")
  ]
}

# Install cert-manager
resource "helm_release" "cert_manager" {
  name       = "cert-manager"
  repository = "https://charts.jetstack.io"
  chart      = "cert-manager"
  version    = "v1.13.0"
  namespace  = "cert-manager"
  
  create_namespace = true
  
  set {
    name  = "installCRDs"
    value = "true"
  }
}

# Install Prometheus
resource "helm_release" "prometheus" {
  name       = "prometheus"
  repository = "https://prometheus-community.github.io/helm-charts"
  chart      = "kube-prometheus-stack"
  version    = "51.0.0"
  namespace  = "monitoring"
  
  create_namespace = true
  
  values = [
    yamlencode({
      prometheus = {
        prometheusSpec = {
          retention = "30d"
          storageSpec = {
            volumeClaimTemplate = {
              spec = {
                accessModes = ["ReadWriteOnce"]
                resources = {
                  requests = {
                    storage = "50Gi"
                  }
                }
              }
            }
          }
        }
      }
      grafana = {
        enabled = true
        adminPassword = var.grafana_password
      }
    })
  ]
}
```

---

## 11. Complete Microservices Example

```hcl
# variables.tf
variable "environment" {
  type = string
}

variable "app_version" {
  type = string
}

# main.tf
resource "kubernetes_namespace" "app" {
  metadata {
    name = var.environment
  }
}

# Database
resource "kubernetes_stateful_set" "db" {
  metadata {
    name      = "postgres"
    namespace = kubernetes_namespace.app.metadata[0].name
  }
  
  spec {
    service_name = "postgres"
    replicas     = 1
    
    selector {
      match_labels = {
        app = "postgres"
      }
    }
    
    template {
      metadata {
        labels = {
          app = "postgres"
        }
      }
      
      spec {
        container {
          name  = "postgres"
          image = "postgres:15"
          
          env {
            name = "POSTGRES_PASSWORD"
            value_from {
              secret_key_ref {
                name = kubernetes_secret.db_credentials.metadata[0].name
                key  = "password"
              }
            }
          }
          
          port {
            container_port = 5432
          }
          
          volume_mount {
            name       = "postgres-data"
            mount_path = "/var/lib/postgresql/data"
          }
        }
      }
    }
    
    volume_claim_template {
      metadata {
        name = "postgres-data"
      }
      spec {
        access_modes = ["ReadWriteOnce"]
        resources {
          requests = {
            storage = "10Gi"
          }
        }
      }
    }
  }
}

resource "kubernetes_service" "db" {
  metadata {
    name      = "postgres"
    namespace = kubernetes_namespace.app.metadata[0].name
  }
  
  spec {
    cluster_ip = "None"
    selector = {
      app = "postgres"
    }
    port {
      port = 5432
    }
  }
}

# Backend API
resource "kubernetes_deployment" "api" {
  metadata {
    name      = "api"
    namespace = kubernetes_namespace.app.metadata[0].name
  }
  
  spec {
    replicas = 3
    
    selector {
      match_labels = {
        app = "api"
      }
    }
    
    template {
      metadata {
        labels = {
          app = "api"
        }
      }
      
      spec {
        container {
          name  = "api"
          image = "myapi:${var.app_version}"
          
          port {
            container_port = 8080
          }
          
          env_from {
            config_map_ref {
              name = kubernetes_config_map.api_config.metadata[0].name
            }
          }
          
          env {
            name = "DB_PASSWORD"
            value_from {
              secret_key_ref {
                name = kubernetes_secret.db_credentials.metadata[0].name
                key  = "password"
              }
            }
          }
          
          resources {
            requests = {
              cpu    = "200m"
              memory = "256Mi"
            }
            limits = {
              cpu    = "1000m"
              memory = "1Gi"
            }
          }
        }
      }
    }
  }
}

resource "kubernetes_service" "api" {
  metadata {
    name      = "api"
    namespace = kubernetes_namespace.app.metadata[0].name
  }
  
  spec {
    selector = {
      app = "api"
    }
    port {
      port        = 80
      target_port = 8080
    }
  }
}

# Frontend
resource "kubernetes_deployment" "frontend" {
  metadata {
    name      = "frontend"
    namespace = kubernetes_namespace.app.metadata[0].name
  }
  
  spec {
    replicas = 2
    
    selector {
      match_labels = {
        app = "frontend"
      }
    }
    
    template {
      metadata {
        labels = {
          app = "frontend"
        }
      }
      
      spec {
        container {
          name  = "frontend"
          image = "myfrontend:${var.app_version}"
          
          port {
            container_port = 80
          }
          
          resources {
            requests = {
              cpu    = "100m"
              memory = "128Mi"
            }
            limits = {
              cpu    = "500m"
              memory = "512Mi"
            }
          }
        }
      }
    }
  }
}

resource "kubernetes_service" "frontend" {
  metadata {
    name      = "frontend"
    namespace = kubernetes_namespace.app.metadata[0].name
  }
  
  spec {
    selector = {
      app = "frontend"
    }
    port {
      port        = 80
      target_port = 80
    }
  }
}

# Ingress
resource "kubernetes_ingress_v1" "app" {
  metadata {
    name      = "app-ingress"
    namespace = kubernetes_namespace.app.metadata[0].name
    
    annotations = {
      "kubernetes.io/ingress.class" = "nginx"
    }
  }
  
  spec {
    rule {
      host = "app.example.com"
      
      http {
        path {
          path      = "/api"
          path_type = "Prefix"
          backend {
            service {
              name = kubernetes_service.api.metadata[0].name
              port {
                number = 80
              }
            }
          }
        }
        
        path {
          path      = "/"
          path_type = "Prefix"
          backend {
            service {
              name = kubernetes_service.frontend.metadata[0].name
              port {
                number = 80
              }
            }
          }
        }
      }
    }
  }
}

# ConfigMap
resource "kubernetes_config_map" "api_config" {
  metadata {
    name      = "api-config"
    namespace = kubernetes_namespace.app.metadata[0].name
  }
  
  data = {
    DB_HOST = "postgres.${kubernetes_namespace.app.metadata[0].name}.svc.cluster.local"
    DB_PORT = "5432"
    DB_NAME = "myapp"
  }
}

# Secret
resource "kubernetes_secret" "db_credentials" {
  metadata {
    name      = "db-credentials"
    namespace = kubernetes_namespace.app.metadata[0].name
  }
  
  data = {
    password = base64encode(random_password.db_password.result)
  }
}

resource "random_password" "db_password" {
  length  = 16
  special = true
}
```

---

## Best Practices

1. **Use namespaces** - Isolate resources by environment
2. **Set resource limits** - Always define requests and limits
3. **Use health checks** - Implement liveness and readiness probes
4. **Use ConfigMaps and Secrets** - Don't hardcode configuration
5. **Use labels** - Consistent labeling strategy
6. **Use Helm for complex apps** - Easier management
7. **Enable RBAC** - Implement proper access controls
8. **Use NetworkPolicies** - Control traffic between pods
9. **Monitor everything** - Use Prometheus/Grafana
10. **Version your images** - Never use :latest in production

# Phase 11: CI/CD & Automation

## 1. Terraform in CI/CD

### CI/CD Workflow

```
┌─────────────────────────────────────────────────────────┐
│  1. Code Push                                           │
│  Developer pushes Terraform code to Git                 │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│  2. CI Pipeline Triggered                               │
│  - terraform fmt -check                                 │
│  - terraform validate                                   │
│  - tflint                                               │
│  - terraform plan                                       │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│  3. Review & Approval                                   │
│  Team reviews plan output                               │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│  4. CD Pipeline                                         │
│  - terraform apply -auto-approve                        │
│  - Post-deployment tests                                │
│  - Notifications                                        │
└─────────────────────────────────────────────────────────┘
```

---

## 2. Terraform with GitHub Actions

### Basic Workflow

**.github/workflows/terraform.yml**
```yaml
name: 'Terraform'

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

env:
  AWS_REGION: us-east-1

jobs:
  terraform:
    name: 'Terraform'
    runs-on: ubuntu-latest
    
    defaults:
      run:
        working-directory: ./terraform
    
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: 1.6.0
      
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}
      
      - name: Terraform Format
        id: fmt
        run: terraform fmt -check
        continue-on-error: true
      
      - name: Terraform Init
        id: init
        run: terraform init
      
      - name: Terraform Validate
        id: validate
        run: terraform validate -no-color
      
      - name: Terraform Plan
        id: plan
        if: github.event_name == 'pull_request'
        run: terraform plan -no-color -input=false
        continue-on-error: true
      
      - name: Comment PR
        uses: actions/github-script@v6
        if: github.event_name == 'pull_request'
        env:
          PLAN: "terraform\n${{ steps.plan.outputs.stdout }}"
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const output = `#### Terraform Format and Style 🖌\`${{ steps.fmt.outcome }}\`
            #### Terraform Initialization ⚙️\`${{ steps.init.outcome }}\`
            #### Terraform Validation 🤖\`${{ steps.validate.outcome }}\`
            #### Terraform Plan 📖\`${{ steps.plan.outcome }}\`
            
            <details><summary>Show Plan</summary>
            
            \`\`\`\n
            ${process.env.PLAN}
            \`\`\`
            
            </details>
            
            *Pushed by: @${{ github.actor }}, Action: \`${{ github.event_name }}\`*`;
            
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: output
            })
      
      - name: Terraform Plan Status
        if: steps.plan.outcome == 'failure'
        run: exit 1
      
      - name: Terraform Apply
        if: github.ref == 'refs/heads/main' && github.event_name == 'push'
        run: terraform apply -auto-approve -input=false
```

### Advanced Workflow with Multiple Environments

**.github/workflows/terraform-deploy.yml**
```yaml
name: 'Terraform Deploy'

on:
  push:
    branches:
      - main
      - develop
  pull_request:
    branches:
      - main
      - develop

jobs:
  terraform:
    name: 'Terraform ${{ matrix.environment }}'
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        environment: [dev, staging, prod]
        exclude:
          - environment: prod
            # Only deploy to prod from main branch
        include:
          - environment: dev
            branch: develop
          - environment: staging
            branch: develop
          - environment: prod
            branch: main
    
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: 1.6.0
      
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      
      - name: Terraform Init
        working-directory: ./environments/${{ matrix.environment }}
        run: terraform init
      
      - name: Terraform Plan
        working-directory: ./environments/${{ matrix.environment }}
        run: terraform plan -out=tfplan
      
      - name: Terraform Apply
        if: github.event_name == 'push'
        working-directory: ./environments/${{ matrix.environment }}
        run: terraform apply -auto-approve tfplan
      
      - name: Slack Notification
        if: always()
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Terraform ${{ matrix.environment }}: ${{ job.status }}'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

---

## 3. Terraform with Jenkins

### Jenkinsfile

```groovy
pipeline {
    agent any
    
    parameters {
        choice(
            name: 'ACTION',
            choices: ['plan', 'apply', 'destroy'],
            description: 'Terraform action to perform'
        )
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'staging', 'prod'],
            description: 'Environment to deploy'
        )
    }
    
    environment {
        AWS_CREDENTIALS = credentials('aws-credentials')
        TF_VERSION = '1.6.0'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Setup') {
            steps {
                script {
                    // Install Terraform
                    sh """
                        wget https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip
                        unzip -o terraform_${TF_VERSION}_linux_amd64.zip
                        chmod +x terraform
                        ./terraform version
                    """
                }
            }
        }
        
        stage('Terraform Init') {
            steps {
                dir("environments/${params.ENVIRONMENT}") {
                    sh '../terraform init'
                }
            }
        }
        
        stage('Terraform Validate') {
            steps {
                dir("environments/${params.ENVIRONMENT}") {
                    sh '../terraform validate'
                }
            }
        }
        
        stage('Terraform Plan') {
            steps {
                dir("environments/${params.ENVIRONMENT}") {
                    sh '../terraform plan -out=tfplan'
                }
            }
        }
        
        stage('Approval') {
            when {
                expression { params.ACTION == 'apply' || params.ACTION == 'destroy' }
            }
            steps {
                script {
                    def userInput = input(
                        id: 'userInput',
                        message: "Apply Terraform changes to ${params.ENVIRONMENT}?",
                        parameters: [
                            booleanParam(
                                defaultValue: false,
                                description: 'Check to approve',
                                name: 'APPROVE'
                            )
                        ]
                    )
                    
                    if (!userInput) {
                        error("Deployment not approved")
                    }
                }
            }
        }
        
        stage('Terraform Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                dir("environments/${params.ENVIRONMENT}") {
                    sh '../terraform apply -auto-approve tfplan'
                }
            }
        }
        
        stage('Terraform Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                dir("environments/${params.ENVIRONMENT}") {
                    sh '../terraform destroy -auto-approve'
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            slackSend(
                color: 'good',
                message: "Terraform ${params.ACTION} succeeded for ${params.ENVIRONMENT}"
            )
        }
        failure {
            slackSend(
                color: 'danger',
                message: "Terraform ${params.ACTION} failed for ${params.ENVIRONMENT}"
            )
        }
    }
}
```

### Multibranch Pipeline

```groovy
pipeline {
    agent any
    
    environment {
        AWS_CREDENTIALS = credentials('aws-credentials')
        ENVIRONMENT = "${env.BRANCH_NAME == 'main' ? 'prod' : env.BRANCH_NAME == 'develop' ? 'staging' : 'dev'}"
    }
    
    stages {
        stage('Terraform Init') {
            steps {
                dir("environments/${ENVIRONMENT}") {
                    sh 'terraform init'
                }
            }
        }
        
        stage('Terraform Plan') {
            steps {
                dir("environments/${ENVIRONMENT}") {
                    sh 'terraform plan -out=tfplan'
                }
            }
        }
        
        stage('Terraform Apply') {
            when {
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                dir("environments/${ENVIRONMENT}") {
                    sh 'terraform apply -auto-approve tfplan'
                }
            }
        }
    }
}
```

---

## 4. Terraform with GitLab CI

**.gitlab-ci.yml**
```yaml
image:
  name: hashicorp/terraform:1.6
  entrypoint:
    - '/usr/bin/env'
    - 'PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin'

variables:
  TF_ROOT: ${CI_PROJECT_DIR}/terraform
  TF_STATE_NAME: default

cache:
  paths:
    - ${TF_ROOT}/.terraform

before_script:
  - cd ${TF_ROOT}
  - terraform --version
  - terraform init

stages:
  - validate
  - plan
  - apply
  - destroy

validate:
  stage: validate
  script:
    - terraform validate
  only:
    - merge_requests
    - main

plan:
  stage: plan
  script:
    - terraform plan -out=tfplan
  artifacts:
    paths:
      - ${TF_ROOT}/tfplan
    expire_in: 1 week
  only:
    - merge_requests
    - main

apply:
  stage: apply
  script:
    - terraform apply -auto-approve tfplan
  dependencies:
    - plan
  only:
    - main
  when: manual

destroy:
  stage: destroy
  script:
    - terraform destroy -auto-approve
  only:
    - main
  when: manual
```

### Multi-Environment GitLab CI

**.gitlab-ci.yml**
```yaml
image:
  name: hashicorp/terraform:1.6
  entrypoint: [""]

variables:
  TF_ROOT: ${CI_PROJECT_DIR}

stages:
  - validate
  - plan
  - apply

.terraform_template:
  before_script:
    - cd ${TF_ROOT}/environments/${ENVIRONMENT}
    - terraform init
  cache:
    key: ${ENVIRONMENT}
    paths:
      - ${TF_ROOT}/environments/${ENVIRONMENT}/.terraform

validate:dev:
  extends: .terraform_template
  stage: validate
  variables:
    ENVIRONMENT: dev
  script:
    - terraform validate
  only:
    - develop

plan:dev:
  extends: .terraform_template
  stage: plan
  variables:
    ENVIRONMENT: dev
  script:
    - terraform plan -out=tfplan
  artifacts:
    paths:
      - ${TF_ROOT}/environments/dev/tfplan
  only:
    - develop

apply:dev:
  extends: .terraform_template
  stage: apply
  variables:
    ENVIRONMENT: dev
  script:
    - terraform apply -auto-approve tfplan
  dependencies:
    - plan:dev
  only:
    - develop
  when: manual

validate:prod:
  extends: .terraform_template
  stage: validate
  variables:
    ENVIRONMENT: prod
  script:
    - terraform validate
  only:
    - main

plan:prod:
  extends: .terraform_template
  stage: plan
  variables:
    ENVIRONMENT: prod
  script:
    - terraform plan -out=tfplan
  artifacts:
    paths:
      - ${TF_ROOT}/environments/prod/tfplan
  only:
    - main

apply:prod:
  extends: .terraform_template
  stage: apply
  variables:
    ENVIRONMENT: prod
  script:
    - terraform apply -auto-approve tfplan
  dependencies:
    - plan:prod
  only:
    - main
  when: manual
  environment:
    name: production
```

---

## 5. Remote Execution

### Terraform Cloud

**main.tf**
```hcl
terraform {
  cloud {
    organization = "my-org"
    
    workspaces {
      name = "my-workspace"
    }
  }
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}
```

### Using Multiple Workspaces

```hcl
terraform {
  cloud {
    organization = "my-org"
    
    workspaces {
      tags = ["app:myapp", "env:prod"]
    }
  }
}
```

### Terraform Cloud Variables

```bash
# Set via CLI
terraform cloud workspace set-variables \
  -workspace=my-workspace \
  -var="region=us-east-1" \
  -var="instance_type=t3.micro"

# Set via API
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request POST \
  --data @payload.json \
  https://app.terraform.io/api/v2/workspaces/ws-xxx/vars
```

---

## 6. Terraform Cloud

### Workspace Configuration

```hcl
# Configure Terraform Cloud backend
terraform {
  cloud {
    organization = "my-organization"
    
    workspaces {
      name = "my-app-production"
    }
  }
}

# Or use tags for multiple workspaces
terraform {
  cloud {
    organization = "my-organization"
    
    workspaces {
      tags = ["app:myapp", "env:prod"]
    }
  }
}
```

### VCS Integration

1. Connect VCS (GitHub, GitLab, Bitbucket)
2. Configure workspace to track repository
3. Set auto-apply or manual approval
4. Configure variables and environment variables
5. Set up notifications (Slack, email)

### Run Triggers

```hcl
# Trigger runs in dependent workspaces
resource "tfe_run_trigger" "test" {
  workspace_id  = tfe_workspace.dependent.id
  sourceable_id = tfe_workspace.source.id
}
```

---

## 7. Terraform Enterprise

### Features

- Private module registry
- Sentinel policy as code
- SSO/SAML integration
- Audit logging
- Cost estimation
- Private VCS integration

### Sentinel Policy Example

**policy.sentinel**
```hcl
import "tfplan/v2" as tfplan

# Require all EC2 instances to have specific tags
main = rule {
  all tfplan.resource_changes as _, rc {
    rc.type is "aws_instance" and
    rc.change.after.tags contains "Environment" and
    rc.change.after.tags contains "Owner"
  }
}
```

---

## 8. Policy as Code (Sentinel)

### Basic Policy

**require-tags.sentinel**
```hcl
import "tfplan/v2" as tfplan

# Get all AWS instances
aws_instances = filter tfplan.resource_changes as _, rc {
  rc.type is "aws_instance" and
  rc.mode is "managed" and
  (rc.change.actions contains "create" or rc.change.actions contains "update")
}

# Required tags
required_tags = ["Environment", "Owner", "Project"]

# Validate tags
instance_tags_validated = rule {
  all aws_instances as _, instance {
    all required_tags as tag {
      instance.change.after.tags contains tag
    }
  }
}

# Main rule
main = rule {
  instance_tags_validated
}
```

### Cost Control Policy

**limit-instance-type.sentinel**
```hcl
import "tfplan/v2" as tfplan

# Allowed instance types
allowed_types = ["t3.micro", "t3.small", "t3.medium"]

# Get all EC2 instances
aws_instances = filter tfplan.resource_changes as _, rc {
  rc.type is "aws_instance" and
  rc.mode is "managed" and
  (rc.change.actions contains "create" or rc.change.actions contains "update")
}

# Validate instance types
main = rule {
  all aws_instances as _, instance {
    instance.change.after.instance_type in allowed_types
  }
}
```

### Security Policy

**enforce-encryption.sentinel**
```hcl
import "tfplan/v2" as tfplan

# Get all EBS volumes
ebs_volumes = filter tfplan.resource_changes as _, rc {
  rc.type is "aws_ebs_volume" and
  rc.mode is "managed"
}

# Get all RDS instances
rds_instances = filter tfplan.resource_changes as _, rc {
  rc.type is "aws_db_instance" and
  rc.mode is "managed"
}

# Validate EBS encryption
ebs_encrypted = rule {
  all ebs_volumes as _, volume {
    volume.change.after.encrypted is true
  }
}

# Validate RDS encryption
rds_encrypted = rule {
  all rds_instances as _, instance {
    instance.change.after.storage_encrypted is true
  }
}

# Main rule
main = rule {
  ebs_encrypted and rds_encrypted
}
```

---

## 9. Automated Testing

### Terratest Example

**test/terraform_test.go**
```go
package test

import (
    "testing"
    "github.com/gruntwork-io/terratest/modules/terraform"
    "github.com/stretchr/testify/assert"
)

func TestTerraformBasicExample(t *testing.T) {
    t.Parallel()
    
    terraformOptions := terraform.WithDefaultRetryableErrors(t, &terraform.Options{
        TerraformDir: "../examples/basic",
        Vars: map[string]interface{}{
            "region": "us-east-1",
        },
    })
    
    defer terraform.Destroy(t, terraformOptions)
    
    terraform.InitAndApply(t, terraformOptions)
    
    instanceID := terraform.Output(t, terraformOptions, "instance_id")
    assert.NotEmpty(t, instanceID)
}
```

### Kitchen-Terraform

**.kitchen.yml**
```yaml
---
driver:
  name: terraform
  root_module_directory: test/fixtures/wrapper

provisioner:
  name: terraform

verifier:
  name: terraform
  systems:
    - name: basic
      backend: aws
      controls:
        - operating_system

platforms:
  - name: terraform

suites:
  - name: default
    driver:
      variables:
        region: us-east-1
    verifier:
      systems:
        - name: basic
          backend: aws
          profile_locations:
            - test/integration/default/controls
```

---

## 10. Complete CI/CD Pipeline Example

### GitHub Actions with Multiple Environments

**.github/workflows/terraform-pipeline.yml**
```yaml
name: 'Terraform Pipeline'

on:
  push:
    branches:
      - main
      - develop
  pull_request:

env:
  TF_VERSION: 1.6.0

jobs:
  lint:
    name: 'Lint'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}
      
      - name: Terraform Format Check
        run: terraform fmt -check -recursive
      
      - name: TFLint
        uses: terraform-linters/setup-tflint@v3
        with:
          tflint_version: latest
      
      - run: tflint --init
      - run: tflint -f compact

  plan-dev:
    name: 'Plan Dev'
    runs-on: ubuntu-latest
    needs: lint
    if: github.ref == 'refs/heads/develop'
    steps:
      - uses: actions/checkout@v3
      
      - uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}
      
      - uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      
      - name: Terraform Init
        working-directory: ./environments/dev
        run: terraform init
      
      - name: Terraform Plan
        working-directory: ./environments/dev
        run: terraform plan -out=tfplan
      
      - name: Upload Plan
        uses: actions/upload-artifact@v3
        with:
          name: dev-tfplan
          path: environments/dev/tfplan

  apply-dev:
    name: 'Apply Dev'
    runs-on: ubuntu-latest
    needs: plan-dev
    if: github.ref == 'refs/heads/develop'
    environment:
      name: development
    steps:
      - uses: actions/checkout@v3
      
      - uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}
      
      - uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      
      - name: Download Plan
        uses: actions/download-artifact@v3
        with:
          name: dev-tfplan
          path: environments/dev
      
      - name: Terraform Init
        working-directory: ./environments/dev
        run: terraform init
      
      - name: Terraform Apply
        working-directory: ./environments/dev
        run: terraform apply -auto-approve tfplan

  plan-prod:
    name: 'Plan Prod'
    runs-on: ubuntu-latest
    needs: lint
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      
      - uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}
      
      - uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      
      - name: Terraform Init
        working-directory: ./environments/prod
        run: terraform init
      
      - name: Terraform Plan
        working-directory: ./environments/prod
        run: terraform plan -out=tfplan
      
      - name: Upload Plan
        uses: actions/upload-artifact@v3
        with:
          name: prod-tfplan
          path: environments/prod/tfplan

  apply-prod:
    name: 'Apply Prod'
    runs-on: ubuntu-latest
    needs: plan-prod
    if: github.ref == 'refs/heads/main'
    environment:
      name: production
    steps:
      - uses: actions/checkout@v3
      
      - uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}
      
      - uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      
      - name: Download Plan
        uses: actions/download-artifact@v3
        with:
          name: prod-tfplan
          path: environments/prod
      
      - name: Terraform Init
        working-directory: ./environments/prod
        run: terraform init
      
      - name: Terraform Apply
        working-directory: ./environments/prod
        run: terraform apply -auto-approve tfplan
      
      - name: Slack Notification
        if: always()
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Production deployment: ${{ job.status }}'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

---

## Best Practices

1. **Always run plan before apply** - Review changes
2. **Use remote state** - Team collaboration
3. **Implement approval gates** - For production
4. **Run automated tests** - Validate infrastructure
5. **Use policy as code** - Enforce standards
6. **Enable notifications** - Stay informed
7. **Version lock Terraform** - Consistency
8. **Secure credentials** - Use secrets management
9. **Tag resources** - Track deployments
10. **Monitor pipelines** - Quick issue detection

# Phase 12: Security & Best Practices

## 1. Terraform Security Best Practices

### Security Checklist

- ✅ Never commit secrets to version control
- ✅ Use remote state with encryption
- ✅ Enable state locking
- ✅ Use IAM roles instead of access keys
- ✅ Encrypt all data at rest and in transit
- ✅ Implement least privilege access
- ✅ Use separate AWS accounts for environments
- ✅ Enable MFA for sensitive operations
- ✅ Regular security audits
- ✅ Use private subnets for databases
- ✅ Enable CloudTrail and logging
- ✅ Implement network segmentation

---

## 2. Secrets Management

### Using AWS Secrets Manager

```hcl
# Create secret
resource "aws_secretsmanager_secret" "db_password" {
  name                    = "db-password"
  recovery_window_in_days = 7
  
  tags = {
    Environment = var.environment
  }
}

resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = random_password.db_password.result
}

resource "random_password" "db_password" {
  length  = 32
  special = true
}

# Use secret in RDS
data "aws_secretsmanager_secret_version" "db_password" {
  secret_id = aws_secretsmanager_secret.db_password.id
}

resource "aws_db_instance" "main" {
  identifier = "mydb"
  engine     = "postgres"
  
  username = "admin"
  password = jsondecode(data.aws_secretsmanager_secret_version.db_password.secret_string)["password"]
  
  # Or if secret is just a string
  # password = data.aws_secretsmanager_secret_version.db_password.secret_string
}
```

### Using HashiCorp Vault

```hcl
terraform {
  required_providers {
    vault = {
      source  = "hashicorp/vault"
      version = "~> 3.20"
    }
  }
}

provider "vault" {
  address = "https://vault.example.com"
  token   = var.vault_token
}

# Read secret from Vault
data "vault_generic_secret" "db_credentials" {
  path = "secret/database/credentials"
}

resource "aws_db_instance" "main" {
  identifier = "mydb"
  engine     = "postgres"
  
  username = data.vault_generic_secret.db_credentials.data["username"]
  password = data.vault_generic_secret.db_credentials.data["password"]
}

# Write secret to Vault
resource "vault_generic_secret" "api_key" {
  path = "secret/api/key"
  
  data_json = jsonencode({
    api_key = random_password.api_key.result
  })
}
```

### Using AWS Parameter Store

```hcl
# Store parameter
resource "aws_ssm_parameter" "db_password" {
  name  = "/myapp/${var.environment}/db/password"
  type  = "SecureString"
  value = random_password.db_password.result
  
  tags = {
    Environment = var.environment
  }
}

# Read parameter
data "aws_ssm_parameter" "db_password" {
  name = "/myapp/${var.environment}/db/password"
}

resource "aws_db_instance" "main" {
  identifier = "mydb"
  engine     = "postgres"
  
  username = "admin"
  password = data.aws_ssm_parameter.db_password.value
}
```

### Environment Variables

```hcl
# Never do this
variable "db_password" {
  type    = string
  default = "hardcoded_password"  # ❌ BAD
}

# Better: Use environment variable
variable "db_password" {
  type      = string
  sensitive = true
  # Set via: export TF_VAR_db_password="secret"
}

# Best: Generate and store in secrets manager
resource "random_password" "db_password" {
  length  = 32
  special = true
}

resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = random_password.db_password.result
}
```

---

## 3. Least Privilege IAM

### Terraform IAM Policy

```hcl
# Minimal IAM policy for Terraform
resource "aws_iam_policy" "terraform" {
  name        = "terraform-policy"
  description = "Minimal policy for Terraform"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:Describe*",
          "ec2:CreateTags",
          "ec2:RunInstances",
          "ec2:TerminateInstances",
          "ec2:StopInstances",
          "ec2:StartInstances"
        ]
        Resource = "*"
      },
      {
        Effect = "Allow"
        Action = [
          "s3:GetObject",
          "s3:PutObject",
          "s3:DeleteObject"
        ]
        Resource = "arn:aws:s3:::terraform-state-bucket/*"
      },
      {
        Effect = "Allow"
        Action = [
          "s3:ListBucket"
        ]
        Resource = "arn:aws:s3:::terraform-state-bucket"
      },
      {
        Effect = "Allow"
        Action = [
          "dynamodb:GetItem",
          "dynamodb:PutItem",
          "dynamodb:DeleteItem"
        ]
        Resource = "arn:aws:dynamodb:*:*:table/terraform-state-lock"
      }
    ]
  })
}

# IAM role for EC2 instances
resource "aws_iam_role" "ec2_role" {
  name = "ec2-minimal-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"
      }
    }]
  })
}

# Attach only necessary policies
resource "aws_iam_role_policy" "ec2_s3_readonly" {
  name = "s3-readonly"
  role = aws_iam_role.ec2_role.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Action = [
        "s3:GetObject",
        "s3:ListBucket"
      ]
      Resource = [
        "arn:aws:s3:::my-app-bucket",
        "arn:aws:s3:::my-app-bucket/*"
      ]
    }]
  })
}
```

### Service-Specific IAM Roles

```hcl
# Lambda execution role
resource "aws_iam_role" "lambda_role" {
  name = "lambda-execution-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "lambda.amazonaws.com"
      }
    }]
  })
}

resource "aws_iam_role_policy_attachment" "lambda_basic" {
  role       = aws_iam_role.lambda_role.name
  policy_arn = "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
}

# ECS task execution role
resource "aws_iam_role" "ecs_task_execution" {
  name = "ecs-task-execution-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "ecs-tasks.amazonaws.com"
      }
    }]
  })
}

resource "aws_iam_role_policy_attachment" "ecs_task_execution" {
  role       = aws_iam_role.ecs_task_execution.name
  policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
}
```

---

## 4. Preventing Accidental Deletes

### Lifecycle Prevent Destroy

```hcl
# Protect critical resources
resource "aws_db_instance" "production" {
  identifier = "prod-db"
  engine     = "postgres"
  
  lifecycle {
    prevent_destroy = true
  }
}

resource "aws_s3_bucket" "critical_data" {
  bucket = "critical-data-bucket"
  
  lifecycle {
    prevent_destroy = true
  }
}

# Conditional prevent destroy
resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  lifecycle {
    prevent_destroy = var.environment == "prod" ? true : false
  }
}
```

### Deletion Protection

```hcl
# RDS deletion protection
resource "aws_db_instance" "main" {
  identifier = "mydb"
  engine     = "postgres"
  
  deletion_protection = true
  skip_final_snapshot = false
  final_snapshot_identifier = "mydb-final-snapshot-${formatdate("YYYY-MM-DD-hhmm", timestamp())}"
}

# S3 bucket versioning (soft delete)
resource "aws_s3_bucket" "data" {
  bucket = "my-data-bucket"
}

resource "aws_s3_bucket_versioning" "data" {
  bucket = aws_s3_bucket.data.id
  
  versioning_configuration {
    status = "Enabled"
  }
}

# EC2 termination protection
resource "aws_instance" "protected" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  disable_api_termination = true
}
```

---

## 5. Code Structure Best Practices

### Directory Structure

```
terraform-project/
├── modules/
│   ├── networking/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── README.md
│   ├── compute/
│   └── database/
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfvars
│   │   └── backend.tf
│   ├── staging/
│   └── prod/
├── .gitignore
├── .terraform-version
└── README.md
```

### File Organization

```hcl
# main.tf - Main resources
terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

# variables.tf - Input variables
variable "region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "environment" {
  description = "Environment name"
  type        = string
  
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

# locals.tf - Local values
locals {
  common_tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
    Project     = var.project
  }
  
  name_prefix = "${var.project}-${var.environment}"
}

# outputs.tf - Output values
output "vpc_id" {
  description = "VPC ID"
  value       = module.vpc.vpc_id
}

# data.tf - Data sources
data "aws_availability_zones" "available" {
  state = "available"
}

data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

# versions.tf - Version constraints
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

---

## 6. Naming Standards

### Resource Naming Convention

```hcl
# Pattern: {project}-{environment}-{resource-type}-{description}

locals {
  name_prefix = "${var.project}-${var.environment}"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "${local.name_prefix}-vpc"
  }
}

resource "aws_subnet" "public" {
  count = 3
  
  vpc_id     = aws_vpc.main.id
  cidr_block = cidrsubnet(aws_vpc.main.cidr_block, 8, count.index)
  
  tags = {
    Name = "${local.name_prefix}-public-subnet-${count.index + 1}"
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  tags = {
    Name = "${local.name_prefix}-web-server"
  }
}

resource "aws_security_group" "web" {
  name        = "${local.name_prefix}-web-sg"
  description = "Security group for web servers"
  vpc_id      = aws_vpc.main.id
}
```

### Tagging Strategy

```hcl
locals {
  common_tags = {
    # Required tags
    Environment = var.environment
    Project     = var.project
    ManagedBy   = "Terraform"
    
    # Optional tags
    Owner       = var.owner
    CostCenter  = var.cost_center
    Compliance  = var.compliance_level
  }
}

# Apply to all resources
resource "aws_instance" "app" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  tags = merge(
    local.common_tags,
    {
      Name = "${var.project}-${var.environment}-app-server"
      Role = "application"
    }
  )
}

# Use default_tags in provider
provider "aws" {
  region = var.region
  
  default_tags {
    tags = local.common_tags
  }
}
```

---

## 7. Terraform Linting (tflint)

### Installation

```bash
# macOS
brew install tflint

# Linux
curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash

# Windows
choco install tflint
```

### Configuration

**.tflint.hcl**
```hcl
plugin "aws" {
  enabled = true
  version = "0.27.0"
  source  = "github.com/terraform-linters/tflint-ruleset-aws"
}

rule "terraform_deprecated_interpolation" {
  enabled = true
}

rule "terraform_documented_outputs" {
  enabled = true
}

rule "terraform_documented_variables" {
  enabled = true
}

rule "terraform_naming_convention" {
  enabled = true
  format  = "snake_case"
}

rule "terraform_required_version" {
  enabled = true
}

rule "terraform_required_providers" {
  enabled = true
}

rule "terraform_unused_declarations" {
  enabled = true
}

rule "aws_instance_invalid_type" {
  enabled = true
}

rule "aws_s3_bucket_invalid_acl" {
  enabled = true
}
```

### Usage

```bash
# Initialize tflint
tflint --init

# Run linting
tflint

# Run with specific config
tflint --config=.tflint.hcl

# Run recursively
tflint --recursive

# Fix auto-fixable issues
tflint --fix
```

---

## 8. Terraform Formatting (terraform fmt)

```bash
# Format all files in current directory
terraform fmt

# Format recursively
terraform fmt -recursive

# Check if files are formatted
terraform fmt -check

# Show diff
terraform fmt -diff

# Write to file
terraform fmt -write=true
```

### Pre-commit Hook

**.pre-commit-config.yaml**
```yaml
repos:
  - repo: https://github.com/antonbabenko/pre-commit-terraform
    rev: v1.83.5
    hooks:
      - id: terraform_fmt
      - id: terraform_validate
      - id: terraform_docs
      - id: terraform_tflint
      - id: terraform_tfsec
```

### Git Hook

**.git/hooks/pre-commit**
```bash
#!/bin/bash

echo "Running terraform fmt..."
terraform fmt -check -recursive

if [ $? -ne 0 ]; then
  echo "Terraform files are not formatted. Run 'terraform fmt -recursive'"
  exit 1
fi

echo "Running terraform validate..."
terraform validate

if [ $? -ne 0 ]; then
  echo "Terraform validation failed"
  exit 1
fi

echo "All checks passed!"
```

---

## 9. Terraform Validation (terraform validate)

```bash
# Validate configuration
terraform validate

# Validate with JSON output
terraform validate -json

# Example output
{
  "valid": true,
  "error_count": 0,
  "warning_count": 0,
  "diagnostics": []
}
```

### Validation in CI/CD

```yaml
# GitHub Actions
- name: Terraform Validate
  run: terraform validate -no-color
  
# GitLab CI
validate:
  script:
    - terraform validate
```

---

## 10. Security Scanning

### tfsec

```bash
# Install
brew install tfsec

# Run scan
tfsec .

# Run with specific checks
tfsec --minimum-severity HIGH .

# Exclude checks
tfsec --exclude aws-s3-enable-bucket-logging .

# Output as JSON
tfsec --format json .
```

**.tfsec.yml**
```yaml
minimum_severity: MEDIUM

exclude:
  - aws-s3-enable-bucket-logging
  - aws-s3-enable-versioning

severity_overrides:
  aws-ec2-no-public-ip: ERROR
```

### Checkov

```bash
# Install
pip install checkov

# Run scan
checkov -d .

# Scan specific file
checkov -f main.tf

# Skip specific checks
checkov -d . --skip-check CKV_AWS_20

# Output as JSON
checkov -d . -o json
```

### Terrascan

```bash
# Install
brew install terrascan

# Run scan
terrascan scan

# Scan with specific policy
terrascan scan -p aws

# Output as JSON
terrascan scan -o json
```

---

## 11. Complete Security Example

```hcl
# variables.tf
variable "environment" {
  description = "Environment name"
  type        = string
  
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

variable "allowed_cidr_blocks" {
  description = "Allowed CIDR blocks for SSH access"
  type        = list(string)
  default     = []
}

# main.tf
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    kms_key_id     = "arn:aws:kms:us-east-1:123456789012:key/12345678"
    dynamodb_table = "terraform-state-lock"
  }
}

provider "aws" {
  region = "us-east-1"
  
  default_tags {
    tags = {
      Environment = var.environment
      ManagedBy   = "Terraform"
      Security    = "High"
    }
  }
}

# VPC with flow logs
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name = "${var.environment}-vpc"
  }
}

resource "aws_flow_log" "main" {
  iam_role_arn    = aws_iam_role.flow_log.arn
  log_destination = aws_cloudwatch_log_group.flow_log.arn
  traffic_type    = "ALL"
  vpc_id          = aws_vpc.main.id
}

# Encrypted S3 bucket
resource "aws_s3_bucket" "data" {
  bucket = "${var.environment}-secure-data-${random_id.bucket_suffix.hex}"
  
  lifecycle {
    prevent_destroy = true
  }
}

resource "aws_s3_bucket_versioning" "data" {
  bucket = aws_s3_bucket.data.id
  
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "data" {
  bucket = aws_s3_bucket.data.id
  
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm     = "aws:kms"
      kms_master_key_id = aws_kms_key.s3.arn
    }
  }
}

resource "aws_s3_bucket_public_access_block" "data" {
  bucket = aws_s3_bucket.data.id
  
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

resource "aws_s3_bucket_logging" "data" {
  bucket = aws_s3_bucket.data.id
  
  target_bucket = aws_s3_bucket.logs.id
  target_prefix = "s3-access-logs/"
}

# KMS key for encryption
resource "aws_kms_key" "s3" {
  description             = "KMS key for S3 encryption"
  deletion_window_in_days = 30
  enable_key_rotation     = true
  
  tags = {
    Name = "${var.environment}-s3-key"
  }
}

resource "aws_kms_alias" "s3" {
  name          = "alias/${var.environment}-s3"
  target_key_id = aws_kms_key.s3.key_id
}

# Secrets Manager for passwords
resource "random_password" "db_password" {
  length  = 32
  special = true
}

resource "aws_secretsmanager_secret" "db_password" {
  name                    = "${var.environment}-db-password"
  recovery_window_in_days = 30
  
  tags = {
    Name = "${var.environment}-db-password"
  }
}

resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = random_password.db_password.result
}

# Encrypted RDS instance
resource "aws_db_instance" "main" {
  identifier = "${var.environment}-db"
  engine     = "postgres"
  
  storage_encrypted   = true
  kms_key_id          = aws_kms_key.rds.arn
  deletion_protection = var.environment == "prod"
  
  username = "admin"
  password = random_password.db_password.result
  
  backup_retention_period = 30
  enabled_cloudwatch_logs_exports = ["postgresql", "upgrade"]
  
  lifecycle {
    prevent_destroy = true
  }
}

# Security group with minimal access
resource "aws_security_group" "app" {
  name        = "${var.environment}-app-sg"
  description = "Security group for application"
  vpc_id      = aws_vpc.main.id
  
  # Only allow HTTPS
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
    description = "HTTPS from anywhere"
  }
  
  # SSH only from specific IPs
  dynamic "ingress" {
    for_each = length(var.allowed_cidr_blocks) > 0 ? [1] : []
    content {
      from_port   = 22
      to_port     = 22
      protocol    = "tcp"
      cidr_blocks = var.allowed_cidr_blocks
      description = "SSH from allowed IPs"
    }
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# CloudTrail for audit logging
resource "aws_cloudtrail" "main" {
  name                          = "${var.environment}-trail"
  s3_bucket_name                = aws_s3_bucket.cloudtrail.id
  include_global_service_events = true
  is_multi_region_trail         = true
  enable_log_file_validation    = true
  
  event_selector {
    read_write_type           = "All"
    include_management_events = true
  }
}

resource "random_id" "bucket_suffix" {
  byte_length = 4
}
```

---

## Best Practices Summary

1. **Never commit secrets** - Use secrets management
2. **Enable encryption** - At rest and in transit
3. **Use least privilege** - Minimal IAM permissions
4. **Prevent accidental deletes** - Use lifecycle rules
5. **Follow naming conventions** - Consistent and descriptive
6. **Use linting tools** - tflint, tfsec, checkov
7. **Format code** - terraform fmt
8. **Validate configurations** - terraform validate
9. **Enable logging** - CloudTrail, VPC Flow Logs
10. **Regular security audits** - Automated scanning


# 🟪 Phase 13: Terraform Interview Preparation

This phase prepares you for **real-world Terraform interviews**, focusing on:
- Common interview questions
- Debugging & troubleshooting
- Production readiness checklist

If you can confidently explain everything in this document, you are **job-ready** for Terraform-based roles.

## 1. Common Terraform Interview Questions

### Basic Questions

**Q1: What is Terraform and how does it work?**
```
Terraform is an Infrastructure as Code (IaC) tool that allows you to define and 
provision infrastructure using a declarative configuration language (HCL).

How it works:
1. Write configuration files (.tf)
2. terraform init - Initialize and download providers
3. terraform plan - Preview changes
4. terraform apply - Apply changes to infrastructure
5. State file tracks current infrastructure state
```

**Q2: What is the difference between Terraform and Ansible?**
```
Terraform:
- Declarative (you define desired state)
- Primarily for infrastructure provisioning
- Maintains state file
- Idempotent by design
- Better for cloud infrastructure

Ansible:
- Procedural (you define steps)
- Primarily for configuration management
- Agentless, uses SSH
- Can be idempotent with proper modules
- Better for server configuration
```

**Q3: Explain Terraform state file and its importance.**
```
The state file (terraform.tfstate):
- Maps configuration to real-world resources
- Tracks metadata and dependencies
- Enables resource tracking and updates
- Contains sensitive data (must be secured)
- Essential for team collaboration (remote state)
```

**Q4: What are Terraform providers?**
```
Providers are plugins that interact with APIs of cloud platforms and services.

Examples:
- aws - Amazon Web Services
- azurerm - Microsoft Azure
- google - Google Cloud Platform
- kubernetes - Kubernetes clusters

Each provider offers resources and data sources specific to that platform.
```

### Intermediate Questions

**Q5: Explain the difference between count and for_each.**
```hcl
# count - Creates resources by index
resource "aws_instance" "web" {
  count = 3
  ami   = "ami-0c55b159cbfafe1f0"
  # Resources: aws_instance.web[0], [1], [2]
}

# for_each - Creates resources by key
resource "aws_instance" "web" {
  for_each = toset(["web", "app", "db"])
  ami      = "ami-0c55b159cbfafe1f0"
  # Resources: aws_instance.web["web"], ["app"], ["db"]
}

# Key differences:
# - count uses numeric index, for_each uses string key
# - for_each is more stable (removing middle item doesn't affect others)
# - Use for_each when resources have unique identifiers
```

**Q6: How do you manage secrets in Terraform?**
```hcl
# 1. Environment variables
export TF_VAR_db_password="secret"

# 2. AWS Secrets Manager
data "aws_secretsmanager_secret_version" "db" {
  secret_id = "db-password"
}

# 3. HashiCorp Vault
data "vault_generic_secret" "db" {
  path = "secret/database"
}

# 4. Mark variables as sensitive
variable "db_password" {
  type      = string
  sensitive = true
}

# Never:
# - Hardcode secrets in .tf files
# - Commit secrets to version control
```

**Q7: What are Terraform modules and why use them?**
```hcl
# Modules are reusable containers for resources

# Benefits:
# - Code reusability
# - Encapsulation
# - Standardization
# - Version control

# Using a module
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.1.0"
  
  name = "my-vpc"
  cidr = "10.0.0.0/16"
}

# Module structure:
# modules/vpc/
# ├── main.tf
# ├── variables.tf
# ├── outputs.tf
# └── README.md
```

**Q8: Explain Terraform workspaces.**
```bash
# Workspaces allow multiple state files for same configuration

# Commands
terraform workspace list      # List workspaces
terraform workspace new dev   # Create workspace
terraform workspace select dev # Switch workspace

# Usage in configuration
locals {
  environment = terraform.workspace
  instance_type = {
    dev  = "t3.micro"
    prod = "t3.large"
  }
}

resource "aws_instance" "app" {
  instance_type = local.instance_type[local.environment]
}
```

### Advanced Questions

**Q9: How do you handle state file locking?**
```hcl
# Use remote backend with locking support

terraform {
  backend "s3" {
    bucket         = "terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"  # Enables locking
  }
}

# DynamoDB table for locking
resource "aws_dynamodb_table" "terraform_lock" {
  name         = "terraform-state-lock"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"
  
  attribute {
    name = "LockID"
    type = "S"
  }
}

# Force unlock (use with caution)
# terraform force-unlock <LOCK_ID>
```

**Q10: Explain Terraform's dependency graph.**
```hcl
# Terraform builds a dependency graph to determine order of operations

# Implicit dependency (through reference)
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "public" {
  vpc_id = aws_vpc.main.id  # Creates dependency
}

# Explicit dependency
resource "aws_instance" "app" {
  depends_on = [aws_iam_role_policy.app]
}

# View graph
# terraform graph | dot -Tpng > graph.png
```

**Q11: How do you implement blue-green deployments with Terraform?**
```hcl
variable "active" {
  default = "blue"
}

resource "aws_lb_listener" "main" {
  default_action {
    type = "forward"
    forward {
      target_group {
        arn    = aws_lb_target_group.blue.arn
        weight = var.active == "blue" ? 100 : 0
      }
      target_group {
        arn    = aws_lb_target_group.green.arn
        weight = var.active == "green" ? 100 : 0
      }
    }
  }
}

# Switch by changing variable
# terraform apply -var="active=green"
```

**Q12: What is the difference between terraform import and terraform state mv?**
```bash
# terraform import - Brings existing resource into Terraform management
terraform import aws_instance.web i-0123456789abcdef0
# Use when: Resource exists but not managed by Terraform

# terraform state mv - Moves resource within state
terraform state mv aws_instance.old aws_instance.new
# Use when: Renaming or restructuring Terraform code
```

---

## 2. Debugging Terraform

### Common Debugging Techniques

```bash
# Enable debug logging
export TF_LOG=DEBUG
export TF_LOG_PATH=terraform.log

# Log levels: TRACE, DEBUG, INFO, WARN, ERROR

# Debug specific component
export TF_LOG_CORE=DEBUG
export TF_LOG_PROVIDER=DEBUG

# Disable logging
unset TF_LOG
```

### Terraform Console

```bash
# Start interactive console
terraform console

# Test expressions
> var.environment
"production"

> length(var.availability_zones)
3

> cidrsubnet("10.0.0.0/16", 8, 1)
"10.0.1.0/24"

> [for s in var.subnets : upper(s)]
["SUBNET-1", "SUBNET-2"]
```

### Common Errors and Solutions

```hcl
# Error: Resource already exists
# Solution: Import the resource
terraform import aws_s3_bucket.data my-bucket

# Error: Provider configuration not present
# Solution: Ensure provider is configured
provider "aws" {
  region = "us-east-1"
}

# Error: Invalid count argument
# Solution: Use known values for count
count = var.create ? 1 : 0  # OK
count = length(data.aws_instances.all.ids)  # May fail

# Error: Cycle detected
# Solution: Remove circular dependencies
# Check depends_on and resource references

# Error: State lock
# Solution: Wait or force unlock
terraform force-unlock <LOCK_ID>
```

### Validate and Format

```bash
# Validate configuration
terraform validate

# Format code
terraform fmt -recursive

# Check formatting
terraform fmt -check

# Show plan in detail
terraform plan -out=tfplan
terraform show tfplan
terraform show -json tfplan > plan.json
```

---

## 3. Production Checklist

### Pre-Deployment Checklist

```markdown
## Infrastructure Checklist

### Code Quality
- [ ] terraform fmt -check passes
- [ ] terraform validate passes
- [ ] tflint shows no errors
- [ ] tfsec shows no critical issues
- [ ] All variables have descriptions
- [ ] All outputs have descriptions
- [ ] README.md is up to date

### State Management
- [ ] Remote state configured
- [ ] State locking enabled
- [ ] State encryption enabled
- [ ] Backup strategy in place

### Security
- [ ] No hardcoded secrets
- [ ] Sensitive variables marked
- [ ] IAM follows least privilege
- [ ] Encryption enabled (S3, EBS, RDS)
- [ ] Security groups are restrictive
- [ ] VPC flow logs enabled

### High Availability
- [ ] Multi-AZ deployment
- [ ] Auto Scaling configured
- [ ] Health checks configured
- [ ] Backup retention set

### Monitoring
- [ ] CloudWatch alarms configured
- [ ] Logging enabled
- [ ] Metrics dashboards created

### Documentation
- [ ] Architecture diagram updated
- [ ] Runbook documented
- [ ] Change log updated
```

### Production Best Practices

```hcl
# 1. Use lifecycle rules for critical resources
resource "aws_db_instance" "prod" {
  lifecycle {
    prevent_destroy = true
  }
}

# 2. Enable deletion protection
resource "aws_db_instance" "prod" {
  deletion_protection = true
}

# 3. Use separate state files per environment
terraform {
  backend "s3" {
    key = "${var.environment}/terraform.tfstate"
  }
}

# 4. Tag everything
locals {
  common_tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
    Owner       = var.owner
    CostCenter  = var.cost_center
  }
}

# 5. Use data sources for AMIs
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-*-amd64-server-*"]
  }
}

# 6. Implement proper error handling
variable "environment" {
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}
```

---

## 4. Scenario-Based Questions

### Scenario 1: State File Corruption

**Question:** Your team accidentally deleted the Terraform state file. How do you recover?

**Answer:**
```bash
# Option 1: Restore from S3 versioning
aws s3api list-object-versions --bucket terraform-state --prefix prod/
aws s3api get-object --bucket terraform-state --key prod/terraform.tfstate \
  --version-id <VERSION_ID> terraform.tfstate

# Option 2: Restore from backup
cp terraform.tfstate.backup terraform.tfstate

# Option 3: Re-import all resources
terraform import aws_vpc.main vpc-xxx
terraform import aws_subnet.public subnet-xxx
# ... import all resources

# Option 4: Use terraform state pull (if remote state exists)
terraform state pull > terraform.tfstate
```

### Scenario 2: Handling Drift

**Question:** You notice that someone manually changed an EC2 instance type in AWS console. How do you handle this?

**Answer:**
```bash
# 1. Detect drift
terraform plan -refresh-only

# 2. Options:
# a) Accept the change (update Terraform code)
# b) Revert to Terraform state (apply will fix it)
# c) Import the new state

# To accept the change:
terraform apply -refresh-only

# To revert:
terraform apply  # Will change instance back to configured type
```

### Scenario 3: Zero-Downtime Deployment

**Question:** How would you implement zero-downtime deployment for a web application?

**Answer:**
```hcl
# Use create_before_destroy
resource "aws_launch_template" "app" {
  lifecycle {
    create_before_destroy = true
  }
}

# Use Auto Scaling with rolling updates
resource "aws_autoscaling_group" "app" {
  instance_refresh {
    strategy = "Rolling"
    preferences {
      min_healthy_percentage = 50
    }
  }
}

# Or use blue-green deployment
resource "aws_lb_listener" "main" {
  default_action {
    type = "forward"
    forward {
      target_group {
        arn    = aws_lb_target_group.blue.arn
        weight = var.active == "blue" ? 100 : 0
      }
      target_group {
        arn    = aws_lb_target_group.green.arn
        weight = var.active == "green" ? 100 : 0
      }
    }
  }
}
```

### Scenario 4: Multi-Region Disaster Recovery

**Question:** Design a multi-region DR setup using Terraform.

**Answer:**
```hcl
# Define providers for each region
provider "aws" {
  alias  = "primary"
  region = "us-east-1"
}

provider "aws" {
  alias  = "dr"
  region = "us-west-2"
}

# Primary region resources
module "primary" {
  source = "./modules/infrastructure"
  providers = {
    aws = aws.primary
  }
  
  environment = "prod"
  is_primary  = true
}

# DR region resources
module "dr" {
  source = "./modules/infrastructure"
  providers = {
    aws = aws.dr
  }
  
  environment = "prod-dr"
  is_primary  = false
}

# Cross-region replication for S3
resource "aws_s3_bucket_replication_configuration" "replication" {
  provider = aws.primary
  bucket   = module.primary.bucket_id
  role     = aws_iam_role.replication.arn
  
  rule {
    status = "Enabled"
    destination {
      bucket = module.dr.bucket_arn
    }
  }
}

# RDS read replica in DR region
resource "aws_db_instance" "dr_replica" {
  provider               = aws.dr
  replicate_source_db    = module.primary.db_arn
  instance_class         = "db.t3.large"
  skip_final_snapshot    = true
}
```

---

# Phase 14: Capstone Projects

## Project 1: Complete 3-Tier Web Application

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Internet                              │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                    Application Load Balancer                 │
│                    (Public Subnets)                          │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                    Web Tier (EC2/ASG)                        │
│                    (Public Subnets)                          │
│                    - Nginx                                   │
│                    - Static content                          │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                    App Tier (EC2/ASG)                        │
│                    (Private Subnets)                         │
│                    - Application servers                     │
│                    - Business logic                          │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                    Database Tier (RDS)                       │
│                    (Private Subnets)                         │
│                    - PostgreSQL Multi-AZ                     │
│                    - Read replicas                           │
└─────────────────────────────────────────────────────────────┘
```

### Project Structure

```
3-tier-app/
├── modules/
│   ├── networking/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── security/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── compute/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── database/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfvars
│   │   └── backend.tf
│   └── prod/
│       ├── main.tf
│       ├── variables.tf
│       ├── terraform.tfvars
│       └── backend.tf
├── templates/
│   └── user_data.sh
└── README.md
```

### Implementation

**modules/networking/main.tf**
```hcl
# VPC
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-vpc"
  })
}

# Internet Gateway
resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-igw"
  })
}

# Public Subnets
resource "aws_subnet" "public" {
  count = length(var.availability_zones)
  
  vpc_id                  = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 4, count.index)
  availability_zone       = var.availability_zones[count.index]
  map_public_ip_on_launch = true
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-public-${count.index + 1}"
    Tier = "Public"
  })
}

# Private Subnets (App Tier)
resource "aws_subnet" "private_app" {
  count = length(var.availability_zones)
  
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet(var.vpc_cidr, 4, count.index + 4)
  availability_zone = var.availability_zones[count.index]
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-private-app-${count.index + 1}"
    Tier = "Private-App"
  })
}

# Private Subnets (DB Tier)
resource "aws_subnet" "private_db" {
  count = length(var.availability_zones)
  
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet(var.vpc_cidr, 4, count.index + 8)
  availability_zone = var.availability_zones[count.index]
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-private-db-${count.index + 1}"
    Tier = "Private-DB"
  })
}

# NAT Gateways
resource "aws_eip" "nat" {
  count  = length(var.availability_zones)
  domain = "vpc"
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-nat-eip-${count.index + 1}"
  })
}

resource "aws_nat_gateway" "main" {
  count = length(var.availability_zones)
  
  allocation_id = aws_eip.nat[count.index].id
  subnet_id     = aws_subnet.public[count.index].id
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-nat-${count.index + 1}"
  })
}

# Route Tables
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
  
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-public-rt"
  })
}

resource "aws_route_table" "private" {
  count  = length(var.availability_zones)
  vpc_id = aws_vpc.main.id
  
  route {
    cidr_block     = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.main[count.index].id
  }
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-private-rt-${count.index + 1}"
  })
}

# Route Table Associations
resource "aws_route_table_association" "public" {
  count = length(var.availability_zones)
  
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}

resource "aws_route_table_association" "private_app" {
  count = length(var.availability_zones)
  
  subnet_id      = aws_subnet.private_app[count.index].id
  route_table_id = aws_route_table.private[count.index].id
}

resource "aws_route_table_association" "private_db" {
  count = length(var.availability_zones)
  
  subnet_id      = aws_subnet.private_db[count.index].id
  route_table_id = aws_route_table.private[count.index].id
}
```

**modules/security/main.tf**
```hcl
# ALB Security Group
resource "aws_security_group" "alb" {
  name        = "${var.name_prefix}-alb-sg"
  description = "Security group for ALB"
  vpc_id      = var.vpc_id
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
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
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-alb-sg"
  })
}

# Web Tier Security Group
resource "aws_security_group" "web" {
  name        = "${var.name_prefix}-web-sg"
  description = "Security group for web servers"
  vpc_id      = var.vpc_id
  
  ingress {
    from_port       = 80
    to_port         = 80
    protocol        = "tcp"
    security_groups = [aws_security_group.alb.id]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-web-sg"
  })
}

# App Tier Security Group
resource "aws_security_group" "app" {
  name        = "${var.name_prefix}-app-sg"
  description = "Security group for app servers"
  vpc_id      = var.vpc_id
  
  ingress {
    from_port       = 8080
    to_port         = 8080
    protocol        = "tcp"
    security_groups = [aws_security_group.web.id]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-app-sg"
  })
}

# Database Security Group
resource "aws_security_group" "db" {
  name        = "${var.name_prefix}-db-sg"
  description = "Security group for database"
  vpc_id      = var.vpc_id
  
  ingress {
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [aws_security_group.app.id]
  }
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-db-sg"
  })
}
```

**modules/compute/main.tf**
```hcl
# Data source for AMI
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

# Application Load Balancer
resource "aws_lb" "main" {
  name               = "${var.name_prefix}-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [var.alb_security_group_id]
  subnets            = var.public_subnet_ids
  
  tags = var.tags
}

resource "aws_lb_target_group" "web" {
  name     = "${var.name_prefix}-web-tg"
  port     = 80
  protocol = "HTTP"
  vpc_id   = var.vpc_id
  
  health_check {
    path                = "/health"
    healthy_threshold   = 2
    unhealthy_threshold = 2
  }
  
  tags = var.tags
}

resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.main.arn
  port              = 80
  protocol          = "HTTP"
  
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.web.arn
  }
}

# Web Tier Launch Template
resource "aws_launch_template" "web" {
  name_prefix   = "${var.name_prefix}-web-"
  image_id      = data.aws_ami.ubuntu.id
  instance_type = var.web_instance_type
  
  vpc_security_group_ids = [var.web_security_group_id]
  
  user_data = base64encode(templatefile("${path.module}/../../templates/web_user_data.sh", {
    app_servers = join(",", var.app_server_ips)
  }))
  
  tag_specifications {
    resource_type = "instance"
    tags = merge(var.tags, {
      Name = "${var.name_prefix}-web"
      Tier = "Web"
    })
  }
}

# Web Tier Auto Scaling Group
resource "aws_autoscaling_group" "web" {
  name                = "${var.name_prefix}-web-asg"
  vpc_zone_identifier = var.public_subnet_ids
  target_group_arns   = [aws_lb_target_group.web.arn]
  health_check_type   = "ELB"
  
  min_size         = var.web_min_size
  max_size         = var.web_max_size
  desired_capacity = var.web_desired_capacity
  
  launch_template {
    id      = aws_launch_template.web.id
    version = "$Latest"
  }
  
  tag {
    key                 = "Name"
    value               = "${var.name_prefix}-web"
    propagate_at_launch = true
  }
}

# App Tier Launch Template
resource "aws_launch_template" "app" {
  name_prefix   = "${var.name_prefix}-app-"
  image_id      = data.aws_ami.ubuntu.id
  instance_type = var.app_instance_type
  
  vpc_security_group_ids = [var.app_security_group_id]
  
  user_data = base64encode(templatefile("${path.module}/../../templates/app_user_data.sh", {
    db_host = var.db_endpoint
    db_name = var.db_name
  }))
  
  tag_specifications {
    resource_type = "instance"
    tags = merge(var.tags, {
      Name = "${var.name_prefix}-app"
      Tier = "App"
    })
  }
}

# App Tier Auto Scaling Group
resource "aws_autoscaling_group" "app" {
  name                = "${var.name_prefix}-app-asg"
  vpc_zone_identifier = var.private_app_subnet_ids
  health_check_type   = "EC2"
  
  min_size         = var.app_min_size
  max_size         = var.app_max_size
  desired_capacity = var.app_desired_capacity
  
  launch_template {
    id      = aws_launch_template.app.id
    version = "$Latest"
  }
  
  tag {
    key                 = "Name"
    value               = "${var.name_prefix}-app"
    propagate_at_launch = true
  }
}

# Auto Scaling Policies
resource "aws_autoscaling_policy" "web_scale_up" {
  name                   = "${var.name_prefix}-web-scale-up"
  autoscaling_group_name = aws_autoscaling_group.web.name
  policy_type            = "TargetTrackingScaling"
  
  target_tracking_configuration {
    predefined_metric_specification {
      predefined_metric_type = "ASGAverageCPUUtilization"
    }
    target_value = 70.0
  }
}
```

**modules/database/main.tf**
```hcl
# DB Subnet Group
resource "aws_db_subnet_group" "main" {
  name       = "${var.name_prefix}-db-subnet-group"
  subnet_ids = var.private_db_subnet_ids
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-db-subnet-group"
  })
}

# DB Parameter Group
resource "aws_db_parameter_group" "main" {
  name   = "${var.name_prefix}-db-params"
  family = "postgres14"
  
  parameter {
    name  = "log_connections"
    value = "1"
  }
  
  parameter {
    name  = "log_disconnections"
    value = "1"
  }
  
  tags = var.tags
}

# Random password
resource "random_password" "db_password" {
  length  = 32
  special = true
}

# Secrets Manager
resource "aws_secretsmanager_secret" "db_password" {
  name = "${var.name_prefix}-db-password"
  
  tags = var.tags
}

resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id = aws_secretsmanager_secret.db_password.id
  secret_string = jsonencode({
    username = var.db_username
    password = random_password.db_password.result
    engine   = "postgres"
    host     = aws_db_instance.main.endpoint
    port     = 5432
    dbname   = var.db_name
  })
}

# RDS Instance
resource "aws_db_instance" "main" {
  identifier = "${var.name_prefix}-db"
  
  engine         = "postgres"
  engine_version = "14.7"
  instance_class = var.db_instance_class
  
  allocated_storage     = var.db_allocated_storage
  max_allocated_storage = var.db_max_allocated_storage
  storage_type          = "gp3"
  storage_encrypted     = true
  
  db_name  = var.db_name
  username = var.db_username
  password = random_password.db_password.result
  
  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [var.db_security_group_id]
  parameter_group_name   = aws_db_parameter_group.main.name
  
  multi_az            = var.environment == "prod"
  publicly_accessible = false
  
  backup_retention_period = var.environment == "prod" ? 30 : 7
  backup_window           = "03:00-04:00"
  maintenance_window      = "mon:04:00-mon:05:00"
  
  deletion_protection = var.environment == "prod"
  skip_final_snapshot = var.environment != "prod"
  
  final_snapshot_identifier = var.environment == "prod" ? "${var.name_prefix}-db-final-${formatdate("YYYY-MM-DD", timestamp())}" : null
  
  enabled_cloudwatch_logs_exports = ["postgresql", "upgrade"]
  
  lifecycle {
    prevent_destroy = false  # Set to true in production
  }
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-db"
  })
}

# Read Replica (Production only)
resource "aws_db_instance" "replica" {
  count = var.environment == "prod" ? 1 : 0
  
  identifier = "${var.name_prefix}-db-replica"
  
  replicate_source_db = aws_db_instance.main.identifier
  instance_class      = var.db_instance_class
  
  publicly_accessible    = false
  vpc_security_group_ids = [var.db_security_group_id]
  
  tags = merge(var.tags, {
    Name = "${var.name_prefix}-db-replica"
  })
}
```

**environments/prod/main.tf**
```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/3-tier-app/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
  }
}

provider "aws" {
  region = var.region
  
  default_tags {
    tags = {
      Environment = "prod"
      Project     = "3-tier-app"
      ManagedBy   = "Terraform"
    }
  }
}

locals {
  name_prefix = "prod-3tier"
  tags = {
    Environment = "prod"
    Project     = "3-tier-app"
  }
}

# Networking
module "networking" {
  source = "../../modules/networking"
  
  name_prefix        = local.name_prefix
  vpc_cidr           = var.vpc_cidr
  availability_zones = var.availability_zones
  tags               = local.tags
}

# Security
module "security" {
  source = "../../modules/security"
  
  name_prefix = local.name_prefix
  vpc_id      = module.networking.vpc_id
  tags        = local.tags
}

# Database
module "database" {
  source = "../../modules/database"
  
  name_prefix            = local.name_prefix
  environment            = "prod"
  private_db_subnet_ids  = module.networking.private_db_subnet_ids
  db_security_group_id   = module.security.db_security_group_id
  
  db_name              = var.db_name
  db_username          = var.db_username
  db_instance_class    = var.db_instance_class
  db_allocated_storage = var.db_allocated_storage
  
  tags = local.tags
}

# Compute
module "compute" {
  source = "../../modules/compute"
  
  name_prefix              = local.name_prefix
  vpc_id                   = module.networking.vpc_id
  public_subnet_ids        = module.networking.public_subnet_ids
  private_app_subnet_ids   = module.networking.private_app_subnet_ids
  
  alb_security_group_id = module.security.alb_security_group_id
  web_security_group_id = module.security.web_security_group_id
  app_security_group_id = module.security.app_security_group_id
  
  web_instance_type     = var.web_instance_type
  web_min_size          = var.web_min_size
  web_max_size          = var.web_max_size
  web_desired_capacity  = var.web_desired_capacity
  
  app_instance_type     = var.app_instance_type
  app_min_size          = var.app_min_size
  app_max_size          = var.app_max_size
  app_desired_capacity  = var.app_desired_capacity
  
  db_endpoint = module.database.db_endpoint
  db_name     = var.db_name
  
  tags = local.tags
}

# Outputs
output "alb_dns_name" {
  value = module.compute.alb_dns_name
}

output "db_endpoint" {
  value     = module.database.db_endpoint
  sensitive = true
}
```

---

## Project 2: Kubernetes Cluster with Monitoring

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         EKS Cluster                          │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────┐    │
│  │                    Namespaces                        │    │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────────────────┐  │    │
│  │  │   app   │  │monitoring│  │     ingress-nginx   │  │    │
│  │  └─────────┘  └─────────┘  └─────────────────────┘  │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                   Node Groups                        │    │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │    │
│  │  │   general   │  │   compute   │  │   memory    │  │    │
│  │  │  t3.medium  │  │  c5.xlarge  │  │  r5.large   │  │    │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Implementation

**main.tf**
```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "~> 2.23"
    }
    helm = {
      source  = "hashicorp/helm"
      version = "~> 2.11"
    }
  }
}

provider "aws" {
  region = var.region
}

# EKS Cluster
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "19.16.0"
  
  cluster_name    = var.cluster_name
  cluster_version = "1.28"
  
  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets
  
  cluster_endpoint_public_access = true
  
  eks_managed_node_groups = {
    general = {
      desired_size = 2
      min_size     = 1
      max_size     = 5
      
      instance_types = ["t3.medium"]
      capacity_type  = "ON_DEMAND"
      
      labels = {
        role = "general"
      }
    }
    
    compute = {
      desired_size = 2
      min_size     = 0
      max_size     = 10
      
      instance_types = ["c5.xlarge"]
      capacity_type  = "SPOT"
      
      labels = {
        role = "compute"
      }
      
      taints = [{
        key    = "dedicated"
        value  = "compute"
        effect = "NO_SCHEDULE"
      }]
    }
  }
  
  tags = var.tags
}

# Kubernetes Provider
provider "kubernetes" {
  host                   = module.eks.cluster_endpoint
  cluster_ca_certificate = base64decode(module.eks.cluster_certificate_authority_data)
  
  exec {
    api_version = "client.authentication.k8s.io/v1beta1"
    command     = "aws"
    args        = ["eks", "get-token", "--cluster-name", module.eks.cluster_name]
  }
}

# Helm Provider
provider "helm" {
  kubernetes {
    host                   = module.eks.cluster_endpoint
    cluster_ca_certificate = base64decode(module.eks.cluster_certificate_authority_data)
    
    exec {
      api_version = "client.authentication.k8s.io/v1beta1"
      command     = "aws"
      args        = ["eks", "get-token", "--cluster-name", module.eks.cluster_name]
    }
  }
}

# Namespaces
resource "kubernetes_namespace" "app" {
  metadata {
    name = "app"
    labels = {
      environment = var.environment
    }
  }
}

resource "kubernetes_namespace" "monitoring" {
  metadata {
    name = "monitoring"
  }
}

# Nginx Ingress Controller
resource "helm_release" "nginx_ingress" {
  name       = "nginx-ingress"
  repository = "https://kubernetes.github.io/ingress-nginx"
  chart      = "ingress-nginx"
  version    = "4.8.0"
  namespace  = "ingress-nginx"
  
  create_namespace = true
  
  set {
    name  = "controller.service.type"
    value = "LoadBalancer"
  }
  
  set {
    name  = "controller.metrics.enabled"
    value = "true"
  }
}

# Prometheus Stack
resource "helm_release" "prometheus" {
  name       = "prometheus"
  repository = "https://prometheus-community.github.io/helm-charts"
  chart      = "kube-prometheus-stack"
  version    = "51.0.0"
  namespace  = kubernetes_namespace.monitoring.metadata[0].name
  
  values = [
    yamlencode({
      prometheus = {
        prometheusSpec = {
          retention = "30d"
          storageSpec = {
            volumeClaimTemplate = {
              spec = {
                accessModes = ["ReadWriteOnce"]
                resources = {
                  requests = {
                    storage = "50Gi"
                  }
                }
              }
            }
          }
        }
      }
      grafana = {
        enabled       = true
        adminPassword = var.grafana_password
        ingress = {
          enabled = true
          hosts   = ["grafana.${var.domain}"]
        }
      }
      alertmanager = {
        enabled = true
      }
    })
  ]
}

# Outputs
output "cluster_endpoint" {
  value = module.eks.cluster_endpoint
}

output "cluster_name" {
  value = module.eks.cluster_name
}

output "grafana_url" {
  value = "https://grafana.${var.domain}"
}
```

---

## Project 3: Serverless Application

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      API Gateway                             │
│                    (REST API)                                │
└─────────────────────────┬───────────────────────────────────┘
                          │
          ┌───────────────┼───────────────┐
          │               │               │
          ▼               ▼               ▼
    ┌──────────┐    ┌──────────┐    ┌──────────┐
    │  Lambda  │    │  Lambda  │    │  Lambda  │
    │  (GET)   │    │  (POST)  │    │ (DELETE) │
    └────┬─────┘    └────┬─────┘    └────┬─────┘
         │               │               │
         └───────────────┼───────────────┘
                         │
                         ▼
                   ┌──────────┐
                   │ DynamoDB │
                   └──────────┘
```

### Implementation

**main.tf**
```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    archive = {
      source  = "hashicorp/archive"
      version = "~> 2.4"
    }
  }
}

provider "aws" {
  region = var.region
}

locals {
  function_name = "${var.project}-${var.environment}"
  tags = {
    Project     = var.project
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

# DynamoDB Table
resource "aws_dynamodb_table" "main" {
  name           = "${local.function_name}-table"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "id"
  
  attribute {
    name = "id"
    type = "S"
  }
  
  tags = local.tags
}

# Lambda IAM Role
resource "aws_iam_role" "lambda" {
  name = "${local.function_name}-lambda-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "lambda.amazonaws.com"
      }
    }]
  })
  
  tags = local.tags
}

resource "aws_iam_role_policy" "lambda" {
  name = "${local.function_name}-lambda-policy"
  role = aws_iam_role.lambda.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "logs:CreateLogGroup",
          "logs:CreateLogStream",
          "logs:PutLogEvents"
        ]
        Resource = "arn:aws:logs:*:*:*"
      },
      {
        Effect = "Allow"
        Action = [
          "dynamodb:GetItem",
          "dynamodb:PutItem",
          "dynamodb:DeleteItem",
          "dynamodb:Scan",
          "dynamodb:Query"
        ]
        Resource = aws_dynamodb_table.main.arn
      }
    ]
  })
}

# Lambda Function
data "archive_file" "lambda" {
  type        = "zip"
  source_dir  = "${path.module}/lambda"
  output_path = "${path.module}/lambda.zip"
}

resource "aws_lambda_function" "api" {
  filename         = data.archive_file.lambda.output_path
  function_name    = local.function_name
  role             = aws_iam_role.lambda.arn
  handler          = "index.handler"
  source_code_hash = data.archive_file.lambda.output_base64sha256
  runtime          = "nodejs18.x"
  timeout          = 30
  memory_size      = 256
  
  environment {
    variables = {
      TABLE_NAME  = aws_dynamodb_table.main.name
      ENVIRONMENT = var.environment
    }
  }
  
  tags = local.tags
}

# API Gateway
resource "aws_apigatewayv2_api" "main" {
  name          = "${local.function_name}-api"
  protocol_type = "HTTP"
  
  cors_configuration {
    allow_origins = ["*"]
    allow_methods = ["GET", "POST", "PUT", "DELETE"]
    allow_headers = ["Content-Type", "Authorization"]
  }
  
  tags = local.tags
}

resource "aws_apigatewayv2_stage" "main" {
  api_id      = aws_apigatewayv2_api.main.id
  name        = var.environment
  auto_deploy = true
  
  access_log_settings {
    destination_arn = aws_cloudwatch_log_group.api.arn
    format = jsonencode({
      requestId      = "$context.requestId"
      ip             = "$context.identity.sourceIp"
      requestTime    = "$context.requestTime"
      httpMethod     = "$context.httpMethod"
      routeKey       = "$context.routeKey"
      status         = "$context.status"
      responseLength = "$context.responseLength"
    })
  }
}

resource "aws_apigatewayv2_integration" "lambda" {
  api_id             = aws_apigatewayv2_api.main.id
  integration_type   = "AWS_PROXY"
  integration_uri    = aws_lambda_function.api.invoke_arn
  integration_method = "POST"
}

resource "aws_apigatewayv2_route" "get" {
  api_id    = aws_apigatewayv2_api.main.id
  route_key = "GET /items"
  target    = "integrations/${aws_apigatewayv2_integration.lambda.id}"
}

resource "aws_apigatewayv2_route" "get_one" {
  api_id    = aws_apigatewayv2_api.main.id
  route_key = "GET /items/{id}"
  target    = "integrations/${aws_apigatewayv2_integration.lambda.id}"
}

resource "aws_apigatewayv2_route" "post" {
  api_id    = aws_apigatewayv2_api.main.id
  route_key = "POST /items"
  target    = "integrations/${aws_apigatewayv2_integration.lambda.id}"
}

resource "aws_apigatewayv2_route" "delete" {
  api_id    = aws_apigatewayv2_api.main.id
  route_key = "DELETE /items/{id}"
  target    = "integrations/${aws_apigatewayv2_integration.lambda.id}"
}

# Lambda Permission
resource "aws_lambda_permission" "api" {
  statement_id  = "AllowAPIGatewayInvoke"
  action        = "lambda:InvokeFunction"
  function_name = aws_lambda_function.api.function_name
  principal     = "apigateway.amazonaws.com"
  source_arn    = "${aws_apigatewayv2_api.main.execution_arn}/*/*"
}

# CloudWatch Log Group
resource "aws_cloudwatch_log_group" "api" {
  name              = "/aws/apigateway/${local.function_name}"
  retention_in_days = 7
  
  tags = local.tags
}

resource "aws_cloudwatch_log_group" "lambda" {
  name              = "/aws/lambda/${local.function_name}"
  retention_in_days = 7
  
  tags = local.tags
}

# Outputs
output "api_endpoint" {
  value = aws_apigatewayv2_stage.main.invoke_url
}

output "function_name" {
  value = aws_lambda_function.api.function_name
}

output "dynamodb_table" {
  value = aws_dynamodb_table.main.name
}
```

---

## Summary

This comprehensive Terraform guide covers:

1. **Basics (Phase 1)**: IaC concepts, installation, CLI commands
2. **Variables & Outputs (Phase 2)**: Input variables, outputs, locals
3. **Providers & Resources (Phase 3)**: Provider configuration, meta-arguments
4. **State Management (Phase 4)**: State files, remote backends, locking
5. **Expressions & Functions (Phase 5)**: Built-in functions, dynamic blocks
6. **Modules (Phase 6)**: Module structure, reusability, versioning
7. **Workspaces (Phase 7)**: Environment management
8. **Provisioners (Phase 8)**: local-exec, remote-exec, templates
9. **AWS (Phase 9)**: VPC, EC2, RDS, ALB, Auto Scaling
10. **Kubernetes (Phase 10)**: Deployments, services, Helm
11. **CI/CD (Phase 11)**: GitHub Actions, Jenkins, GitLab CI
12. **Security (Phase 12)**: Best practices, secrets management
13. **Advanced (Phase 13)**: Large scale architecture, blue-green deployments
14. **Interview Prep (Phase 14)**: Common questions, debugging
15. **Capstone Projects (Phase 15)**: Real-world implementations

### Next Steps

1. Practice each phase with hands-on exercises
2. Build the capstone projects in your own AWS account
3. Contribute to open-source Terraform modules
4. Get HashiCorp Terraform Associate certification
5. Explore Terraform Cloud/Enterprise features

### Resources

- [Terraform Documentation](https://www.terraform.io/docs)
- [Terraform Registry](https://registry.terraform.io/)
- [HashiCorp Learn](https://learn.hashicorp.com/terraform)
- [AWS Terraform Modules](https://github.com/terraform-aws-modules)
- [Terraform Best Practices](https://www.terraform-best-practices.com/)

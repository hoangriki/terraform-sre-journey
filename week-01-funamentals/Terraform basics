# Week 1 – Terraform Fundamentals

## Overview
This week focuses on understanding what Terraform is, why it is used, and how the core workflow operates. The goal is to build a solid mental model of Infrastructure as Code (IaC) before moving into more advanced concepts like state management and modules.

---

## What is Terraform?
Terraform is an Infrastructure as Code (IaC) tool that allows infrastructure to be defined, provisioned, and managed using declarative configuration files.

Key characteristics:
- Declarative (define *what* you want, not *how* to do it)
- Provider-based (AWS, Azure, GCP, etc.)
- Manages infrastructure lifecycle through state

Terraform is commonly used in SRE and platform engineering to:
- Automate infrastructure provisioning
- Reduce configuration drift
- Enable repeatable and auditable infrastructure changes

---

## Core Terraform Components

### Providers
Providers are plugins that allow Terraform to interact with external APIs.

Example:
```hcl
provider "aws" {
  region = "us-east-1"
}
```

Example – Launching an EC2 Instance with Resoruces:
```
resource "aws_instance" "web" {
  ami           = "<AMI>"                     # Amazon Machine Image ID
  instance_type = "t3.micro"                  # EC2 instance type

  subnet_id               = "<SUBNET>"        # Subnet where instance will reside
  vpc_security_group_ids  = ["<SECURITY_GROUP>"] # Attach security groups

  tags = {
    "Identity" = "<IDENTITY>"                 # Custom tag to identify the instance
  }
}
```
To install the Terraform AWS provider, and set the provider version in a way that is very similar to how you did for Terraform. To begin you need to let Terraform know to use the provider through a required_providers block in the terraform.tf file as seen below.

Example - INstalling the Terraform AWS provider:
```
terraform {
  required_version = ">= 1.0.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
    random = {
      source  = "hashicorp/random"
      version = "3.1.0"
    }
  }
}
```

Authentication Methods
1. Hardcoded Credentials (Not Recommended)

You can specify the AWS access key and secret key directly in the provider block, but this is strongly discouraged.
```
provider "aws" {
  region     = "us-east-1"
  access_key = "<ACCESS_KEY>"
  secret_key = "<SECRET_KEY>"
}
```

Why this is bad:

Credentials may be accidentally committed to GitHub

Violates security best practices

Not suitable for teams or production environments

This method should only be used for quick testing and never in real projects.

2. Environment Variables (Recommended for Local Development)

AWS credentials can be exported as environment variables in the terminal.

```
export AWS_ACCESS_KEY_ID="<ACCESS_KEY>"
export AWS_SECRET_ACCESS_KEY="<SECRET_KEY>"
export AWS_DEFAULT_REGION="us-east-1"
```

Terraform will automatically detect and use these variables.

3. Shared Credentials File (Best Practice)

Terraform can use AWS’s shared credentials file, typically located at:

~/.aws/credentials

Example provider configuration:
```
provider "aws" {
  region                  = "us-east-2"
  shared_credentials_files = ["/Users/rick/.aws/credentials"]
  profile                 = "rickey-us-east-2"
}
```

Benefits:

Keeps secrets out of Terraform code

Supports multiple AWS accounts and profiles

Commonly used in professional environments

Changing AWS Regions

Changing the region in the provider block points Terraform to a different AWS region.

Important note:

Resources are region-specific

Existing resources in the old region are not automatically migrated

Best practice is to:

Run terraform destroy (if appropriate)

Update the region

Run terraform apply to recreate resources

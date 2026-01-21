Lab – Introduction to the Terraform Data Block
Objective

Learn how Terraform data sources are used to query existing information from providers (such as AWS) and dynamically inject that data into Terraform configurations without hard-coding values.

What Are Terraform Data Sources?

Terraform data blocks allow configurations to:

1. Read information from external APIs (AWS, GCP, etc.)

2. Query existing infrastructure

3. Dynamically adapt to different environments and regions

4. Share data between workspaces or modules

Unlike resource blocks:

1. Data sources do not create infrastructure

2. They only read and return data

3. Returned values can be referenced throughout the configuration

__________________________________________________________________
Data Block Structure

```
data "<DATA_TYPE>" "<LOCAL_NAME>" {
  <argument> = <value>
}
```
Reference format:
```
data.<type>.<name>.<attribute>
```

__________________________________________________________________

Task 1 – Query the Current AWS Region

Added a data source to retrieve the active AWS region dynamically:

```
data "aws_region" "current" {}
```

This removes the need to hard-code region values and allows the same configuration to run across multiple regions.

__________________________________________________________________

Task 2 – Use Data Source Output in a Resource

Updated the VPC resource to include the AWS region as a tag:

```
Region = data.aws_region.current.name
```

Terraform queries AWS during plan and apply and injects the returned value into the resource.

__________________________________________________________________

Task 3 – Availability Zones Data Source

Identified an existing data source used to retrieve availability zones:

```
data "aws_availability_zones" "available" {}
```

This allows Terraform to dynamically obtain AZs for the current region instead of hard-coding them.

__________________________________________________________________

Task 4 – Validate Data Source Usage in Subnets

Confirmed the availability zones data source is used when creating subnets:

```
availability_zone = tolist(data.aws_availability_zones.available.names)[each.value]
```

Benefits:

1. Region-agnostic infrastructure

2. No manual AZ updates

3. Improved portability

__________________________________________________________________

Task 5 – Query the Latest Ubuntu 22.04 AMI

Added a data source to retrieve the most recent Ubuntu 22.04 AMI from Canonical:

```
data "aws_ami" "ubuntu_22_04" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
  }

  owners = ["099720109477"]
}
```

This ensures the EC2 instance always uses a trusted and up-to-date Ubuntu image.

__________________________________________________________________

Task 6 – Update EC2 Instance to Use the Data Source

Modified the EC2 instance to use the AMI returned by the data source:

```
ami = data.aws_ami.ubuntu_22_04.id
```
Terraform detected this as a change requiring resource replacement.

__________________________________________________________________

Validation

Ran:

```
terraform apply
```
Observed:

1. Existing EC2 instance destroyed

2. New EC2 instance created using Ubuntu 22.04

3. No manual AMI updates required

__________________________________________________________________

Key Takeaways

1. Data blocks query information instead of creating resources

2. Data sources improve flexibility and reusability

3. Referencing data avoids hard-coding values

4. Some data-driven changes (such as AMIs) force resource recreation

5. Data sources are essential for production-ready Terraform

__________________________________________________________________

Why This Matters for SRE / IaC

Data sources enable:

1. Multi-region deployments

2. Dynamic infrastructure configuration

3. Safer and more maintainable automation

They are a core Terraform concept used heavily in real SRE and platform engineering environments.

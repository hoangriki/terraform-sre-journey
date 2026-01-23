Lab: Introduction to the Terraform Module Block
Overview

Terraform modules are reusable containers that group related resources together. Modules help reduce code duplication, improve consistency, and make Terraform configurations easier to maintain and scale. Every Terraform configuration has a root module, and any modules it calls are considered child modules.

Modules can be sourced from:

1. Local directories

2. Remote Git repositories

3. The Terraform Registry

Why Modules Matter

1. Encourage reusable infrastructure patterns

2. Reduce duplicated code

3. Enable standardized deployments

4. Improve readability and maintainability

5. Commonly used in real-world SRE and platform engineering environments

______________________________________________________

Module Block Structure
```
module "<MODULE_NAME>" {
  source = <MODULE_SOURCE>
  <INPUT_NAME> = <VALUE>
}
```
Key components:

1. Module name – Unique identifier within the configuration

2. Source – Location of the module (local or remote)

3. Version – (Optional) Locks the module version

4. Inputs – Values passed into the child module

Task 1: Create a Module Block Using a Remote Module

A new module block was added to main.tf to call a remote module from the Terraform Registry.

```
module "subnet_addrs" {
  source  = "hashicorp/subnets/cidr"
  version = "1.0.0"

  base_cidr_block = "10.0.0.0/22"
  networks = [
    {
      name     = "module_network_a"
      new_bits = 2
    },
    {
      name     = "module_network_b"
      new_bits = 2
    }
  ]
}
```
This module:

1. Accepts a base CIDR block

2. Calculates subnet CIDR ranges

3. Returns subnet values as outputs

4. Does not create any AWS resources

______________________________________________________

Task 1.1: Initialize Terraform Modules

Command executed: terraform init

Result:

1. Terraform downloaded the module from the registry

2. Module stored locally in .terraform/modules/

3. Terraform environment initialized successfully

______________________________________________________

Task 1.2: Apply the Configuration

An output was added to display values returned by the module.
```
output "subnet_addrs" {
  value = module.subnet_addrs.network_cidr_blocks
}
```
Command executed: terraform apply -auto-approve

Result:
```
Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:
subnet_addrs = {
  "module_network_a" = "10.0.0.0/24"
  "module_network_b" = "10.0.1.0/24"
}
```
______________________________________________________

Key Observations

1. Modules can return computed values without creating infrastructure

2. Outputs from modules are accessed using module.<name>.<output>

3. terraform init must be rerun when adding or changing modules

4. Version pinning ensures predictable behavior

Key Takeaways

1. Modules are the foundation of scalable Terraform design

2. Remote modules accelerate development by reusing trusted code

3. Outputs allow modules to behave like reusable functions

4. This pattern is commonly used for networking, IAM, and shared infrastructure

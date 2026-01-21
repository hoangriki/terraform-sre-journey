## Lab – Introduction to the Terraform Locals Block

### Objective
Understand how **Terraform locals** are used to simplify configuration files by defining reusable values and expressions that reduce repetition and improve readability.

---

## What Are Terraform Locals?

Terraform **locals** are named values defined within a `locals` block. They are similar to input variables but differ in important ways:

- Locals are **not provided by users or environment variables**
- Their values are **computed once** and do not change during `init`, `plan`, or `apply`
- They are commonly used to:
  - Reduce duplicated expressions
  - Create consistent naming conventions
  - Improve readability of resource definitions

Locals are referenced using:
```hcl
local.<name>
```
(Note: local, not locals)
____________________________________________________
Locals Block Syntax
```
locals {
  local_variable_name = <EXPRESSION OR VALUE>
}
```

Example:
```
locals {
  time        = timestamp()
  application = "api_server"
  server_name = "${var.account}-${local.application}"
}
```
____________________________________________________
Task 1 – Define EC2 Naming Using Locals

Added a locals block to define consistent tagging and naming for an EC2 instance:
```
locals {
  team        = "api_mgmt_dev"
  application = "corp_api"
  server_name = "ec2-${var.environment}-api-${var.variables_sub_az}"
}
```
This approach centralizes naming logic instead of repeating string values throughout the configuration.

____________________________________________________

Task 1.1 – Apply Locals to an EC2 Resource

Updated the EC2 resource to reference the local variables:

```
resource "aws_instance" "web_server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  subnet_id     = aws_subnet.public_subnets["public_subnet_1"].id

  tags = {
    Name  = local.server_name
    Owner = local.team
    App   = local.application
  }
}
```
Key observations:

1. Locals cleanly separate naming logic from resource definitions

2 Tag values are now consistent and easier to update

3. Terraform automatically detects only a tag update, not a resource replacement

____________________________________________________

Task 1.2 – Validate with Terraform Plan & Apply

Ran:

```
terraform plan
```

Result:

1. Terraform showed in-place updates only

2. The EC2 instance was not destroyed

3. Only tag values were modified

____________________________________________________

Applied changes with:

```
terraform apply
```

Validated in AWS Console that:

1. EC2 instance name was updated

2. Tags reflected values defined in the locals block

____________________________________________________

Cleanup (Optional)

After validation, locals and tags can be removed to revert the configuration:

1. Remove locals block

2. Remove tag references

3. Re-run terraform apply to restore original values

____________________________________________________

Key Takeaways

1.Locals help reduce repetition and improve maintainability

2. They are ideal for naming conventions and shared values

3. Locals are static during a Terraform run

4. Using locals avoids hard-coding values in multiple resources

5. Terraform correctly performs non-destructive updates when only tags change

____________________________________________________

Why Locals Matter for SRE / IaC

In real-world environments:

1. Naming standards matter

2. Tags drive cost allocation, ownership, and automation

3. Locals make infrastructure code easier to scale and maintain

This pattern becomes especially powerful when combined with:

1. Input variables

2. Modules

3. Multi-environment deployments

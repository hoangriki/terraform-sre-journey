Lab: Terraform Configuration Block
Overview

Terraform uses a configuration block (terraform {}) to define global settings that control how Terraform behaves. One of the most important uses 
of this block is enforcing Terraform CLI version constraints, ensuring consistency across teams and environments.

__________________________________________________________

Purpose of the Terraform Configuration Block
1. Declare required Terraform CLI versions
2. Prevent accidental execution with unsupported Terraform versions
3. Improve reproducibility and stability of infrastructure code
4. Enforce standards in team and CI/CD environments

___________________________________________________________

Task 1: Check Terraform Version

run:

terraform -version, the output will be the terraform version you are running

___________________________________________________________

Task 2: Require a Specific Terraform Version

A new file named terraform.tf was created to define Terraform settings.

Initial Configuration

```
terraform {
  required_version = ">= 1.0.0"
}
```

Command executed:

```
terraform init
```

___________________________________________________________

Result:

1. Terraform initialized successfully

2. Version 1.0.8 satisfies the version constraint

___________________________________________________________

Testing a Strict Version Constraint

The version constraint was changed to require exactly version 1.0.0.

```
terraform {
  required_version = "= 1.0.0"
}
```
Command executed:
```
terraform init
```
Resulting Error
```
Error: Unsupported Terraform Core version

This configuration does not support Terraform version 1.0.8.
```
Explanation

1. The = operator enforces an exact version match

2. Terraform 1.0.8 does not satisfy = 1.0.0

3. Terraform correctly blocks execution to prevent incompatibility

___________________________________________________________

Resolution

The version constraint was reverted back to a compatible range:
```
terraform {
  required_version = ">= 1.0.0"
}
```
Command executed:
```
terraform init
```
Terraform initialized successfully again.
___________________________________________________________
Key Takeaways

1. The terraform {} block controls global Terraform behavior

2. required_version protects infrastructure from:

3. Breaking changes

4. Inconsistent Terraform versions

5. Use version ranges (>=) for flexibility

6. Use strict versions (=) only when absolutely required
___________________________________________________________
Best Practices

1. Always define required_version in production Terraform code

2. Store Terraform settings in terraform.tf

3. Align version constraints with CI/CD pipelines

4. Avoid overly strict version pinning unless necessary

## Lab – Terraform Resource Blocks

### Objective
Learn how Terraform uses **resource blocks** to define, create, and manage infrastructure across providers, and how resources interact with each other through references and dependencies.

---

## Resource Block Structure

Terraform resource blocks follow this general format:

```hcl
resource "<PROVIDER_RESOURCE_TYPE>" "<LOCAL_NAME>" {
  <argument> = <value>
}
Key points:

resource declares infrastructure Terraform will manage

The resource type maps directly to a provider API (e.g., AWS)

The local name is used for referencing the resource elsewhere

Arguments can be required or optional depending on the resource

Example mappings from the AWS provider:

Terraform Resource	AWS Service
aws_instance	EC2
aws_security_group	Security Group
aws_s3_bucket	S3 Bucket
aws_key_pair	EC2 Key Pair

Without resource blocks, Terraform will not create infrastructure. Other blocks (provider, variable, output, etc.) exist to support resources.

Task 1 – Understanding an Existing Resource
Reviewed an aws_route_table resource and learned:

Resource IDs are a combination of resource type and name
(aws_route_table.public_route_table)

Resources can reference other resources using interpolation

Required arguments (like vpc_id) enforce correct infrastructure relationships

This demonstrated how Terraform builds a dependency graph automatically.

Task 2 – Creating an S3 Bucket
Created a new Amazon S3 bucket using an aws_s3_bucket resource.

Key takeaways:

terraform plan shows exactly what will be created

S3 bucket names must be globally unique

Tags are first-class citizens and should always be included

Additional supporting resources (like ownership controls) may be required

Validated the resource by logging into the AWS console after apply.

Task 3 – Creating a Security Group
Created an aws_security_group resource to allow inbound HTTPS (443).

Learned that:

Different resource types require different arguments

Security groups define ingress/egress rules explicitly

Terraform plans clearly show networking rules before creation

Confirmed creation via the EC2/VPC console.

Task 4 – Using a Non-AWS Provider (random)
Introduced the random provider by creating a random_id resource.

Important lessons:

Terraform only downloads providers defined in the configuration

Adding a new provider requires re-running terraform init

.terraform.lock.hcl tracks provider versions and should be committed

Not all Terraform resources create cloud infrastructure

This reinforced Terraform’s provider-based architecture.

Task 5 – Using Resource References & Forced Replacement
Updated the S3 bucket name to include a randomly generated ID:

hcl
Copy code
bucket = "my-new-tf-test-bucket-${random_id.randomness.hex}"
Key takeaways:

Terraform allows dynamic values via interpolation

Some changes (like S3 bucket names) force resource replacement

terraform plan clearly indicates destructive changes (-/+)

This demonstrated how Terraform manages lifecycle changes safely.

Cleanup
All resources created in this lab were destroyed after completion:

S3 bucket

Security group

Random ID

Key Takeaways
Resource blocks are the core of Terraform

Terraform automatically manages dependencies between resources

Providers must be initialized before use

terraform plan is critical for understanding impact

Some changes are destructive and must be reviewed carefully

Terraform is not AWS-only — providers extend functionality

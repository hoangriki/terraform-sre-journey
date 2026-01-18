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


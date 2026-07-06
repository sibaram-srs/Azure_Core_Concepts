# Terraform on Azure - Training Session
### Beginner Friendly Session Notes

---

# Session Agenda

1. What is DevOps?
2. Why DevOps?
3. What is Microsoft Azure?
4. Why Azure?
5. Azure Resources Overview
6. Introduction to Terraform
7. Why Infrastructure as Code (IaC)?
8. Terraform Architecture
9. Terraform Basics
10. Terraform Workflow
11. Creating Azure Resources using Terraform
12. Best Practices
13. Demo
14. Q&A

---

# 1. What is DevOps?

## Definition

**DevOps** is a combination of **Development (Dev)** and **Operations (Ops)**.

It is a culture, set of practices, and tools that help developers and operations teams work together to deliver applications faster, more reliably, and with better quality.

Instead of working separately, both teams collaborate throughout the software lifecycle.

---

## Traditional Approach

```
Developer
      ↓
Develop Application
      ↓
Send to Operations
      ↓
Operations Deploy
      ↓
Issues Found
      ↓
Back to Developer
```

Problems:
- Slow deployment
- Manual work
- Communication gap
- Frequent deployment failures

---

## DevOps Approach

```
Plan
   ↓
Develop
   ↓
Build
   ↓
Test
   ↓
Deploy
   ↓
Monitor
   ↓
Feedback
```

Everything is automated as much as possible.

---

# Why DevOps?

Without DevOps:

- Manual deployments
- Human errors
- Slow delivery
- Difficult rollback
- Environment mismatch
- Lack of collaboration

With DevOps:

✅ Faster Delivery

✅ Automation

✅ Better Collaboration

✅ Continuous Integration (CI)

✅ Continuous Delivery (CD)

✅ Reliable Deployments

✅ Easy Rollback

---

# DevOps Lifecycle

```
Planning
    ↓
Coding
    ↓
Building
    ↓
Testing
    ↓
Release
    ↓
Deployment
    ↓
Monitoring
    ↓
Feedback
```

---

# Popular DevOps Tools

| Stage | Tool |
|---------|------|
| Source Code | Git |
| Repository | GitHub / Azure Repos |
| Build | Maven / Gradle |
| CI/CD | Azure DevOps / Jenkins |
| Container | Docker |
| Orchestration | Kubernetes |
| Cloud | Azure / AWS / GCP |
| IaC | Terraform |
| Monitoring | Azure Monitor / Prometheus |

---

# 2. What is Microsoft Azure?

Azure is Microsoft's Cloud Computing Platform.

It provides hundreds of cloud services for:

- Computing
- Networking
- Storage
- Security
- Databases
- AI
- DevOps
- Analytics

Instead of buying physical servers, we rent resources from Azure.

---

# Why Azure?

Advantages:

- Pay as you go
- High Availability
- Global Datacenters
- Auto Scaling
- Disaster Recovery
- Strong Security
- Easy Resource Management

---

# Azure Global Infrastructure

```
Azure

   Regions
       ↓
Availability Zones
       ↓
Resource Groups
       ↓
Resources
```

---

# Azure Resource Hierarchy

```
Management Group

      ↓

Subscription

      ↓

Resource Group

      ↓

Resources
```

---

# What is Resource Group?

A Resource Group is a logical container.

It contains Azure resources like:

- Virtual Machine
- Storage Account
- Virtual Network
- SQL Database
- App Service

Example:

```
Resource Group

RG-Training

    ├── VM
    ├── Storage
    ├── VNet
    ├── SQL
```

---

# Common Azure Resources

## Compute

- Virtual Machine
- VM Scale Set
- App Service
- AKS

---

## Networking

- Virtual Network
- Subnet
- NSG
- Public IP
- Load Balancer

---

## Storage

- Storage Account
- Blob Storage
- File Share

---

## Database

- Azure SQL
- Cosmos DB
- MySQL
- PostgreSQL

---

# Azure Portal

Azure resources can be created using:

- Azure Portal (GUI)
- Azure CLI
- PowerShell
- ARM Template
- Bicep
- Terraform

Question:

If we have to create 500 VMs...

Will we create them manually?

Obviously No.

This is where Terraform comes into the picture.

---

# 3. What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool developed by **HashiCorp**.

It allows us to create, update, and manage infrastructure using code instead of manually clicking in the Azure Portal.

---

# What is Infrastructure as Code (IaC)?

Infrastructure is written as code.

Instead of manually creating:

- VM
- Storage
- Network

We write a configuration file.

Example:

```
resource "azurerm_resource_group" "rg" {
  name     = "rg-demo"
  location = "Central India"
}
```

Run one command.

Terraform creates everything automatically.

---

# Why Terraform?

Without Terraform

```
Portal

Click
Click
Click
Click
Click
```

Problems

- Time consuming
- Human error
- Difficult to repeat
- No version control

---

With Terraform

```
Code

↓

terraform apply

↓

Infrastructure Ready
```

Benefits

- Automation
- Version Control
- Repeatability
- Reusability
- Consistency
- Easy Rollback

---

# Terraform Architecture

```
Terraform Code

        ↓

Terraform CLI

        ↓

Terraform Provider

        ↓

Azure APIs

        ↓

Azure Resources
```

---

# Terraform Core Components

## Provider

Provider connects Terraform to Cloud.

Examples:

- Azure
- AWS
- GCP
- GitHub
- Kubernetes

---

## Resource

Actual infrastructure.

Example

- VM
- Storage
- Resource Group

---

## Variable

Stores values.

Example

```
variable "location" {}
```

---

## Output

Displays information.

Example

VM IP Address

Storage Name

Resource Group Name

---

## State File

Terraform stores infrastructure information in

```
terraform.tfstate
```

This file tracks what Terraform has created.

---

# Terraform Workflow

```
Write Code

      ↓

terraform init

      ↓

terraform plan

      ↓

terraform apply

      ↓

Infrastructure Created

      ↓

terraform destroy

(Delete Infrastructure)
```

---

# Important Terraform Commands

## Initialize

```
terraform init
```

Downloads provider plugins.

---

## Validate

```
terraform validate
```

Checks syntax.

---

## Format

```
terraform fmt
```

Formats Terraform code.

---

## Plan

```
terraform plan
```

Shows what Terraform will create.

Nothing changes yet.

---

## Apply

```
terraform apply
```

Creates resources.

---

## Destroy

```
terraform destroy
```

Deletes infrastructure.

---

# Terraform File Structure

```
terraform-project/

│

├── main.tf

├── provider.tf

├── variables.tf

├── outputs.tf

├── terraform.tfvars

└── terraform.tfstate
```

---

# Azure Authentication

Terraform can authenticate using:

- Azure CLI
- Service Principal
- Managed Identity

For learning/demo, Azure CLI is easiest.

```
az login
```

---

# Demo: Create Azure Resource Group

provider.tf

```hcl
provider "azurerm" {
  features {}
}
```

---

main.tf

```hcl
resource "azurerm_resource_group" "demo" {

  name = "rg-demo"

  location = "Central India"

}
```

Commands

```bash
terraform init

terraform plan

terraform apply
```

Check Azure Portal.

Resource Group created.

---

# Demo 2: Create Storage Account

```hcl
resource "azurerm_storage_account" "storage" {

  name = "storagedemo12345"

  resource_group_name = azurerm_resource_group.demo.name

  location = azurerm_resource_group.demo.location

  account_tier = "Standard"

  account_replication_type = "LRS"

}
```

Apply again.

Terraform creates Storage Account.

---

# Understanding Terraform Dependency

Terraform automatically understands dependencies.

Example

```
Storage Account

↓

Needs Resource Group

↓

Terraform creates Resource Group first
```

No manual ordering required.

---

# Terraform State

Terraform compares

Desired State

vs

Current State

Then makes only the required changes.

This is why Terraform is called **Declarative**.

---

# Best Practices

- Keep code modular.
- Use variables.
- Never hardcode secrets.
- Store state remotely.
- Use Git for version control.
- Review `terraform plan` before `apply`.
- Use meaningful resource names.

---

# Real World Example

Suppose a company needs:

- 20 Resource Groups
- 50 Virtual Machines
- 30 Storage Accounts
- 15 VNets

Manual Creation

❌ Hours

Terraform

✅ Few minutes

---

# Advantages of Terraform

- Open Source
- Multi-Cloud Support
- Automation
- Reusable Code
- Version Controlled
- Easy Scaling
- Consistent Infrastructure
- Faster Deployments

---

# Terraform vs Azure Portal

| Azure Portal | Terraform |
|---------------|-----------|
| Manual | Automated |
| Slow | Fast |
| Human Errors | Consistent |
| No Version Control | Git Integration |
| Difficult to Repeat | Easily Repeatable |

---

# Key Takeaways

- DevOps improves collaboration and automation.
- Azure is Microsoft's cloud platform.
- Azure resources are organized using Resource Groups.
- Terraform is an Infrastructure as Code (IaC) tool.
- Terraform automates cloud infrastructure creation.
- Core workflow: **init → plan → apply → destroy**.
- Infrastructure is managed through code, making deployments repeatable and reliable.

---

# Demo Flow

1. Login to Azure

```bash
az login
```

2. Create Terraform files

3. Run

```bash
terraform init
```

4. Check execution plan

```bash
terraform plan
```

5. Create resources

```bash
terraform apply
```

6. Verify in Azure Portal

7. Clean up

```bash
terraform destroy
```

---

# Thank You!

## Questions?

**Happy Learning! 🚀**

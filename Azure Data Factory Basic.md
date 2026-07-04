# Azure Data Factory (ADF) for an Azure DevOps & Terraform Engineer

> **Audience:** Someone who has worked with **Azure Cloud, Azure DevOps, GitHub, and Terraform**, but is new to Azure Data Factory (ADF).

---

# 1. First Understand What ADF Actually Is

Since you've worked on infrastructure using Terraform and Azure DevOps, think of Azure Data Factory (ADF) like this:

| What you know | ADF Equivalent |
|---------------|----------------|
| Terraform provisions infrastructure | ADF creates and orchestrates data workflows |
| Azure DevOps runs deployment pipelines | ADF runs data pipelines |
| GitHub stores infrastructure code | Git stores ADF pipeline definitions |
| Azure VM runs applications | Integration Runtime runs data movement |
| Azure DevOps triggers deployment | ADF Trigger starts data pipelines |

**Simple Definition**

> Azure Data Factory is Microsoft's cloud-native service used to **move, transform, and orchestrate data** between different systems.

Instead of deploying infrastructure...

ADF deploys **data**.

---

# 2. Real World Example

Imagine your company has:

```
Production SQL Server
        │
        │
Customer Data
        │
        ▼
Azure Data Factory
        │
        ▼
Azure Data Lake
        │
        ▼
Azure Synapse
        │
        ▼
Power BI Dashboard
```

Every night at 1 AM

ADF automatically

- Reads new customer records
- Copies them to Data Lake
- Cleans the data
- Loads it into Synapse
- Refreshes reports

Nobody manually runs scripts.

---

# 3. Why Not Use Bash or Python Scripts?

You might ask:

> "I can already write Python scripts."

Absolutely.

But imagine handling:

- 500 databases
- 20 APIs
- 15 Storage Accounts
- 10 countries
- Automatic retries
- Scheduling
- Monitoring
- Logging
- Alerts

Managing all this manually becomes difficult.

ADF provides these capabilities out of the box.

---

# 4. Think Like Azure DevOps

Azure DevOps Pipeline

```
Git Push

↓

Build

↓

Test

↓

Deploy
```

ADF Pipeline

```
Read Data

↓

Copy Data

↓

Transform

↓

Store

↓

Notify
```

Both are orchestration engines.

One orchestrates deployments.

The other orchestrates data movement.

---

# 5. Main Components

## Pipeline

Exactly like Azure DevOps Pipeline.

Example

```
Pipeline

├── Read SQL
├── Copy Data
├── Clean Data
├── Store Data
└── Send Email
```

Pipeline = Workflow

---

## Activity

Activities are individual tasks.

Like Azure DevOps Tasks

Azure DevOps

```
Checkout

Terraform Init

Terraform Plan

Terraform Apply
```

ADF

```
Copy Activity

Lookup Activity

Data Flow

Stored Procedure

Web Activity
```

Each activity performs one job.

---

## Dataset

Dataset represents the data.

Example

```
Customer.csv

Sales.xlsx

Employee Table

Orders Table
```

Dataset only describes

- File
- Table
- Schema

No connection details.

---

## Linked Service

This is similar to

Terraform Provider

or

Database Connection String.

Example

```
Server

Username

Password

Authentication
```

Without Linked Service

ADF doesn't know how to connect.

---

## Integration Runtime

This is the compute engine.

Think of it like

```
Azure VM

Azure Container

GitHub Runner

Azure DevOps Agent
```

Its job is to execute activities.

Without Integration Runtime

Nothing runs.

---

# 6. Types of Integration Runtime

## Azure Integration Runtime

Used when everything is inside Azure.

Example

```
Azure SQL

↓

Azure Data Factory

↓

Azure Blob Storage
```

Microsoft manages everything.

No servers.

---

## Self Hosted Integration Runtime

Imagine

Company has

```
On-Prem SQL Server
```

Azure cannot access it.

So company installs

```
Self Hosted IR
```

on their local Windows Server.

Now

```
On Prem SQL

↓

Self Hosted IR

↓

ADF

↓

Azure
```

Think of it like

Azure DevOps Self Hosted Agent.

Very similar concept.

---

## Azure SSIS Runtime

Used only when migrating old SSIS packages.

---

# 7. Copy Activity

Most used activity.

Its only job

Move data.

Example

```
SQL Database

↓

ADF Copy Activity

↓

Blob Storage
```

Supported Sources

- SQL Server
- Oracle
- MySQL
- SAP
- REST API
- FTP
- Amazon S3
- Azure Blob
- Azure Data Lake

---

# 8. Data Flow

Copy Activity only copies.

Data Flow transforms.

Example

Customer Table

```
Name

Age

Salary
```

Need to

- Remove duplicates
- Remove nulls
- Convert lowercase
- Join another table
- Aggregate

Use Data Flow.

Internally

ADF creates Spark clusters.

No need to manage Spark yourself.

---

# 9. Control Flow

Exactly like conditions in Azure DevOps.

Example

```
If File Exists

↓

Yes

↓

Copy

↓

Else

↓

Send Email
```

Activities

- If
- Switch
- Until
- ForEach
- Wait

---

# 10. Parameters

Same idea as Terraform Variables.

Terraform

```hcl
variable "location" {}
```

ADF

```
FileName

DatabaseName

TableName

Date
```

Instead of hardcoding values.

---

# 11. Variables

Temporary values during execution.

Example

```
Counter = 10

Status = Success

TotalRows = 5000
```

---

# 12. Triggers

Exactly like Azure DevOps Schedule.

Schedule Trigger

```
Run Every Day

1 AM
```

Event Trigger

```
Blob Uploaded

↓

Run Pipeline
```

Tumbling Window

```
Run Every Hour
```

---

# 13. Dynamic Expressions

Like Terraform Functions.

Terraform

```
format()

join()

replace()
```

ADF

```
@utcnow()

@concat()

@if()

@substring()

@replace()
```

Used to build dynamic pipelines.

---

# 14. Security

Never store passwords.

Use

Azure Key Vault

Exactly like Terraform fetching secrets.

Example

```
ADF

↓

Managed Identity

↓

Key Vault

↓

SQL Password

↓

Connect SQL
```

---

# 15. Incremental Load

Interview Favorite.

Instead of copying

```
10 Million Rows
```

Copy only

```
100 New Rows
```

Example

Yesterday

```
100000 Records
```

Today

```
100120 Records
```

Copy only

```
120 Records
```

Query

```sql
SELECT *
FROM Customers
WHERE ModifiedDate > LastLoadDate
```

Much faster.

---

# 16. Monitoring

Exactly like Azure DevOps Pipeline Logs.

You can see

- Failed Pipeline
- Success
- Execution Time
- Retry Count
- Activity Logs

---

# 17. Git Integration

ADF supports

- Azure DevOps Repos
- GitHub

Pipeline definitions are stored as JSON.

Developer Workflow

```
GitHub

↓

Feature Branch

↓

ADF Development

↓

Pull Request

↓

Merge

↓

Publish
```

Very similar to Infrastructure as Code workflows.

---

# 18. CI/CD for ADF (This is where your Azure DevOps knowledge helps)

## Development

Developer creates pipelines.

↓

Commits code.

↓

Creates Pull Request.

↓

Merge.

↓

Publish.

ADF generates ARM Templates automatically.

---

## Azure DevOps Pipeline

Azure DevOps

```
GitHub

↓

Build

↓

ARM Template

↓

Deploy

↓

ADF Dev
```

Next

```
ADF Dev

↓

ADF Test

↓

ADF Production
```

Terraform is generally used to provision the Azure resources (ADF instance, Storage Accounts, Key Vault, etc.), while Azure DevOps deploys the ADF artifacts (pipelines, datasets, linked services).

---

# 19. Terraform + ADF

Terraform creates infrastructure.

Example

```text
Terraform

↓

Resource Group

↓

Storage Account

↓

Key Vault

↓

Azure Data Factory

↓

Managed Identity
```

Terraform does **not** create your data pipelines (except basic resources). Those are usually developed in ADF and deployed using ARM/Bicep or CI/CD pipelines.

---

# 20. Real Project Example

Imagine you're working for an e-commerce company.

Every night

Orders are stored in

```
Azure SQL Database
```

Business wants

```
Power BI Dashboard
```

Updated every morning.

Pipeline

```
Schedule Trigger

↓

Copy Orders

↓

Store in Data Lake

↓

Data Flow

↓

Clean Data

↓

Load Synapse

↓

Stored Procedure

↓

Refresh Dashboard

↓

Send Success Email
```

Everything runs automatically.

---

# 21. Benefits of Azure Data Factory

- No need to write custom ETL scripts for every workflow.
- Supports 100+ data sources (SQL, Blob, S3, REST APIs, SAP, Oracle, etc.).
- Easy scheduling and event-based execution.
- Built-in monitoring, retry, and logging.
- Secure authentication using Managed Identity and Azure Key Vault.
- Scales automatically with Azure-managed compute.
- Integrates with Azure DevOps and GitHub for version control and CI/CD.
- Low-code interface for building complex data pipelines.

---

# 22. Common Interview Questions

### What is Azure Data Factory?

A cloud-based data integration and orchestration service that moves and transforms data between different sources and destinations.

---

### What is the difference between Pipeline and Activity?

- **Pipeline:** A workflow containing multiple activities.
- **Activity:** A single task within the pipeline, such as copying data or executing a stored procedure.

---

### What is a Linked Service?

A connection definition that stores the information required to connect to a data source or destination.

---

### What is a Dataset?

A dataset represents the structure or location of the data (table, file, etc.) that an activity uses.

---

### What is Integration Runtime?

The compute infrastructure responsible for executing activities and moving data.

---

### Difference between Azure IR and Self-hosted IR?

| Azure IR | Self-hosted IR |
|-----------|----------------|
| Azure-managed | Customer-managed |
| Cloud resources | On-premises/private network resources |
| No infrastructure to manage | Installed on a Windows server or VM |

---

### What is Copy Activity?

Copies data from a source to a destination without modifying it.

---

### What is Mapping Data Flow?

A visual data transformation feature that uses Azure-managed Spark clusters to transform data at scale.

---

### How do you secure credentials?

Use **Managed Identity** with **Azure Key Vault** instead of storing usernames and passwords directly.

---

### How is CI/CD implemented in ADF?

1. Connect ADF to GitHub or Azure DevOps Repos.
2. Develop changes in feature branches.
3. Raise a Pull Request and merge.
4. Publish to generate ARM templates.
5. Use Azure DevOps pipelines to deploy to Test and Production environments.

---

# 23. If the Interviewer Asks:
**"You have Terraform experience. How is ADF different?"**

A good answer:

> "Terraform is an Infrastructure as Code tool that provisions Azure resources such as Resource Groups, Storage Accounts, Key Vaults, and Azure Data Factory itself. Azure Data Factory is a managed service used to orchestrate and automate data movement and transformation. In a typical enterprise setup, Terraform provisions the infrastructure, while Azure DevOps handles CI/CD for ADF artifacts, and ADF executes the business data pipelines."

---

# 24. One-Line Analogy (Easy to Remember)

- **Terraform** → Builds the **house** (Azure infrastructure).
- **Azure DevOps** → Delivers new **features** to the house (application and ADF deployments).
- **Azure Data Factory** → Moves and processes the **goods** inside the house (data).
- **Azure Key Vault** → Keeps the **keys** secure.
- **GitHub** → Stores the **source code**.
- **Integration Runtime** → The **worker/agent** that performs the data movement.

This analogy helps explain the relationship between these tools in a modern Azure environment.

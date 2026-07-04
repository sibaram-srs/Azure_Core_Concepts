# Azure Data Factory (ADF) Interview Notes for DevOps & Data Engineer Roles

# Azure Data Factory (ADF) Complete Interview Guide

---

# 1. What is Azure Data Factory?

**Azure Data Factory (ADF)** is Microsoft's cloud-based **ETL (Extract, Transform, Load)** and **ELT (Extract, Load, Transform)** service used for creating, scheduling, and orchestrating data pipelines.

It helps move and transform data between different data sources.

Think of ADF as:

```
Source Systems --> Copy Data --> Transform --> Store --> Analytics
```

Example:

```
SQL Server
      |
      |
Azure Data Factory
      |
      |
Azure Data Lake
      |
      |
Azure Synapse / Power BI
```

---

# 2. Why use Azure Data Factory?

- Data Integration
- ETL / ELT Pipelines
- Workflow Orchestration
- Data Migration
- Data Transformation
- Scheduling Jobs
- Monitoring Pipelines

---

# 3. Core Components

## 3.1 Pipeline

A pipeline is a logical grouping of activities.

Example:

```
Pipeline
├── Copy Activity
├── Data Flow
├── Stored Procedure
└── Web Activity
```

Interview Question:

**Q:** What is a Pipeline?

**Answer:**
A Pipeline is a collection of activities that perform a specific workflow.

---

## 3.2 Activity

Activities are individual tasks inside a pipeline.

Examples:

- Copy Activity
- Lookup Activity
- Data Flow
- Execute Pipeline
- Stored Procedure
- Web Activity
- Azure Function
- Databricks Notebook

---

## 3.3 Dataset

Dataset represents the structure of data.

Example:

```
CSV File
SQL Table
Parquet File
JSON
Excel
```

Dataset only describes the data.

---

## 3.4 Linked Service

Linked Service is the connection information.

Similar to a Database Connection String.

Example:

```
Server Name
Username
Password
Authentication
```

Example Linked Services

- Azure SQL Database
- Blob Storage
- Azure Data Lake
- Amazon S3
- Oracle
- Snowflake
- SAP
- REST API

---

## 3.5 Integration Runtime (IR)

This is one of the most important interview topics.

Integration Runtime performs the actual data movement.

Types:

### Azure Integration Runtime

Used for:

- Cloud to Cloud
- Azure SQL
- Blob Storage
- Azure Data Lake

---

### Self Hosted Integration Runtime

Used for:

- On-Prem SQL Server
- Oracle
- Local File System
- Private Networks

Runs on your own server.

---

### Azure SSIS Integration Runtime

Used to run existing SSIS Packages.

---

Interview Question

**Q:** What is Integration Runtime?

Answer:

Integration Runtime is the compute infrastructure responsible for data movement and activity execution.

---

# 4. Copy Activity

Most commonly used activity.

Used for copying data.

Example:

```
SQL Server
      |
Copy Activity
      |
Blob Storage
```

Supports:

- SQL
- Oracle
- MySQL
- PostgreSQL
- Blob Storage
- S3
- REST API
- FTP
- SAP

---

# 5. Data Flow

Used for transformations without writing code.

Supports:

- Filter
- Aggregate
- Join
- Derived Column
- Pivot
- Unpivot
- Lookup
- Window
- Conditional Split

Example:

```
Input
   |
Filter
   |
Join
   |
Aggregate
   |
Output
```

Runs on Azure Spark Cluster.

---

# 6. Control Flow Activities

These control execution.

Examples:

- If Condition
- Switch
- Until
- ForEach
- Wait
- Fail
- Validation

Example:

```
If File Exists
        |
     Yes --> Copy
      No --> Send Alert
```

---

# 7. Parameters

Pipelines can be parameterized.

Example:

```
FileName

TableName

Date
```

Instead of hardcoding values.

Interview Question:

Why use Parameters?

Answer:

To make pipelines reusable.

---

# 8. Variables

Variables store temporary values.

Types:

- String
- Boolean
- Array

Example:

```
Counter = 5
```

---

# 9. Expressions

ADF uses Dynamic Expressions.

Examples:

```
@utcnow()

@pipeline().parameters.filename

@concat()

@substring()

@equals()

@if()

@replace()
```

Common Interview Question.

---

# 10. Triggers

Triggers start pipeline execution.

Types:

## Schedule Trigger

Runs daily.

Example:

```
Every day at 10 PM
```

---

## Tumbling Window Trigger

Runs for fixed time intervals.

Example:

```
Every Hour
```

Maintains dependency.

---

## Event Trigger

Triggered by events.

Example:

```
New File Uploaded
```

Blob Storage Event.

---

# 11. Monitoring

ADF provides monitoring.

Can monitor:

- Success
- Failure
- Execution Time
- Activity Logs
- Retry Count

---

# 12. Error Handling

Methods:

Try-Catch Pattern

```
Pipeline

     |
 Execute Activity
     |
Success ----> Continue

Failure ----> Log Error
```

Activities:

- Fail Activity
- Webhook
- Email
- Azure Monitor

---

# 13. Security

Authentication:

- Managed Identity
- Service Principal
- Azure Key Vault
- SAS Token

Interview Tip:

Avoid storing passwords.

Use Azure Key Vault.

---

# 14. Azure Key Vault Integration

Store:

- Passwords
- Secrets
- Certificates

ADF fetches secrets securely.

---

# 15. Incremental Load

Instead of copying everything.

Copy only new records.

Example:

```
WHERE ModifiedDate >
LastLoadedDate
```

Interview favorite question.

---

# 16. Full Load vs Incremental Load

Full Load

```
Entire Table
```

Incremental

```
Only New Records
```

---

# 17. Mapping Data Flow vs Wrangling Data Flow

Mapping Data Flow

- Spark
- Large Data
- Enterprise ETL

Wrangling Data Flow

- Power Query
- Small Data
- Data Cleaning

---

# 18. CI/CD in Azure Data Factory

Very important for DevOps interviews.

Development Flow

```
Developer

↓

Feature Branch

↓

Publish

↓

ARM Template

↓

Azure DevOps

↓

Test

↓

Production
```

ADF integrates with Git.

Supports:

- Azure DevOps Repo
- GitHub

---

# 19. Publishing Process

Developer Mode

↓

Publish

↓

ADF Generates

```
ARM Templates
```

Deploy using

- Azure DevOps
- Release Pipeline
- YAML

---

# 20. ARM Templates

Generated automatically.

Contains:

```
Pipelines

Datasets

Triggers

Linked Services

Integration Runtime

Dataflows
```

---

# 21. CI/CD Steps

Step 1

Connect ADF to Git.

↓

Step 2

Create Feature Branch.

↓

Step 3

Develop Pipeline.

↓

Step 4

Publish.

↓

Step 5

Generate ARM Template.

↓

Step 6

Deploy through Azure DevOps.

↓

Step 7

Production.

---

# 22. YAML Example

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- task: AzureResourceManagerTemplateDeployment@3
  inputs:
    deploymentScope: Resource Group
```

---

# 23. Branch Strategy

```
main

develop

feature/*
```

Never develop directly in main.

---

# 24. Best Practices

- Parameterize everything
- Store secrets in Key Vault
- Use Managed Identity
- Enable Monitoring
- Enable Alerts
- Reuse Pipelines
- Use Incremental Loads
- Use Git Integration
- Follow Naming Standards
- Modular Design

---

# 25. Common Interview Questions

### What is Azure Data Factory?

Cloud ETL/ELT orchestration service.

---

### Difference between Pipeline and Activity?

Pipeline = Workflow

Activity = Individual Task

---

### Difference between Dataset and Linked Service?

Dataset = Data Structure

Linked Service = Connection

---

### What is Integration Runtime?

Execution engine that moves and transforms data.

---

### Types of Integration Runtime?

- Azure IR
- Self Hosted IR
- Azure SSIS IR

---

### What is Copy Activity?

Copies data from source to destination.

---

### What are Triggers?

Mechanism to automatically start pipelines.

---

### What is Parameterization?

Passing dynamic values to pipelines.

---

### Difference between ETL and ELT?

ETL

```
Extract
Transform
Load
```

Transformation happens before loading.

ELT

```
Extract
Load
Transform
```

Transformation happens after loading.

---

### What is Incremental Load?

Copy only changed or new records.

---

### Why use Azure Key Vault?

Secure secret management.

---

### What is Managed Identity?

Azure-managed identity for secure authentication without storing credentials.

---

### Explain CI/CD in ADF.

- Connect ADF to Git
- Develop in feature branches
- Publish changes
- Generate ARM templates
- Deploy via Azure DevOps pipelines
- Promote through Dev/Test/Prod environments

---

# 26. Real-Time Project Scenario

Scenario:

Daily sales data is stored in an on-premises SQL Server. Every night, the latest data needs to be loaded into Azure Data Lake, transformed, and made available for reporting in Azure Synapse.

Pipeline Steps:

1. Trigger runs at 12:00 AM.
2. Self-hosted Integration Runtime connects to on-prem SQL Server.
3. Lookup activity retrieves the last successful load timestamp.
4. Copy Activity performs an incremental load to Azure Data Lake.
5. Mapping Data Flow cleans and transforms the data.
6. Stored Procedure updates metadata in Azure SQL.
7. Success and failure notifications are sent.
8. Pipeline execution is monitored through Azure Monitor.

---

# 27. ADF Architecture

```
                +---------------------+
                | Source Systems      |
                | SQL | Oracle | API  |
                +----------+----------+
                           |
                    Linked Service
                           |
                  Integration Runtime
                           |
                     Azure Data Factory
                           |
      +--------------------+--------------------+
      |         Pipeline (Workflow)             |
      |-----------------------------------------|
      | Copy | Data Flow | Lookup | ForEach     |
      +--------------------+--------------------+
                           |
                    Azure Data Lake
                           |
                    Azure Synapse
                           |
                        Power BI
```

---

# 28. DevOps + ADF Interview Questions

1. How do you deploy ADF across environments?
2. How do ARM templates work?
3. How do you manage secrets?
4. How do you implement CI/CD?
5. How do you roll back deployments?
6. How do you parameterize linked services?
7. How do you monitor failed pipelines?
8. What is the role of Git integration?
9. How do you promote changes from Dev to Production?
10. What are global parameters in ADF?

---

# 29. Tips to Crack the Interview

- Understand ADF architecture and core components.
- Be comfortable explaining Integration Runtime and Linked Services.
- Know the difference between ETL and ELT.
- Practice Copy Activity and Mapping Data Flow.
- Learn pipeline parameterization and dynamic expressions.
- Understand Git integration, ARM templates, and CI/CD deployment with Azure DevOps.
- Prepare one real-world project scenario to discuss confidently.

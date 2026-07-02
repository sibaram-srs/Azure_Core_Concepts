# Major Azure Services and Their Definitions (Interview Ready)

Below is a structured, interview-focused list of the **most important Azure services** with clear definitions and use cases.

---

# 1. Compute Services

## Azure Virtual Machines (VMs)
A service that provides **on-demand virtual servers** in Azure.

- Used to run Windows/Linux workloads
- Full OS control
- Lift-and-shift migrations

---

## Azure Kubernetes Service (AKS)
A **managed Kubernetes platform** for deploying, scaling, and managing containerized applications.

- Orchestrates containers
- Handles scaling, rolling updates
- Integrates with Azure DevOps

---

## Azure App Service
A **fully managed PaaS service** for hosting web apps, APIs, and backend services.

- No infrastructure management
- Auto-scaling
- Supports .NET, Java, Node.js, Python

---

## Azure Functions
A **serverless compute service** that runs code in response to events.

- Event-driven execution
- Pay-per-execution
- No server management

---

# 2. Storage Services

## Azure Blob Storage
Object storage for **unstructured data** like images, videos, backups.

- Highly scalable
- Used for static content, logs, backups

---

## Azure Disk Storage
Provides **managed disks for VMs**.

- SSD/HDD options
- High performance for VM workloads

---

## Azure File Storage
Fully managed **file shares accessible via SMB/NFS**.

- Shared storage across VMs
- Lift-and-shift file systems

---

## Azure Table Storage
NoSQL key-value store for **structured but non-relational data**.

---

## Azure Queue Storage
Messaging service for **asynchronous communication between components**.

---

# 3. Networking Services

## Azure Virtual Network (VNet)
A private network in Azure that allows resources to communicate securely.

---

## Azure Load Balancer
A **Layer 4 load balancer** that distributes TCP/UDP traffic across multiple servers.

---

## Azure Application Gateway
A **Layer 7 load balancer** with SSL offloading, WAF, and URL-based routing.

---

## Azure Front Door
A **global Layer 7 load balancer** for fast and secure global application delivery.

---

## Azure VPN Gateway
Provides secure **site-to-site and point-to-site VPN connectivity**.

---

## Azure Bastion
Provides **secure RDP/SSH access to VMs without public IPs**.

---

## Azure DNS
Manages domain name resolution using Azure infrastructure.

---

# 4. Identity & Security

## Microsoft Entra ID (Azure AD)
Cloud-based identity service for authentication and authorization.

- User management
- Single Sign-On (SSO)
- MFA
- Role-Based Access Control

---

## Azure Key Vault
Secure service for storing **secrets, keys, and certificates**.

---

## Azure Defender (Microsoft Defender for Cloud)
Security monitoring and threat protection service.

- Security posture management
- Threat detection
- Compliance tracking

---

## Azure Policy
Enforces rules and compliance across Azure resources.

---

# 5. Database Services

## Azure SQL Database
Fully managed **relational database (PaaS)**.

- Auto-scaling
- Built-in backup
- High availability

---

## Azure Cosmos DB
Globally distributed **NoSQL database**.

- Low latency
- Multi-region replication
- Supports multiple APIs

---

## Azure Database for PostgreSQL/MySQL
Managed relational database services.

---

# 6. Integration Services

## Azure Service Bus
Enterprise messaging service for **reliable communication between applications**.

- Queues and Topics
- Guaranteed delivery
- Decoupled architecture

---

## Azure Event Grid
Event routing service for **event-driven architectures**.

- Reactive programming
- Serverless integration

---

## Azure Event Hubs
Big data streaming platform for **high-volume event ingestion**.

---

# 7. DevOps Services

## Azure DevOps
End-to-end DevOps platform.

- Boards (Agile planning)
- Repos (Git)
- Pipelines (CI/CD)
- Artifacts (package management)

---

## Azure Container Registry (ACR)
Private registry to store and manage **Docker images**.

---

## Azure Pipelines
CI/CD service to build and deploy applications.

---

# 8. Monitoring & Management

## Azure Monitor
Central monitoring platform for metrics, logs, and alerts.

---

## Log Analytics Workspace
Stores and queries logs using Kusto Query Language (KQL).

---

## Application Insights
Application performance monitoring (APM) tool.

- Tracks requests
- Detects failures
- Monitors performance

---

# 9. Container & Orchestration

## Azure Kubernetes Service (AKS)
Managed Kubernetes cluster for container orchestration.

---

## Azure Container Instances (ACI)
Run containers without managing servers.

---

## Azure Container Apps
Serverless container hosting with autoscaling.

---

# 10. AI & Analytics (Optional but Important)

## Azure Synapse Analytics
Big data and analytics platform combining data warehousing and data lakes.

---

## Azure Data Factory
Data integration service for ETL/ELT pipelines.

---

## Azure Machine Learning
Platform for building, training, and deploying ML models.

---

# 11. Simple Interview Summary

If asked:

> “What are the major Azure services?”

You can answer:

- Compute (VMs, AKS, App Service, Functions)
- Storage (Blob, Disk, File, Queue)
- Networking (VNet, Load Balancer, Front Door, App Gateway)
- Identity (Entra ID, Key Vault)
- Databases (SQL, Cosmos DB)
- Integration (Service Bus, Event Grid)
- DevOps (Azure DevOps, ACR, Pipelines)
- Monitoring (Azure Monitor, App Insights)

---

# One-Line Interview Answer

> **Azure provides a wide range of cloud services including compute, storage, networking, identity, databases, DevOps, and monitoring services that enable building scalable, secure, and highly available applications.**

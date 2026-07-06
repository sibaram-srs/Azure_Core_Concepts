# Azure Networking Interview Revision Notes

---

# 1. Virtual Network (VNet)

## What is it?

A **Virtual Network (VNet)** is the fundamental networking service in Azure. It allows Azure resources (VMs, App Services, AKS, etc.) to securely communicate with each other, the internet, and on-premises networks.

Think of it like your **private network inside Azure**.

Example:

```
Azure Subscription
    |
    +-- Production VNet (10.0.0.0/16)
            |
            +-- Web Subnet
            +-- App Subnet
            +-- Database Subnet
```

---

## Features

- Isolation from other VNets
- IP Address Management
- Internet Communication
- VNet Peering
- Hybrid Connectivity
- DNS Support
- Route Tables
- Security using NSGs and Firewall

---

## Address Space

When creating a VNet, assign an IP range.

Example:

```
VNet
Address Space:
10.0.0.0/16
```

This means:

```
10.0.0.1
to
10.0.255.254
```

---

## How to Configure

Azure Portal

```
Create Resource

↓

Networking

↓

Virtual Network

↓

Enter

Name
Region
Address Space

↓

Create Subnets

↓

Review + Create
```

CLI

```bash
az network vnet create \
--resource-group RG1 \
--name ProdVNet \
--address-prefix 10.0.0.0/16
```

---

## Benefits

- Secure communication
- Resource isolation
- Hybrid networking
- Supports VPN and ExpressRoute
- Easy subnet management

---

# 2. Subnets

## What is it?

A subnet divides a VNet into smaller logical networks.

Instead of putting every VM together, you separate them.

Example:

```
VNet
10.0.0.0/16

|

+-- Web
10.0.1.0/24

+-- App
10.0.2.0/24

+-- DB
10.0.3.0/24
```

---

## Why use Subnets?

- Better Security
- Easier Management
- Separate workloads
- Apply NSGs
- Apply Route Tables

---

## Configuration

Portal

```
VNet

↓

Subnets

↓

Add Subnet

↓

Name

Address Prefix

↓

Save
```

CLI

```bash
az network vnet subnet create \
--resource-group RG1 \
--vnet-name ProdVNet \
--name WebSubnet \
--address-prefixes 10.0.1.0/24
```

---

## Benefits

- Traffic isolation
- Security
- Better IP management
- Required for Azure Firewall, VPN Gateway etc.

---

# 3. Network Security Group (NSG)

## What is it?

An NSG is a Layer-4 firewall.

It filters traffic based on

- Source IP
- Destination IP
- Port
- Protocol
- Direction

---

Example

```
Internet

↓

NSG

↓

VM
```

Only allowed traffic enters.

---

## NSG Rules

Each rule has

- Priority
- Source
- Destination
- Port
- Protocol
- Action

Example

```
Priority : 100

Source : Internet

Destination : VM

Port : 80

Action : Allow
```

---

## Default Rules

Azure automatically creates:

```
Allow VNet

Allow Azure Load Balancer

Deny All Inbound
```

Outbound:

```
Allow Internet

Allow VNet

Deny Others
```

---

## How to Configure

Portal

```
Create NSG

↓

Inbound Rules

↓

Add Rule

↓

Attach to

VM

or

Subnet
```

CLI

```bash
az network nsg create \
--resource-group RG1 \
--name WebNSG
```

Add Rule

```bash
az network nsg rule create
```

---

## Benefits

- Controls traffic
- Protects VMs
- Easy rule management
- Stateful firewall

---

# 4. Application Security Groups (ASG)

## What is it?

ASG allows grouping VMs logically instead of using IP addresses.

Instead of writing rules for IPs,

Write rules for application groups.

Example

```
Web Servers

↓

ASG-Web

App Servers

↓

ASG-App
```

Rule

```
Allow

ASG-Web

↓

ASG-App

Port 443
```

No IP required.

---

## Configuration

```
Create ASG

↓

Add VM

↓

Create NSG Rule

↓

Source ASG

Destination ASG
```

---

## Benefits

- Easy management
- No IP dependency
- Cleaner NSG rules
- Scalable

---

# 5. Azure Firewall

## What is it?

Azure Firewall is a fully managed stateful firewall service.

It protects the entire Azure network.

Unlike NSG,

Azure Firewall supports

- Application rules
- Network rules
- DNAT
- Threat Intelligence
- Logging

---

Architecture

```
Internet

↓

Azure Firewall

↓

Subnet

↓

VMs
```

---

## Firewall Rules

### Network Rules

Based on

- IP
- Port
- Protocol

---

### Application Rules

Based on

```
www.google.com

github.com

microsoft.com
```

---

### NAT Rules

Public IP

↓

Private VM

---

## Configuration

```
Create Firewall

↓

Dedicated Firewall Subnet

↓

Assign Public IP

↓

Configure Rules

↓

Update Route Tables
```

Firewall subnet must be

```
AzureFirewallSubnet
```

---

## Benefits

- Central security
- High availability
- Logging
- Threat Intelligence
- FQDN filtering

---

# 6. Azure Load Balancer

## What is it?

Distributes traffic across multiple backend servers.

Works at

```
Layer 4

TCP

UDP
```

---

Example

```
Users

↓

Load Balancer

↓

VM1

↓

VM2

↓

VM3
```

---

## Types

### Public

Internet traffic

### Internal

Private traffic

---

## Components

- Frontend IP
- Backend Pool
- Health Probe
- Load Balancing Rule

---

Health Probe

Checks

```
VM alive?

↓

Yes

↓

Send traffic
```

---

## Configuration

```
Create LB

↓

Frontend

↓

Backend Pool

↓

Health Probe

↓

LB Rule
```

---

## Benefits

- High Availability
- Fault Tolerance
- Scalability
- Automatic Distribution

---

# 7. Application Gateway

## What is it?

Layer 7 Load Balancer.

Works with

HTTP

HTTPS

Unlike Load Balancer,

it understands URLs.

---

Example

```
/images

↓

Image Server

/api

↓

API Server

/admin

↓

Admin Server
```

---

## Features

- SSL Offloading
- URL Routing
- Cookie Affinity
- WAF
- HTTP Redirect

---

Architecture

```
Internet

↓

Application Gateway

↓

Backend Pool
```

---

## Configuration

```
Create Gateway

↓

Frontend IP

↓

Listener

↓

Backend Pool

↓

Routing Rules
```

---

## Benefits

- Layer 7 Routing
- WAF
- SSL termination
- URL based routing

---

# Load Balancer vs Application Gateway

| Feature | Load Balancer | Application Gateway |
|----------|---------------|---------------------|
| Layer | 4 | 7 |
| Protocol | TCP UDP | HTTP HTTPS |
| URL Routing | No | Yes |
| SSL Termination | No | Yes |
| WAF | No | Yes |

---

# 8. VPN Gateway

## What is it?

VPN Gateway securely connects Azure to:

- On-Premises
- Another Azure VNet
- Remote Users

using encrypted tunnels.

---

Types

### Site-to-Site

Office ↔ Azure

### Point-to-Site

Laptop ↔ Azure

### VNet-to-VNet

Azure ↔ Azure

---

Architecture

```
Office

↓

Internet

↓

VPN Gateway

↓

Azure
```

---

## Configuration

```
Gateway Subnet

↓

Create VPN Gateway

↓

Public IP

↓

Local Network Gateway

↓

Connection

↓

Shared Key
```

Gateway subnet name must be

```
GatewaySubnet
```

---

## Benefits

- Secure communication
- Hybrid Cloud
- Encrypted tunnels
- Cost-effective

---

# 9. ExpressRoute

## What is it?

ExpressRoute provides a **private dedicated connection** between Azure and your on-premises network.

It does **not** use the public internet.

---

Architecture

```
Office

↓

ISP

↓

ExpressRoute

↓

Azure
```

---

## Benefits

- Faster
- Lower latency
- Private
- Reliable
- Higher bandwidth

---

## VPN vs ExpressRoute

| VPN | ExpressRoute |
|------|--------------|
| Internet | Private Circuit |
| Cheaper | Expensive |
| Encrypted | Private |
| Lower Speed | High Speed |
| Good for SMB | Good for Enterprise |

---

# 10. Azure DNS

## What is it?

Azure DNS hosts public DNS zones.

Example

```
company.com

↓

Azure DNS

↓

52.100.20.15
```

---

## Records

- A
- AAAA
- CNAME
- MX
- TXT
- NS

---

## Configuration

```
Create DNS Zone

↓

Add Record

↓

Update Name Servers
```

---

## Benefits

- Highly Available
- Fast
- Secure
- Global

---

# 11. Private DNS Zone

## What is it?

Private DNS resolves names only inside Azure VNets.

Not accessible from the internet.

Example

```
db.internal.local

↓

10.0.2.5
```

---

## Configuration

```
Create Private DNS Zone

↓

Link VNet

↓

Add Records
```

---

## Benefits

- Internal name resolution
- Secure
- No public exposure

---

# Azure DNS vs Private DNS

| Azure DNS | Private DNS |
|------------|-------------|
| Public | Internal |
| Internet Accessible | VNet Only |
| Public Websites | Internal Applications |

---

# 12. Troubleshooting Connectivity

---

## NSG Flow Logs

### What is it?

Records all traffic allowed or denied by an NSG.

Useful for troubleshooting.

Example

```
VM

↓

NSG

↓

Flow Logs

↓

Storage Account
```

Shows

```
Source IP

Destination IP

Port

Allow/Deny
```

Enable

```
NSG

↓

Diagnostic Settings / NSG Flow Logs (via Network Watcher)

↓

Storage Account

↓

Enable
```

Benefits

- Security auditing
- Troubleshooting
- Traffic analysis

> Note: NSG flow logs are configured through **Azure Network Watcher** and stored in a Storage Account (or integrated with Log Analytics, depending on your setup).

---

## Packet Capture

### What is it?

Captures network packets from Azure VMs.

Similar to Wireshark.

Useful when packets are reaching the VM but the application still isn't working.

---

Configuration

```
Network Watcher

↓

Packet Capture

↓

Select VM

↓

Capture

↓

Download File

↓

Analyze
```

File format

```
.pcap
```

---

Benefits

- Deep packet analysis
- Debug network issues
- Diagnose application communication problems

---

# Common Interview Questions

### What is the difference between NSG and Azure Firewall?

| NSG | Azure Firewall |
|------|---------------|
| Layer 4 | Layer 3–7 (supports network and application filtering) |
| Works on Subnet/VM NIC | Protects entire VNet or hub |
| Free (service itself) | Paid |
| Basic filtering | Advanced filtering, DNAT, FQDN rules, Threat Intelligence |

---

### Difference between VNet and Subnet?

VNet is the entire private network.

Subnet is a smaller logical division inside the VNet.

---

### Difference between Load Balancer and Application Gateway?

Load Balancer works at Layer 4 using TCP/UDP.

Application Gateway works at Layer 7 using HTTP/HTTPS and supports URL-based routing, SSL termination, and WAF.

---

### Difference between VPN and ExpressRoute?

VPN uses the public internet with encrypted tunnels.

ExpressRoute uses a private dedicated circuit, offering lower latency, higher bandwidth, and more consistent performance.

---

### What is GatewaySubnet?

A reserved subnet required for deploying Azure VPN Gateway or ExpressRoute Gateway.

---

### What is AzureFirewallSubnet?

A dedicated subnet required for Azure Firewall. The subnet must be named **AzureFirewallSubnet**.

---

### Can one NSG be attached to multiple subnets?

Yes. An NSG can be associated with multiple subnets or multiple NICs, depending on your design.

---

### Can NSG be attached to a VM?

Not directly to the VM object. It is attached to the **VM's Network Interface (NIC)** or to the **subnet** where the VM resides.

---

### What happens if both Subnet NSG and NIC NSG exist?

Both are evaluated. Traffic must be **allowed by both** effective rule sets to pass. A deny in either location blocks the traffic.

---

### Which service is used to troubleshoot Azure networking?

**Azure Network Watcher** provides tools such as:
- IP Flow Verify
- Next Hop
- Effective Security Rules
- Connection Troubleshoot
- NSG Flow Logs
- Packet Capture
- Topology

---

# Easy Memory Trick

```
VNet
│
├── Subnets
│     ├── NSG
│     ├── ASG
│     ├── Azure Firewall
│
├── Load Balancer (Layer 4)
│
├── Application Gateway (Layer 7 + WAF)
│
├── VPN Gateway (Encrypted Internet)
│
├── ExpressRoute (Private Connection)
│
├── Azure DNS (Public)
│
├── Private DNS (Internal)
│
└── Network Watcher
      ├── NSG Flow Logs
      └── Packet Capture
```


# Azure Storage Interview Revision Notes

---

# 1. Azure Storage

## What is it?

**Azure Storage** is Microsoft's cloud storage service that provides **highly available, durable, scalable, and secure storage** for structured and unstructured data.

It is used to store:

- Files
- Images
- Videos
- Logs
- Backups
- Virtual Machine disks
- Application data
- Messages

Think of Azure Storage as a **cloud-based hard drive** that can be accessed securely from anywhere.

---

## Types of Azure Storage

Azure Storage Account provides four major storage services:

```
Storage Account
│
├── Blob Storage
├── File Storage
├── Queue Storage
└── Table Storage
```

---

## Features

- High Availability
- Scalability
- Encryption at Rest
- Backup Support
- Access Control
- Lifecycle Management
- Multiple Access Tiers

---

## Benefits

- Highly Durable (11 9's durability)
- Globally Available
- Low Cost
- Secure
- Easy Integration with Azure Services

---

# 2. Storage Account

## What is it?

A **Storage Account** is a container that holds all Azure Storage services.

Everything inside Azure Storage must exist inside a Storage Account.

Example:

```
Storage Account
storageprod001

│
├── Blob Containers
├── Azure Files
├── Queues
└── Tables
```

---

## Types of Storage Accounts

### General Purpose v2 (GPv2)

Most commonly used.

Supports:

- Blob
- Files
- Queue
- Table
- Data Lake Gen2

---

### Premium Storage

Used for high-performance workloads.

Supports SSD-backed storage.

---

## Configuration

Azure Portal

```
Create Resource

↓

Storage Account

↓

Select

Subscription

Resource Group

Storage Account Name

Region

Performance

Redundancy

↓

Review + Create
```

CLI

```bash
az storage account create \
--name mystorage123 \
--resource-group RG1 \
--location eastus \
--sku Standard_LRS
```

---

## Benefits

- Central place to store data
- Secure
- Scalable
- Supports multiple storage services

---

# 3. Blob Storage

## What is it?

Blob Storage stores **unstructured data**.

Examples:

- Images
- Videos
- PDFs
- Documents
- Backups
- VM disks
- Logs

Blob = Binary Large Object

---

Architecture

```
Storage Account

↓

Container

↓

Blob Files
```

Example

```
storageaccount

↓

images

↓

photo.jpg

logo.png
```

---

## Blob Types

### Block Blob

Most common.

Used for:

- Images
- Documents
- Videos

---

### Append Blob

Optimized for logging.

Example

```
Application Logs
```

---

### Page Blob

Used for Azure VM disks.

---

## Configuration

```
Storage Account

↓

Containers

↓

Create Container

↓

Upload Files
```

CLI

```bash
az storage container create
```

---

## Benefits

- Unlimited scalability
- Cheap
- Secure
- Lifecycle Management
- Global Access

---

# 4. Azure Files

## What is it?

Azure Files provides **fully managed SMB/NFS file shares** in the cloud.

It behaves like a shared network drive.

Example

```
Company Shared Folder

↓

Azure Files

↓

Employees Access
```

---

Supports

- Windows
- Linux
- macOS

---

Protocols

- SMB
- NFS

---

Configuration

```
Storage Account

↓

File Shares

↓

Create Share

↓

Mount on VM
```

---

Benefits

- Shared storage
- Lift-and-shift applications
- Easy migration
- Multiple VM access

---

# 5. Queue Storage

## What is it?

Queue Storage stores messages between applications.

Used for asynchronous communication.

Example

```
User Uploads File

↓

Queue

↓

Background Worker

↓

Processes File
```

---

Maximum Message Size

```
64 KB
```

---

Benefits

- Decouples applications
- Reliable messaging
- Scalable
- Handles spikes

---

Configuration

```
Storage Account

↓

Queues

↓

Create Queue
```

---

# 6. Table Storage

## What is it?

Table Storage is a NoSQL key-value database.

Stores structured but non-relational data.

---

Example

```
Employee

PartitionKey

RowKey

Name

Salary
```

---

No tables like SQL.

No joins.

No relationships.

---

Benefits

- Fast
- Cheap
- Massive Scale
- Flexible Schema

---

Configuration

```
Storage Account

↓

Tables

↓

Create Table
```

---

# Blob vs File vs Queue vs Table

| Service | Used For |
|----------|-----------|
| Blob | Images, Videos, Backups |
| File | Shared File System |
| Queue | Messages |
| Table | NoSQL Data |

---

# 7. Access Tiers

## What is it?

Blob Storage supports different pricing tiers based on access frequency.

```
Hot

↓

Cool

↓

Archive
```

---

## Hot Tier

Used when data is accessed frequently.

Examples

- Active Website Images
- Current Documents

Cost

```
Storage Cost = High

Access Cost = Low
```

---

## Cool Tier

Used for infrequently accessed data.

Examples

- Monthly Reports
- Backup Files

Cost

```
Storage = Lower

Access = Higher
```

---

## Archive Tier

Used for rarely accessed data.

Examples

- Compliance
- Old Backups
- Historical Data

Cost

```
Storage = Lowest

Access = Highest

Retrieval Time = Hours
```

---

## Configuration

```
Blob

↓

Change Access Tier

↓

Hot

Cool

Archive
```

---

## Benefits

- Saves Cost
- Lifecycle Management
- Optimized Storage

---

# Hot vs Cool vs Archive

| Feature | Hot | Cool | Archive |
|----------|------|------|----------|
| Storage Cost | High | Medium | Lowest |
| Access Cost | Low | High | Highest |
| Access Frequency | Frequent | Rare | Very Rare |
| Availability | Immediate | Immediate | Rehydration Required |

---

# 8. SAS Token (Shared Access Signature)

## What is it?

A **Shared Access Signature (SAS)** provides **temporary, limited access** to storage resources **without sharing the account key**.

Instead of giving someone the Storage Account credentials, generate a SAS token with specific permissions and an expiry time.

---

Example

```
Blob

↓

Generate SAS

↓

User Accesses Blob

↓

Expires Automatically
```

---

Permissions

- Read
- Write
- Delete
- List
- Create

---

Example

```
Read Only

Valid for 2 Hours
```

---

Configuration

```
Storage Account

↓

Container

↓

Generate SAS

↓

Select

Permissions

Expiry

↓

Generate URL
```

---

Benefits

- Temporary Access
- More Secure than sharing keys
- Granular Permissions
- Time-based Expiry

---

# 9. Access Keys

## What is it?

Every Storage Account has **two access keys**.

These keys provide **full administrative access** to all data in the storage account.

Example

```
Storage Account

↓

Key1

Key2
```

Two keys allow **key rotation** without downtime.

---

Configuration

```
Storage Account

↓

Access Keys

↓

Copy Connection String
```

---

Benefits

- Full Storage Access
- Easy Authentication
- Supports Key Rotation

---

## Access Keys vs SAS Token

| Access Keys | SAS Token |
|-------------|-----------|
| Full Access | Limited Access |
| Long-Term | Temporary |
| Less Secure if Shared | More Secure |
| No Expiry | Configurable Expiry |

---

# 10. Azure RBAC (Role-Based Access Control)

## What is it?

RBAC controls **who can perform actions on Azure resources** based on assigned roles.

Instead of sharing storage keys, users can authenticate with Microsoft Entra ID (Azure AD) and receive only the permissions they need.

---

Common Roles

| Role | Permission |
|------|------------|
| Owner | Full access including access management |
| Contributor | Manage resources, cannot assign roles |
| Reader | View only |
| Storage Blob Data Reader | Read blobs only |
| Storage Blob Data Contributor | Read, write, delete blobs |
| Storage Blob Data Owner | Full blob data access |

---

Configuration

```
Storage Account

↓

Access Control (IAM)

↓

Add Role Assignment

↓

Choose Role

↓

Select User

↓

Assign
```

CLI

```bash
az role assignment create
```

---

Benefits

- Least Privilege Access
- Secure Authentication
- Centralized Access Management
- Auditing

---

# RBAC vs Access Keys vs SAS

| Feature | RBAC | Access Keys | SAS |
|----------|------|-------------|-----|
| Identity-Based | Yes | No | Optional |
| Temporary | No | No | Yes |
| Granular Permissions | Yes | Limited | Yes |
| Recommended | ✅ Yes | Only when necessary | Yes for temporary sharing |

---

# 11. Backup & Restore

## What is it?

Azure Storage data can be protected using:

- Blob Soft Delete
- Versioning
- Snapshots
- Azure Backup (for Azure Files)
- Point-in-Time Restore (supported scenarios)

These features help recover accidentally deleted or modified data.

---

### Blob Soft Delete

If a blob is deleted, it can be recovered during the configured retention period.

```
Delete Blob

↓

Soft Delete

↓

Restore
```

---

### Blob Versioning

Stores previous versions whenever a blob is modified.

```
Version 1

↓

Version 2

↓

Version 3

↓

Restore Any Version
```

---

### Snapshots

Creates a read-only copy of a blob or file share at a specific point in time.

Useful before major updates.

---

### Azure Backup (Azure Files)

Protects Azure File Shares with scheduled backups and restores.

---

Configuration

```
Storage Account

↓

Data Protection

↓

Enable

Soft Delete

Versioning

Point-in-Time Restore (if applicable)

↓

Configure Azure Backup for Azure Files
```

---

Benefits

- Recover deleted data
- Protection against accidental changes
- Business Continuity
- Disaster Recovery

---

# 12. Troubleshooting Storage Latency

## What is Latency?

Latency is the time taken to read or write data from Azure Storage.

Example

```
Application

↓

Storage

↓

Response Time = 500 ms
```

---

## Common Causes

- Large file uploads
- Network latency
- Incorrect storage tier
- Throttling due to high request rates
- Application making too many small requests

---

## Troubleshooting Steps

1. Check Azure Monitor metrics
2. Check Storage Account metrics
3. Review transaction counts and latency metrics
4. Verify storage tier (Hot/Cool/Archive)
5. Use Azure Monitor Logs and diagnostic settings
6. Check client location relative to the storage region
7. Review retry logic in the application

---

## Best Practices

- Keep compute close to storage (same region)
- Use Premium Storage for performance-sensitive workloads
- Upload/download in parallel when appropriate
- Enable CDN for static content

---

# 13. Troubleshooting Access Issues

## Common Issues

### Authentication Failure

Possible causes:

- Wrong Access Key
- Expired SAS Token
- Invalid Connection String

---

### Authorization Failure (403 Forbidden)

Possible causes:

- Missing RBAC permissions
- SAS missing required permissions
- Firewall or networking restrictions
- Storage account network rules blocking access

---

### Network Issues

Check:

- Storage Firewall
- Private Endpoint configuration
- Service Endpoints
- NSGs (if accessing through private networking)
- DNS resolution for private endpoints

---

### Blob Not Found (404)

Possible causes:

- Wrong container name
- Wrong blob path
- Blob deleted
- Incorrect URL

---

### Tools for Troubleshooting

- Azure Monitor
- Storage Account Metrics
- Diagnostic Settings
- Activity Log
- Storage Explorer
- Azure Portal IAM (RBAC)
- Network Watcher (for networking-related issues)

---

# Common Interview Questions

### What is a Storage Account?

A Storage Account is the top-level Azure resource that contains storage services like Blob, File, Queue, and Table.

---

### Difference between Blob and File Storage?

| Blob | Azure Files |
|------|-------------|
| Object Storage | File Share |
| HTTP/HTTPS Access | SMB/NFS |
| Images, Videos, Backups | Shared Drives |

---

### Difference between Queue Storage and Service Bus?

| Queue Storage | Azure Service Bus |
|---------------|-------------------|
| Simple messaging | Advanced enterprise messaging |
| Lower cost | Rich features (topics, sessions, dead-lettering) |
| Basic queue | FIFO, duplicate detection, transactions |

---

### What is a SAS Token?

A secure URL that grants **temporary, limited permissions** to storage resources without exposing the storage account keys.

---

### Why are there two Access Keys?

To support **key rotation**. One key can be regenerated while applications continue using the other.

---

### Which is more secure: Access Keys or RBAC?

**RBAC** is more secure because it uses Microsoft Entra ID and follows the principle of least privilege.

---

### Difference between Hot, Cool, and Archive?

- **Hot** → Frequently accessed data
- **Cool** → Infrequently accessed data
- **Archive** → Rarely accessed data; requires rehydration before use

---

### What is Soft Delete?

A feature that allows recovery of deleted blobs or file shares during a configurable retention period.

---

### What is Versioning?

Automatically keeps previous versions of blobs so earlier versions can be restored if needed.

---

### How do you troubleshoot a 403 error on Blob Storage?

Check:
- RBAC permissions
- SAS token validity and permissions
- Access key/connection string
- Storage firewall and network rules
- Private endpoint or DNS configuration (if used)

---

# Easy Memory Trick

```
Storage Account
│
├── Blob Storage
│     ├── Hot
│     ├── Cool
│     └── Archive
│
├── Azure Files
│
├── Queue Storage
│
├── Table Storage
│
├── Security
│     ├── RBAC
│     ├── SAS Token
│     └── Access Keys
│
├── Data Protection
│     ├── Soft Delete
│     ├── Versioning
│     ├── Snapshots
│     └── Azure Backup
│
└── Troubleshooting
      ├── Latency
      ├── Access Issues
      ├── Azure Monitor
      └── Storage Explorer
```


# Azure Identity & Access Management (IAM) Interview Revision Notes

---

# 1. Microsoft Entra ID (Azure AD)

## What is it?

**Microsoft Entra ID (formerly Azure Active Directory)** is Microsoft's cloud-based **Identity and Access Management (IAM)** service.

It is used to **authenticate users** (Who are you?) and **authorize access** (What are you allowed to do?).

It manages:

- Users
- Groups
- Applications
- Devices
- Service Principals
- Managed Identities

Think of it as the **central identity provider** for Azure and Microsoft 365.

---

## Authentication vs Authorization

Authentication = Verify Identity

```
Username + Password

↓

Microsoft Entra ID

↓

Verified User
```

Authorization = Verify Permissions

```
Verified User

↓

RBAC

↓

Allowed to Create VM
```

---

## Components

```
Microsoft Entra ID

│
├── Users
├── Groups
├── Applications
├── Service Principals
├── Managed Identities
└── Conditional Access
```

---

## Features

- Single Sign-On (SSO)
- Multi-Factor Authentication (MFA)
- Identity Management
- Device Management
- Conditional Access
- OAuth/OpenID Connect Support
- Integration with Microsoft 365 and Azure

---

## Configuration

Portal

```
Azure Portal

↓

Microsoft Entra ID

↓

Users

↓

New User

↓

Assign Roles/Groups
```

CLI

```bash
az ad user create
```

---

## Benefits

- Centralized identity management
- Secure authentication
- SSO
- MFA support
- Works across Azure and Microsoft 365

---

# 2. Role-Based Access Control (RBAC)

## What is it?

**RBAC (Role-Based Access Control)** controls **who can do what** on Azure resources.

Instead of giving everyone full access, assign **roles** based on job responsibilities.

RBAC answers:

```
Who?

Can do What?

On Which Resource?
```

---

## RBAC Components

```
Security Principal

↓

Role Definition

↓

Scope

↓

Access Granted
```

---

### 1. Security Principal

The identity receiving permissions.

Examples:

- User
- Group
- Service Principal
- Managed Identity

---

### 2. Role Definition

Defines permissions.

Examples:

```
Reader

Contributor

Owner

Virtual Machine Contributor

Storage Blob Data Reader
```

---

### 3. Scope

Where the role applies.

Hierarchy:

```
Management Group

↓

Subscription

↓

Resource Group

↓

Resource
```

Permissions inherited downward.

Example:

```
Contributor

↓

Resource Group

↓

All Resources inside RG
```

---

## Built-in Roles

### Owner

Can:

- Create resources
- Delete resources
- Assign RBAC roles

---

### Contributor

Can:

- Create
- Update
- Delete Resources

Cannot:

- Assign RBAC roles

---

### Reader

Can:

- View resources only

Cannot modify anything.

---

### User Access Administrator

Can manage role assignments but cannot manage resources unless granted additional roles.

---

## Configuration

Portal

```
Resource

↓

Access Control (IAM)

↓

Add Role Assignment

↓

Choose Role

↓

Select User

↓

Save
```

CLI

```bash
az role assignment create
```

---

## Benefits

- Least Privilege Access
- Secure Management
- Easy Auditing
- Centralized Permissions

---

# RBAC Scope Hierarchy

```
Management Group

↓

Subscription

↓

Resource Group

↓

VM

↓

Disk

↓

NIC
```

Permission inheritance:

```
Subscription

↓

Resource Group

↓

VM

↓

Disk
```

---

# 3. Managed Identities

## What is it?

A **Managed Identity** is an automatically managed identity in Microsoft Entra ID for an Azure resource.

It allows Azure resources to authenticate to other Azure services **without storing usernames, passwords, or secrets**.

Instead of storing credentials inside code:

```
VM

↓

Managed Identity

↓

Azure Key Vault
```

No passwords.

No secrets.

---

## Types

### System Assigned

One identity linked to one Azure resource.

```
VM

↓

Managed Identity

↓

Deleted when VM deleted
```

Characteristics:

- Automatically created
- Automatically deleted with the resource
- One-to-one relationship

---

### User Assigned

Independent identity.

Can be attached to multiple Azure resources.

```
Managed Identity

↓

VM1

VM2

App Service
```

Characteristics:

- Created separately
- Reusable
- Exists independently

---

## Example

Without Managed Identity

```
App

↓

Username

Password

↓

Key Vault
```

Risk:

Credentials stored in code.

---

With Managed Identity

```
App

↓

Managed Identity

↓

Microsoft Entra ID

↓

Key Vault
```

No credentials required.

---

## Configuration

Portal

```
VM

↓

Identity

↓

Enable System Assigned

↓

Save

↓

Assign RBAC Role on Target Resource
```

---

CLI

```bash
az vm identity assign
```

---

## Benefits

- No secrets in code
- Secure authentication
- Automatic credential rotation
- Simplified application development

---

# System Assigned vs User Assigned

| Feature | System Assigned | User Assigned |
|----------|----------------|---------------|
| Lifecycle | Tied to Resource | Independent |
| Multiple Resources | No | Yes |
| Automatically Deleted | Yes | No |
| Reusable | No | Yes |

---

# 4. Conditional Access (Basics)

## What is it?

**Conditional Access** allows administrators to apply security policies based on conditions before granting access.

Think of it as:

```
IF condition is true

THEN apply policy
```

Example:

```
If user logs in from outside India

↓

Require MFA
```

---

## Common Conditions

- User or Group
- Device
- Location
- Risk Level
- Application
- Operating System

---

## Common Controls

- Require MFA
- Block Access
- Require Compliant Device
- Require Password Change
- Require Hybrid Azure AD Joined Device

---

Example Policy

```
Users

↓

Outside Office Network

↓

Require MFA
```

---

Architecture

```
User Login

↓

Conditional Access

↓

Evaluate Conditions

↓

Grant or Block
```

---

## Configuration

Portal

```
Microsoft Entra ID

↓

Protection

↓

Conditional Access

↓

New Policy

↓

Select Users

↓

Conditions

↓

Grant Controls

↓

Enable Policy
```

---

## Benefits

- Increased Security
- Zero Trust
- Protect Against Stolen Credentials
- Flexible Access Policies

---

# Multi-Factor Authentication (MFA)

## What is MFA?

MFA requires users to provide **two or more authentication factors**.

Example

```
Password

+

Phone Approval
```

Factors include:

- Password (Something you know)
- Mobile App / OTP (Something you have)
- Biometrics (Something you are)

---

Benefits

- Prevents password-only attacks
- Reduces account compromise
- Required by many Conditional Access policies

---

# 5. Troubleshooting Login Issues

## Common Login Problems

### Wrong Username or Password

Check:

- Correct username
- Password reset if needed

---

### MFA Failure

Possible causes:

- Phone unavailable
- Authenticator app not working
- Incorrect time on device
- User not registered for MFA

---

### Account Locked

Possible reasons:

- Too many failed login attempts
- Administrator disabled account

Check:

```
Microsoft Entra ID

↓

Users

↓

Account Status
```

---

### Conditional Access Block

Possible causes:

- Login from blocked country
- Unmanaged device
- Policy requires MFA
- Device not compliant

Review:

```
Microsoft Entra ID

↓

Sign-in Logs

↓

Conditional Access Tab
```

---

### Expired Password

Reset password.

---

### Disabled User

Enable account in Microsoft Entra ID.

---

# Troubleshooting Permission Issues

## Scenario

User can sign in but cannot access an Azure resource.

---

### Check RBAC

Verify role assignment.

```
Resource

↓

IAM

↓

Role Assignments
```

---

### Check Scope

Maybe permission exists at:

```
Subscription

Not

Resource Group
```

or vice versa.

---

### Check Role

Maybe user is:

```
Reader

Instead of

Contributor
```

---

### Check Group Membership

If access comes through a group:

```
User

↓

Group

↓

RBAC Role
```

Ensure the user belongs to the correct group.

---

### Check Managed Identity Permissions

If an application uses Managed Identity:

Verify it has the required RBAC role on the target resource.

---

### Check PIM (If Used)

If **Microsoft Entra Privileged Identity Management (PIM)** is enabled, ensure the eligible role has been activated.

---

### Check Resource Locks

Resource locks can prevent changes even when RBAC permissions exist.

---

# Useful Troubleshooting Tools

| Tool | Purpose |
|------|----------|
| Microsoft Entra Sign-in Logs | Login failures, Conditional Access evaluation |
| Audit Logs | User and role changes |
| Access Control (IAM) | Verify RBAC assignments |
| Activity Log | Azure resource operations |
| My Sign-Ins | User view of login history |
| Microsoft Entra Portal | Identity management |

---

# Authentication Flow

```
User

↓

Microsoft Entra ID

↓

Authentication

↓

Conditional Access

↓

RBAC Authorization

↓

Azure Resource
```

---

# Common Interview Questions

### What is Microsoft Entra ID?

A cloud-based Identity and Access Management (IAM) service used to authenticate users and manage access to Azure and Microsoft services.

---

### Difference between Authentication and Authorization?

| Authentication | Authorization |
|----------------|---------------|
| Verifies identity | Determines permissions |
| "Who are you?" | "What can you do?" |

---

### What is RBAC?

RBAC is Azure's authorization system that grants permissions based on assigned roles at specific scopes.

---

### What are the main built-in RBAC roles?

- Owner
- Contributor
- Reader
- User Access Administrator

---

### Difference between Owner and Contributor?

| Owner | Contributor |
|--------|-------------|
| Can manage resources | Can manage resources |
| Can assign roles | Cannot assign roles |

---

### What is a Managed Identity?

A Microsoft Entra identity automatically managed for an Azure resource, allowing secure access to other Azure services without storing credentials.

---

### Difference between System Assigned and User Assigned Managed Identity?

| System Assigned | User Assigned |
|----------------|---------------|
| One resource | Multiple resources |
| Deleted with resource | Independent lifecycle |
| Not reusable | Reusable |

---

### What is Conditional Access?

A feature that enforces access policies based on conditions such as user, location, device, or risk. It is commonly used to require MFA or block risky sign-ins.

---

### Difference between RBAC and Conditional Access?

| RBAC | Conditional Access |
|------|--------------------|
| Controls resource permissions | Controls sign-in conditions |
| Authorization | Authentication/Access policy |
| Defines what users can do | Defines how and when users can sign in |

---

### How do you troubleshoot a user who cannot access a VM?

1. Verify the user can sign in to Microsoft Entra ID.
2. Check Conditional Access policies and Sign-in Logs.
3. Verify RBAC role assignments.
4. Confirm the correct scope (Subscription/RG/Resource).
5. Check group membership (if permissions are inherited).
6. Verify resource locks or PIM activation if applicable.

---

# Easy Memory Trick

```
Microsoft Entra ID
│
├── Users
├── Groups
├── Applications
├── Service Principals
├── Managed Identities
│
├── Authentication
│     ├── Password
│     ├── MFA
│     └── SSO
│
├── Conditional Access
│     ├── User
│     ├── Device
│     ├── Location
│     └── Risk
│
├── RBAC
│     ├── Owner
│     ├── Contributor
│     ├── Reader
│     └── User Access Administrator
│
└── Troubleshooting
      ├── Sign-in Logs
      ├── Audit Logs
      ├── IAM
      └── Activity Log
```


# Azure Monitoring & Logging Interview Revision Notes

---

# 1. Azure Monitor

## What is it?

**Azure Monitor** is Azure's centralized monitoring service that collects, analyzes, and acts on telemetry from Azure resources, applications, virtual machines, containers, and networks.

It helps answer questions like:

- Is my VM running?
- Why is my application slow?
- Is CPU usage high?
- Did a resource fail?
- Who deleted a resource?

Think of Azure Monitor as the **health dashboard of your Azure environment.**

---

## Azure Monitor Architecture

```
Azure Resources
        │
        ▼
Azure Monitor
        │
 ┌──────┼─────────┐
 │      │         │
 ▼      ▼         ▼
Metrics Logs   Alerts
        │
        ▼
Log Analytics Workspace
        │
        ▼
KQL Queries
```

---

## What Azure Monitor Collects

- Metrics
- Logs
- Activity Logs
- Platform Logs
- Application Logs
- Guest OS Metrics
- Diagnostic Logs

---

## Components

```
Azure Monitor
│
├── Metrics
├── Logs
├── Alerts
├── Dashboards
├── Application Insights
├── Log Analytics
└── Workbooks
```

---

## Configuration

Portal

```
Azure Portal

↓

Azure Monitor

↓

Select Resource

↓

Enable Diagnostics

↓

Send Logs to

Log Analytics Workspace

↓

Create Alerts
```

---

## Benefits

- Centralized Monitoring
- Performance Tracking
- Resource Health
- Alerting
- Troubleshooting
- Root Cause Analysis

---

# 2. Log Analytics Workspace

## What is it?

A **Log Analytics Workspace** is a centralized repository where Azure Monitor stores and analyzes log data.

You query these logs using **Kusto Query Language (KQL).**

Think of it as the **database for Azure logs.**

---

Architecture

```
Azure Resources

↓

Diagnostic Settings

↓

Log Analytics Workspace

↓

KQL Queries
```

---

## What Logs are Stored?

- Azure Activity Logs
- VM Logs
- NSG Flow Logs
- Azure Firewall Logs
- Application Logs
- Performance Counters
- Security Logs

---

## Configuration

```
Create

↓

Log Analytics Workspace

↓

Open Resource

↓

Diagnostic Settings

↓

Send Logs

↓

Select Workspace
```

---

CLI

```bash
az monitor log-analytics workspace create
```

---

## Benefits

- Centralized Logging
- Long-Term Storage
- Fast Searching
- KQL Support
- Correlation Across Resources

---

# Azure Monitor vs Log Analytics

| Azure Monitor | Log Analytics Workspace |
|---------------|------------------------|
| Monitoring Service | Log Storage & Query Engine |
| Collects Data | Stores Logs |
| Metrics + Logs | Logs Only |
| Generates Alerts | Runs KQL Queries |

---

# 3. Application Insights (Basics)

## What is it?

**Application Insights** is an Azure Monitor feature used to monitor the **performance and availability of applications.**

It provides visibility into:

- Response Time
- Failed Requests
- Exceptions
- Dependencies
- User Sessions
- Performance Bottlenecks

---

Architecture

```
Web App

↓

Application Insights SDK

↓

Azure Monitor

↓

Dashboards
```

---

## What Does It Monitor?

- Request Rate
- Failed Requests
- Response Time
- Exceptions
- Availability Tests
- Dependencies
- User Behavior

---

Example

```
User Opens Website

↓

Application Insights

↓

Response Time

↓

800 ms
```

---

## Configuration

```
Create Application Insights

↓

Connect App

↓

Install SDK (or enable auto-instrumentation)

↓

View Live Metrics
```

---

## Benefits

- Detect Slow Applications
- Exception Tracking
- Performance Monitoring
- End-to-End Request Tracing
- Application Diagnostics

---

# Azure Monitor vs Application Insights

| Azure Monitor | Application Insights |
|---------------|----------------------|
| Infrastructure Monitoring | Application Monitoring |
| VMs, Storage, Networks | Web Apps, APIs |
| Metrics & Logs | Requests, Exceptions, Performance |

---

# 4. Metrics

## What are Metrics?

Metrics are **numerical values collected over time**.

Examples:

- CPU %
- Memory Usage
- Disk IOPS
- Network Throughput
- Request Count

---

Example

```
CPU Usage

10%

20%

45%

85%
```

---

Characteristics

- Lightweight
- Near Real-Time
- Time-Series Data
- Used for Alerts

---

Configuration

```
Azure Monitor

↓

Metrics

↓

Select Resource

↓

Choose Metric

↓

View Graph
```

---

## Benefits

- Real-Time Monitoring
- Trend Analysis
- Capacity Planning
- Alert Triggering

---

# 5. Alerts

## What are Alerts?

Alerts automatically notify administrators when a condition becomes true.

Example

```
CPU > 80%

↓

Alert Triggered

↓

Email

SMS

Webhook

ITSM
```

---

Types

### Metric Alerts

Based on metrics.

Example

```
CPU > 80%
```

---

### Log Alerts

Based on KQL queries.

Example

```
Failed Logins > 10
```

---

### Activity Log Alerts

Based on Azure Activity Logs.

Example

```
VM Deleted
```

---

## Configuration

```
Azure Monitor

↓

Alerts

↓

Create Alert Rule

↓

Choose Resource

↓

Condition

↓

Action Group

↓

Create
```

---

## Action Groups

Alerts can notify through:

- Email
- SMS
- Push Notification
- Webhook
- Azure Function
- Logic App

---

## Benefits

- Immediate Notifications
- Automatic Incident Detection
- Automation Integration

---

# Metrics vs Logs

| Metrics | Logs |
|----------|------|
| Numeric Data | Detailed Records |
| Near Real-Time | Rich Diagnostic Data |
| Lightweight | More Detailed |
| Used for Alerts | Used for Investigation |

---

# 6. KQL (Kusto Query Language)

## What is KQL?

**Kusto Query Language (KQL)** is the query language used to search and analyze data stored in Log Analytics Workspace.

Think of it as **SQL for Azure Logs**, but optimized for telemetry data.

---

Basic Syntax

```
TableName

|

Operator

|

Result
```

---

Example

```
AzureActivity

|

take 10
```

Returns first 10 records.

---

## Common Operators

### where

Filters data.

Example

```kql
AzureActivity
| where ResourceGroup == "Production"
```

---

### project

Select columns.

```kql
AzureActivity
| project TimeGenerated, OperationName, Caller
```

---

### summarize

Aggregate data.

```kql
AzureActivity
| summarize count() by ResourceGroup
```

---

### sort

Sort results.

```kql
AzureActivity
| sort by TimeGenerated desc
```

---

### take

Returns first records.

```kql
AzureActivity
| take 5
```

---

### top

Returns top values.

```kql
AzureActivity
| top 10 by TimeGenerated
```

---

### count

Count rows.

```kql
AzureActivity
| count
```

---

## Common Interview KQL Queries

### Last 10 Activity Logs

```kql
AzureActivity
| take 10
```

---

### Failed Requests

```kql
requests
| where success == false
```

---

### Exceptions

```kql
exceptions
| project TimeGenerated, type, outerMessage
```

---

### High CPU Performance

```kql
Perf
| where CounterName == "% Processor Time"
```

---

### Failed Sign-ins

```kql
SigninLogs
| where ResultType != 0
```

---

# 7. Root Cause Analysis (RCA) Using Logs

## What is Root Cause Analysis?

Root Cause Analysis (RCA) is the process of identifying the **actual reason** behind an issue instead of only fixing the symptom.

Example

```
Website Down

↓

Investigate Logs

↓

Database Timeout

↓

Storage Latency

↓

Root Cause Found
```

---

## Typical RCA Process

```
Issue Reported

↓

Check Azure Monitor

↓

Review Metrics

↓

Check Alerts

↓

Run KQL Queries

↓

Analyze Logs

↓

Identify Root Cause

↓

Fix Issue
```

---

## Example Scenario 1

Problem

```
Application Slow
```

Steps

```
Azure Monitor

↓

CPU Metrics

↓

VM CPU = 95%

↓

Problem Found
```

Root Cause

```
High CPU Usage
```

---

## Example Scenario 2

Problem

```
Website Returns 500 Error
```

Steps

```
Application Insights

↓

Exceptions

↓

SQL Timeout

↓

Database Issue
```

Root Cause

```
Database Connection Timeout
```

---

## Example Scenario 3

Problem

```
User Cannot Access VM
```

Steps

```
Sign-in Logs

↓

RBAC

↓

Activity Logs

↓

Permission Missing
```

Root Cause

```
Reader Role Assigned Instead of Contributor
```

---

## Example Scenario 4

Problem

```
Storage Upload Fails
```

Steps

```
Storage Logs

↓

403 Forbidden

↓

Expired SAS Token
```

Root Cause

```
Expired Authentication Token
```

---

# Common Monitoring Workflow

```
Azure Resource

↓

Azure Monitor

↓

Metrics

↓

Alerts

↓

Log Analytics

↓

KQL Query

↓

Root Cause Analysis
```

---

# Common Interview Questions

### What is Azure Monitor?

Azure Monitor is Azure's centralized monitoring service that collects metrics, logs, and diagnostic data from Azure resources and applications.

---

### What is Log Analytics Workspace?

A Log Analytics Workspace is the centralized repository where Azure Monitor stores logs and enables querying using KQL.

---

### What is Application Insights?

Application Insights is an Azure Monitor feature that monitors application performance, availability, requests, dependencies, and exceptions.

---

### Difference between Metrics and Logs?

| Metrics | Logs |
|----------|------|
| Numeric | Detailed Records |
| Lightweight | Rich Diagnostic Information |
| Real-Time | Historical Analysis |
| Monitoring | Troubleshooting |

---

### Difference between Azure Monitor and Application Insights?

| Azure Monitor | Application Insights |
|---------------|----------------------|
| Infrastructure Monitoring | Application Monitoring |
| VMs, Storage, Networking | Web Apps & APIs |
| Metrics + Logs | Requests, Exceptions, Dependencies |

---

### What is KQL?

Kusto Query Language (KQL) is the query language used to search, filter, summarize, and analyze data stored in Log Analytics Workspace.

---

### How do you investigate a high CPU alert?

1. Open Azure Monitor.
2. Check the CPU metric for the affected VM.
3. Review recent alerts.
4. Query performance logs in Log Analytics using KQL.
5. Check Activity Logs for recent configuration changes.
6. If it's an application issue, review Application Insights for slow requests or exceptions.
7. Identify the root cause and take corrective action.

---

### What is the difference between Activity Log and Diagnostic Logs?

| Activity Log | Diagnostic Logs |
|--------------|-----------------|
| Subscription-level operations | Resource-specific logs |
| Tracks create, update, delete actions | Tracks internal resource behavior |
| Available automatically | Must be enabled via Diagnostic Settings |

---

### Can Azure Monitor send notifications?

Yes. Azure Monitor Alerts can notify through:
- Email
- SMS
- Push Notifications
- Webhooks
- Azure Functions
- Logic Apps
- ITSM tools

---

# Easy Memory Trick

```
Azure Resources
       │
       ▼
Azure Monitor
│
├── Metrics
├── Logs
├── Alerts
├── Activity Log
├── Dashboards
│
├── Log Analytics Workspace
│     ├── KQL Queries
│     ├── Search Logs
│     └── Root Cause Analysis
│
├── Application Insights
│     ├── Requests
│     ├── Response Time
│     ├── Exceptions
│     └── Dependencies
│
└── Alerts
      ├── Metric Alerts
      ├── Log Alerts
      └── Activity Log Alerts
```


# Azure Operational / L2 Responsibilities Interview Notes

> **Interview Tip:** These topics are less about Azure services and more about **how an L2 Cloud Support Engineer works in production.** Interviewers want to know if you understand ITIL concepts, incident handling, RCA, SLAs, and operational support.

---

# What is L2 Support?

## What is it?

L2 (Level 2) Support is the **technical support team** responsible for investigating, troubleshooting, and resolving issues that L1 cannot solve.

L2 engineers have deeper technical knowledge and access to Azure resources.

Typical responsibilities:

- Investigate production incidents
- Analyze logs and monitoring data
- Troubleshoot Azure resources
- Perform Root Cause Analysis (RCA)
- Execute operational runbooks
- Support deployments
- Coordinate with L3 engineers if needed

---

## Support Hierarchy

```
Customer

↓

L1 Support
(Basic troubleshooting)

↓

L2 Support
(Technical Investigation)

↓

L3 Support
(Product Engineers / Developers)

↓

Microsoft Engineering
```

---

## Daily Responsibilities of an Azure L2 Engineer

- Monitor Azure resources
- Resolve alerts
- Investigate VM issues
- Troubleshoot networking
- Restore services
- Review Azure Monitor logs
- Validate deployments
- Create incident reports
- Perform RCA
- Maintain SLA

---

# 1. Incident Management

## What is an Incident?

An **Incident** is any unplanned interruption or degradation of an IT service.

Examples:

- VM not starting
- Website down
- High CPU
- Database unavailable
- VPN disconnected
- Storage inaccessible

Goal:

> **Restore the service as quickly as possible.**

Incident Management focuses on **restoration**, not necessarily finding the root cause immediately.

---

## Incident Lifecycle

```
Issue Reported

↓

Ticket Created

↓

Priority Assigned

↓

Investigation

↓

Troubleshooting

↓

Resolution

↓

Verification

↓

Ticket Closed
```

---

## Incident Priorities

| Priority | Example | Expected Response |
|-----------|----------|------------------|
| P1 (Critical) | Production Down | Immediate |
| P2 (High) | Major Service Impact | High Priority |
| P3 (Medium) | Partial Functionality Lost | Normal |
| P4 (Low) | Minor Issue / Request | Low Priority |

---

## L2 Responsibilities During an Incident

- Analyze alerts
- Check Azure Monitor
- Review Logs
- Verify Resource Health
- Restart services if required
- Escalate to L3 if necessary
- Update stakeholders
- Document actions taken

---

## Example

Problem

```
Website Down
```

L2 Investigation

```
Azure Monitor

↓

CPU = 95%

↓

App Service Restart

↓

Website Restored
```

Incident Closed.

Later, RCA is performed separately.

---

## Benefits

- Fast Service Recovery
- Reduced Downtime
- Improved Customer Satisfaction

---

# 2. Problem Management

## What is Problem Management?

Problem Management identifies the **root cause** of recurring incidents and prevents them from happening again.

Unlike Incident Management:

Incident Management = Restore Service

Problem Management = Remove Root Cause

---

## Problem Lifecycle

```
Recurring Incident

↓

Problem Identified

↓

Root Cause Analysis

↓

Permanent Fix

↓

Validation

↓

Problem Closed
```

---

## Example

Incident

```
VM CPU reaches 100%

Every Monday
```

Problem Investigation

```
Application

↓

Memory Leak

↓

Developer Fixes Code
```

Problem Solved.

---

## Root Cause Analysis (RCA)

RCA identifies:

- Why did it happen?
- How did it happen?
- How can it be prevented?

---

Example RCA

Issue

```
Website Slow
```

Investigation

```
Azure Monitor

↓

High CPU

↓

Application Logs

↓

Infinite Loop

↓

Developer Fix
```

Root Cause

```
Application Bug
```

---

## Trend Analysis

Trend Analysis looks for repeated patterns over time.

Example

```
Storage Failure

Monday

↓

Wednesday

↓

Friday
```

Trend:

```
Same Storage Account

↓

Capacity Issue
```

Benefits

- Predict future issues
- Prevent incidents
- Improve system reliability

---

# Incident vs Problem Management

| Incident Management | Problem Management |
|--------------------|--------------------|
| Restore Service | Find Root Cause |
| Immediate Action | Long-Term Solution |
| Focus on Downtime | Focus on Prevention |
| Short-Term Fix | Permanent Fix |

---

# 3. Change Validation & Deployment Support

## What is a Change?

A **Change** is any modification made to the production environment.

Examples:

- Deploy new application
- Update NSG rules
- Increase VM size
- Upgrade database
- Change Firewall rules
- Modify Storage configuration

---

## Change Lifecycle

```
Change Request

↓

Approval

↓

Validation

↓

Deployment

↓

Testing

↓

Monitoring

↓

Closure
```

---

## L2 Responsibilities

Before Deployment

- Verify prerequisites
- Validate resources
- Review change plan
- Confirm backup availability

---

During Deployment

- Monitor Azure Monitor
- Watch alerts
- Validate services
- Verify application health

---

After Deployment

- Functional testing
- Performance validation
- Check logs
- Close change if successful

---

Example

Deployment

```
Upgrade VM Size

↓

Restart VM

↓

Application Test

↓

CPU Improved

↓

Success
```

---

## Rollback

If deployment fails:

```
Deployment

↓

Failure

↓

Rollback

↓

Restore Previous State
```

Always have a rollback plan before production changes.

---

## Benefits

- Safe Deployments
- Reduced Downtime
- Controlled Production Changes

---

# 4. Runbook Execution & Improvement

## What is a Runbook?

A **Runbook** is a documented, step-by-step procedure for handling operational tasks or incidents.

Think of it as an SOP (Standard Operating Procedure).

---

Example Runbook

```
VM Down

↓

Check Azure Monitor

↓

Check Resource Health

↓

Restart VM

↓

Verify Services

↓

Update Ticket
```

---

Common Runbooks

- Restart VM
- Restart App Service
- Scale VM
- Restore Backup
- Rotate Keys
- Clear Disk Space
- Restart AKS Pods
- Renew Certificates

---

## L2 Responsibilities

- Execute existing runbooks
- Update documentation
- Improve runbooks
- Suggest automation
- Reduce manual effort

---

## Example

Old Runbook

```
20 Manual Steps
```

Improved Runbook

```
Azure Automation

↓

One Click

↓

Restart Services
```

---

Benefits

- Faster Resolution
- Standardized Process
- Fewer Human Errors
- Knowledge Sharing

---

# 5. SLA (Service Level Agreement)

## What is an SLA?

An **SLA (Service Level Agreement)** is the agreed target for service quality between the service provider and the customer.

It defines:

- Response Time
- Resolution Time
- Availability
- Uptime

---

Example

```
Critical Incident

Response

15 Minutes

Resolution

2 Hours
```

---

## Example SLA Table

| Priority | Response Time | Resolution Time |
|-----------|---------------|-----------------|
| P1 | 15 Minutes | 2 Hours |
| P2 | 30 Minutes | 4 Hours |
| P3 | 1 Hour | 8 Hours |
| P4 | 4 Hours | 24 Hours |

> **Note:** Actual SLA values vary between organizations.

---

## L2 Responsibilities

- Respond within SLA
- Resolve incidents within SLA
- Escalate before SLA breach
- Update tickets regularly
- Communicate with stakeholders

---

## SLA Breach

Example

```
Ticket

↓

No Response

↓

SLA Timer Expires

↓

SLA Breached
```

Consequences

- Customer dissatisfaction
- Escalation
- Management review
- Possible financial penalties (depending on contracts)

---

# Reporting

## What is Operational Reporting?

L2 teams regularly prepare reports on system health and support performance.

Common reports include:

- Incident Count
- Problem Count
- SLA Compliance
- Mean Time to Resolve (MTTR)
- Root Cause Trends
- System Availability
- Open vs Closed Tickets

---

Example Dashboard

```
Incidents

120

↓

Resolved

118

↓

Open

2

↓

SLA

99.8%
```

---

## Common KPIs

| KPI | Meaning |
|------|----------|
| SLA Compliance | % of tickets resolved within SLA |
| MTTR | Mean Time To Resolve |
| MTTA | Mean Time To Acknowledge |
| Incident Volume | Number of incidents |
| Problem Trends | Recurring issues over time |
| Availability | System uptime percentage |

---

# Real-Time L2 Incident Example

```
Alert Triggered

↓

Website Down

↓

Azure Monitor Alert

↓

Check VM Health

↓

CPU = 95%

↓

Application Logs

↓

Memory Leak Found

↓

Restart Service

↓

Website Restored

↓

Monitor for Stability

↓

Create RCA

↓

Recommend Code Fix

↓

Problem Closed
```

---

# Common Interview Questions

### What is the role of an L2 Support Engineer?

An L2 Support Engineer investigates technical issues that L1 cannot resolve, restores services, performs root cause analysis, validates changes, executes runbooks, and ensures SLA compliance.

---

### Difference between Incident and Problem Management?

| Incident | Problem |
|-----------|---------|
| Restore service quickly | Find and eliminate root cause |
| Short-term focus | Long-term prevention |
| May involve workaround | Results in permanent fix |

---

### What is RCA?

Root Cause Analysis (RCA) is the process of identifying the underlying cause of an issue so it can be permanently resolved and prevented from recurring.

---

### What is a Runbook?

A runbook is a documented set of step-by-step instructions used to perform operational tasks or resolve common incidents consistently.

---

### What is Change Validation?

Change validation ensures that a deployment or configuration change has been successfully implemented without negatively impacting production services.

---

### What would you do if a production VM suddenly becomes unavailable?

1. Acknowledge the incident.
2. Check Azure Monitor alerts and Resource Health.
3. Verify VM status and boot diagnostics.
4. Check networking (NSG, Load Balancer, VPN if applicable).
5. Review Activity Logs for recent changes.
6. Restart or recover the VM if appropriate.
7. Escalate to L3 if unresolved.
8. Restore service, document actions, and perform RCA.

---

### What is an SLA?

An SLA defines agreed service targets such as response time, resolution time, and availability. L2 engineers are responsible for meeting these commitments.

---

# Easy Memory Trick

```
L2 Operations
│
├── Incident Management
│     ├── Detect
│     ├── Investigate
│     ├── Restore
│     └── Close
│
├── Problem Management
│     ├── RCA
│     ├── Trend Analysis
│     └── Permanent Fix
│
├── Change Management
│     ├── Validate
│     ├── Deploy
│     ├── Test
│     └── Rollback
│
├── Runbooks
│     ├── Execute
│     ├── Improve
│     └── Automate
│
└── SLA & Reporting
      ├── Response Time
      ├── Resolution Time
      ├── MTTR
      ├── Availability
      └── Compliance
```

---

# ⭐ Interview Scenario (Very Common)

**Interviewer:** *"A production website is down. As an L2 Engineer, what will you do?"*

**Answer:**

1. Acknowledge the incident and determine its priority (P1/P2).
2. Check Azure Monitor for alerts and Resource Health.
3. Review VM/App Service status, CPU, memory, disk, and network metrics.
4. Analyze logs in Log Analytics and Application Insights to identify errors.
5. Check if any recent deployments or configuration changes were made using the Activity Log.
6. Restore the service (restart service, scale resources, or fail over if appropriate).
7. Continuously monitor to ensure the issue is resolved.
8. Document all actions, communicate updates to stakeholders, and prepare an RCA if required.
9. If the issue cannot be resolved at L2, escalate to L3 with all collected evidence (logs, metrics, timeline, and findings).

This structured approach demonstrates strong operational knowledge and aligns with standard ITIL-based L2 support practices.

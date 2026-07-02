# Azure Bastion – Interview Notes (Azure DevOps Engineer)

---

## Interview Question (Corrected)

**What is Azure Bastion and why is it used?**

---

# Interview Ready Answer

Azure Bastion is a **fully managed PaaS service in Azure** that provides **secure RDP and SSH access to virtual machines directly through the Azure portal over SSL (HTTPS)** without exposing a public IP on the VM.

It allows administrators to connect to VMs in a virtual network **without using public IP addresses, VPN, or exposing ports 22/3389 to the internet**.

---

# Architecture Overview

```
User (Browser)
      │ HTTPS (443)
      ▼
Azure Portal
      │
      ▼
Azure Bastion (in VNet)
      │ Private IP
      ▼
Virtual Machine (VM)
```

---

# Key Idea

> Bastion acts as a secure jump server managed by Azure.

---

# Why Azure Bastion is Used

Without Bastion:

```
Internet → Public IP → VM (RDP/SSH exposed ❌)
```

Problems:
- Security risk (open ports 22/3389)
- Brute force attacks
- IP whitelisting complexity

---

With Bastion:

```
Internet → Azure Bastion → Private VM (No public IP ❌)
```

Benefits:
- No public IP required
- No RDP/SSH exposed to internet
- Secure browser-based access
- TLS encrypted connection
- Centralized access control via Entra ID

---

# Key Features

## 1. Secure RDP/SSH via Browser
- Access VM directly from Azure portal
- No need for RDP client or SSH tools

---

## 2. No Public IP Required
- VM stays completely private
- Reduces attack surface

---

## 3. TLS Encrypted Connection
- All traffic goes through HTTPS (443)

---

## 4. Works with NSGs
- Only private subnet communication required
- No inbound internet rule needed for VM

---

## 5. Centralized Access Control
- Uses Microsoft Entra ID authentication
- Supports RBAC roles:
  - Reader
  - Virtual Machine Administrator Login
  - Virtual Machine User Login

---

# Types of Azure Bastion

## 1. Basic SKU
- Standard RDP/SSH
- Limited features
- No native client support

## 2. Standard SKU
- Native client support (SSH/RDP client)
- Shareable links
- IP-based restrictions
- More scaling options

---

# How It Works (Step-by-Step)

```
1. Admin logs into Azure Portal
2. Selects VM
3. Clicks "Connect using Bastion"
4. Browser opens secure session
5. Azure Bastion proxies connection
6. VM accessed privately
```

---

# Components

- Bastion Host (deployed in VNet)
- Public IP (for Bastion only, not VM)
- Private VM subnet
- NSG rules (allow Bastion subnet access)
- Entra ID authentication

---

# Network Requirements

Azure Bastion must be deployed in a subnet called:

```
AzureBastionSubnet
```

Minimum size:
```
/26 subnet or larger recommended
```

---

# NSG Rules Required

Allow:

```
Source: AzureBastionSubnet
Destination: VM subnet
Ports:
- 22 (SSH)
- 3389 (RDP)
```

No internet inbound rules required.

---

# Azure Bastion vs Jump Server

| Feature | Azure Bastion | Jump VM |
|----------|--------------|----------|
| Public IP required | ❌ No | ✅ Yes |
| Managed service | ✅ Yes | ❌ No |
| Patching required | ❌ No | ✅ Yes |
| Security | High | Medium/Low |
| Maintenance | None | High |
| Scaling | Automatic | Manual |

---

# Azure Bastion vs VPN

| Feature | Bastion | VPN |
|----------|--------|-----|
| Access type | VM access only | Full network access |
| Setup complexity | Low | Medium/High |
| Security | High | High |
| Client required | Browser | VPN client |
| Scope | Single VM access | Entire VNet access |

---

# Use Cases

- Secure admin access to VMs
- Production troubleshooting
- Dev/Test VM access
- No-public-IP architectures
- Compliance-driven environments

---

# Common Interview Questions

## Q1: Does Azure Bastion require public IP on VM?

**Answer:**
No. VMs do not need public IPs. Only Bastion has a public IP.

---

## Q2: Can Bastion be used for AKS nodes?

**Answer:**
Yes, indirectly. You can use Bastion to access VM nodes in AKS node pools for troubleshooting.

---

## Q3: Is Bastion region-specific?

**Answer:**
Yes. It is deployed per region inside a VNet.

---

## Q4: Is Bastion secure?

**Answer:**
Yes. It removes exposure of SSH/RDP ports and uses Entra ID + TLS encryption.

---

# Real Production Architecture

```
Internet
   │
Azure Portal
   │
Azure Bastion (Public IP)
   │
Private VNet
   │
VM / AKS Nodes / Backend servers
```

---

# One-Line Interview Answer

> **Azure Bastion is a fully managed Azure service that provides secure RDP and SSH access to virtual machines through a browser over SSL without requiring public IP addresses or exposing management ports to the internet.**

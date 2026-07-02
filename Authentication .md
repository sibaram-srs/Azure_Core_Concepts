# Managed Identity vs Service Principal vs App Registration vs Entra ID vs Service Connection (Azure DevOps)

These are commonly confused concepts in Azure + DevOps interviews. Let’s make them crystal clear.

---

# 1. Microsoft Entra ID (Azure AD)

## Interview Correct Question
**What is Microsoft Entra ID?**

## Interview Ready Answer

Microsoft Entra ID (formerly Azure Active Directory) is Microsoft’s **identity and access management (IAM) system** used to authenticate and authorize users, applications, and services in Azure and other Microsoft services.

It is the **identity backbone of Azure**.

---

## Key Functions

- User authentication (login)
- Application authentication
- Role-Based Access Control (RBAC)
- Conditional Access policies
- Multi-Factor Authentication (MFA)
- Identity governance

---

## Simple Analogy

> Entra ID = “Central identity database for Azure”

---

# 2. App Registration

## Interview Correct Question
**What is an App Registration in Entra ID?**

## Interview Ready Answer

An App Registration is a **definition of an application in Entra ID**. It tells Azure:

- This application exists
- It needs identity
- It can request tokens
- What permissions it requires

---

## What it creates

When you register an app, Azure creates:

- Application Object (global definition)
- Client ID (Application ID)

---

## Used for

- OAuth authentication
- API access
- Service-to-service communication

---

## Example

```
Frontend App → API App Registration → Access Token → Backend API
```

---

# 3. Service Principal

## Interview Correct Question
**What is a Service Principal?**

## Interview Ready Answer

A Service Principal is the **identity of an application in a specific tenant**.

While App Registration is the global definition, Service Principal is the **runtime identity used for authentication in a tenant**.

---

## Relationship

| Concept | Meaning |
|----------|--------|
| App Registration | Global app definition |
| Service Principal | Local instance of that app in a tenant |

---

## Simple Analogy

> App Registration = Passport  
> Service Principal = Visa in a country

---

## Used for

- CI/CD pipelines
- Azure resource access
- Automation scripts (Terraform, Azure CLI, etc.)

---

# 4. Managed Identity

## Interview Correct Question
**What is Managed Identity in Azure?**

## Interview Ready Answer

Managed Identity is an **Azure-managed Service Principal that eliminates the need to store credentials**.

Azure automatically creates and manages the identity.

---

## Types

### 1. System-Assigned Managed Identity
- Tied to a specific resource (VM, AKS, App Service)
- Deleted when resource is deleted

### 2. User-Assigned Managed Identity
- Independent resource
- Can be shared across multiple services

---

## Example Use Case

```
AKS Pod → Managed Identity → Azure Key Vault → Get Secrets
```

No passwords, no secrets stored in code.

---

## Key Benefit

> No credential management required (no client secrets, no certificates)

---

# 5. Service Principal vs Managed Identity

| Feature | Service Principal | Managed Identity |
|----------|------------------|------------------|
| Credentials | Yes (secret/cert) | No |
| Rotation | Manual | Automatic |
| Security | Medium risk | High security |
| Used in | CI/CD, external apps | Azure resources |
| Maintenance | Required | Zero maintenance |

---

# 6. Service Connection (Azure DevOps)

## Interview Correct Question
**What is a Service Connection in Azure DevOps?**

## Interview Ready Answer

A Service Connection is a **secure link between Azure DevOps and external services like Azure, AWS, Docker, or Kubernetes**.

It allows pipelines to deploy resources without exposing credentials.

---

## Example

```
Azure DevOps Pipeline
        │
        ▼
Service Connection
        │
        ▼
Azure Subscription / AKS / ACR
```

---

## Authentication Types

- Service Principal (most common)
- Managed Identity (recommended in some setups)
- Workload Identity Federation (modern approach, no secrets)

---

# 7. How They Work Together (Real Scenario)

## Example: CI/CD Deploy to AKS

```
Azure DevOps Pipeline
        │
        ▼
Service Connection (Service Principal or Workload Identity)
        │
        ▼
Azure Subscription (Entra ID Auth)
        │
        ▼
AKS / ACR / Key Vault
```

---

## Example: Runtime Access

```
AKS Pod
   │
   ▼
Managed Identity
   │
   ▼
Azure Key Vault / Storage / SQL
```

---

# 8. Key Differences Summary

| Concept | Purpose | Who Uses It |
|----------|--------|--------------|
| Entra ID | Identity system | Everything in Azure |
| App Registration | Define application | Developers |
| Service Principal | App identity in tenant | Azure services / pipelines |
| Managed Identity | Auto-managed identity | Azure resources |
| Service Connection | DevOps integration | Azure DevOps pipelines |

---

# 9. Common Interview Traps

### ❌ Wrong Answer
“Service Principal and App Registration are same”

### ✅ Correct Answer
They are related but not the same:
- App Registration = application definition
- Service Principal = runtime identity in tenant

---

### ❌ Wrong Answer
“Managed Identity uses secrets internally”

### ✅ Correct Answer
Managed Identity does NOT use secrets. Azure handles authentication automatically.

---

# 10. One-Line Interview Answers

- **Entra ID:** Identity and access management system for Azure.
- **App Registration:** Definition of an application in Entra ID.
- **Service Principal:** Identity of an app used for authentication in a tenant.
- **Managed Identity:** Auto-managed identity for Azure resources without credentials.
- **Service Connection:** Secure link between Azure DevOps and external systems.

---

If you want, I can next give you:
👉 real interview scenarios (end-to-end CI/CD using all 5 concepts)  
👉 or tricky interview questions on Managed Identity vs SPN vs Workload Identity Federation

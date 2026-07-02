# Authentication vs Authorization in Azure (Deep Interview Explanation)

---

# 1. Core Concept (Most Important)

## Authentication (AuthN)
**Answer:** “Who are you?”

It verifies the identity of a user, application, or service.

Example:
- Login with username + password
- MFA verification
- Token validation

---

## Authorization (AuthZ)
**Answer:** “What are you allowed to do?”

It checks permissions after identity is confirmed.

Example:
- Can you read a storage account?
- Can you deploy to AKS?
- Can you access Key Vault secrets?

---

# 2. Simple Real-World Analogy

| Concept | Real Life Example |
|----------|------------------|
| Authentication | Showing your ID card at gate |
| Authorization | Security checking which rooms you can enter |

---

# 3. How Azure Handles Authentication

Azure uses **Microsoft Entra ID** for authentication.

## Flow:

```
User / App
   │
   ▼
Entra ID Authentication
   │
   ├── Username/Password
   ├── MFA
   ├── Certificates
   ├── Tokens (OAuth2 / OpenID Connect)
   ▼
Access Token Issued (JWT)
```

---

## Output of Authentication:
- ID Token (who you are)
- Access Token (used for APIs)

---

# 4. How Azure Handles Authorization

Authorization happens **after authentication**.

Azure uses:

## 1. Azure RBAC (Role-Based Access Control)
Used for Azure resources.

Example roles:
- Owner
- Contributor
- Reader
- Key Vault Secrets User

---

## RBAC Flow:

```
Authenticated Identity (User/SPN/MI)
        │
        ▼
Azure Resource (Subscription / RG / VM / AKS)
        │
        ▼
RBAC Role Assignment Check
        │
        ▼
Allow / Deny
```

---

## 2. Azure Policy (Governance-level authorization)
- Enforces rules like:
  - Only allowed VM sizes
  - No public IP creation
  - Required tagging

---

## 3. Access Control at Data Level

| Service | Authorization Model |
|----------|------------------|
| Storage Account | RBAC + Access Keys + SAS |
| Key Vault | RBAC + Access Policies |
| SQL Database | SQL roles + Entra ID |
| AKS | Kubernetes RBAC |

---

# 5. Authentication in Azure DevOps / AKS / Pipelines

## Common identities used:

- User (interactive login)
- Service Principal
- Managed Identity
- Workload Identity Federation (modern approach)

---

## Example CI/CD Authentication

```
Azure DevOps Pipeline
        │
        ▼
Service Connection (SPN / MI)
        │
        ▼
Azure Resource Authentication via Entra ID
```

---

# 6. Authentication + Authorization Flow Together

## Example: Accessing Azure Key Vault

```
Step 1: Authentication
AKS Pod / User → Entra ID → Token issued

Step 2: Authorization
Token → Key Vault → RBAC check → Allow/Deny

Step 3: Access
Secret returned if allowed
```

---

# 7. JWT Token Structure (Important in Interviews)

Access tokens are **JWT (JSON Web Tokens)**:

```
Header.Payload.Signature
```

Payload contains:
- User identity
- Roles
- Permissions
- App ID
- Tenant ID

---

# 8. RBAC vs IAM vs Policies

| Concept | Purpose |
|----------|--------|
| Authentication | Verify identity |
| Authorization | Grant access |
| RBAC | Role-based permissions |
| Azure Policy | Governance rules |
| IAM | Combined identity + access management |

---

# 9. Common Azure Authorization Levels

```
Management Group
   ↓
Subscription
   ↓
Resource Group
   ↓
Resource (VM, AKS, Storage)
```

RBAC can be assigned at any level.

---

# 10. Example Scenarios

## Scenario 1: User accessing Azure Portal

1. Login → Entra ID authenticates user
2. Token issued
3. RBAC checks subscription access
4. Portal shows allowed resources

---

## Scenario 2: AKS Pod accessing Key Vault

1. Pod uses Managed Identity (Authentication)
2. Entra ID issues token
3. Key Vault checks RBAC (Authorization)
4. Secret returned

---

## Scenario 3: CI/CD pipeline deploying to AKS

1. Pipeline authenticates via Service Connection
2. Entra ID validates Service Principal
3. RBAC allows AKS deployment
4. kubectl apply succeeds

---

# 11. Key Differences Table

| Feature | Authentication | Authorization |
|----------|---------------|---------------|
| Purpose | Verify identity | Grant access |
| Question | Who are you? | What can you do? |
| Azure Service | Entra ID | RBAC / Policies |
| Output | Token | Allow / Deny |
| Happens first? | Yes | No |

---

# 12. Common Interview Mistakes

### ❌ Wrong:
“RBAC is authentication”

### ✅ Correct:
RBAC is **authorization**, not authentication.

---

### ❌ Wrong:
“Service Principal is used for authentication only”

### ✅ Correct:
Service Principal is used for **authentication AND authorization (via RBAC roles)**

---

# 13. One-Line Interview Answer

> **Authentication in Azure verifies identity using Microsoft Entra ID, while authorization determines access rights using RBAC, policies, and resource-level permissions after a valid token is issued.**

---

If you want next level prep, I can give:
👉 real interview scenario questions on Entra ID + RBAC  
👉 tricky DevOps pipeline authentication questions  
👉 managed identity vs service principal deep comparison  

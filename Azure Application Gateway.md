# Azure Application Gateway – Interview Notes (Azure DevOps Engineer)

## Interview Question

**What is Azure Application Gateway?**

### Interview Ready Answer

Azure Application Gateway is a **regional Layer 7 (Application Layer) load balancer** that distributes **HTTP/HTTPS** traffic to backend applications. It provides intelligent routing, SSL termination, Web Application Firewall (WAF), health monitoring, and load balancing for web applications.

Unlike Azure Load Balancer, which works at **Layer 4 (TCP/UDP)**, Application Gateway understands HTTP requests and can make routing decisions based on URLs, host headers, cookies, and other HTTP attributes.

---

# Where Does Application Gateway Sit?

```
                  Internet
                      │
              HTTPS Request
                      │
             Azure Application Gateway
                      │
        ┌─────────────┴─────────────┐
        │                           │
     AKS Cluster                App Service
        │
     Microservices
```

It acts as the entry point for web traffic.

---

# Key Features

## 1. Layer 7 Load Balancing

Routes traffic based on HTTP/HTTPS requests.

Example:

```
Client
   │
Application Gateway
   │
   ├── Server A
   ├── Server B
   └── Server C
```

Traffic is distributed across healthy backend servers.

---

## 2. SSL Offloading

Application Gateway terminates the HTTPS connection.

```
Client
    │ HTTPS
    ▼
Application Gateway
    │ HTTP
    ▼
AKS Pods
```

Benefits:

- Backend CPU is reduced.
- Centralized SSL certificate management.
- Faster TLS processing.

---

## 3. End-to-End SSL

If security policies require encryption throughout the path:

```
Client
    │ HTTPS
    ▼
Application Gateway
    │ HTTPS
    ▼
AKS
```

The gateway decrypts the client request, applies routing/WAF policies, and establishes a new HTTPS connection to the backend.

---

## 4. Web Application Firewall (WAF)

Protects applications against:

- SQL Injection
- Cross-Site Scripting (XSS)
- Remote Code Execution attacks
- OWASP Top 10 vulnerabilities
- Malicious bots
- IP restrictions
- Geo filtering
- Rate limiting (with WAF policies)

---

## 5. URL-Based Routing

Different URLs can be routed to different backend services.

Example:

```
example.com/api/*
        │
        ▼
AKS API Service

example.com/web/*
        │
        ▼
Frontend Service

example.com/admin/*
        │
        ▼
Admin Service
```

---

## 6. Host-Based Routing

Routes based on hostname.

Example:

```
api.company.com
      │
      ▼
API Backend

portal.company.com
      │
      ▼
Portal Backend
```

---

## 7. Health Probes

Application Gateway continuously checks backend health.

Example:

```
Backend 1  ✅

Backend 2  ❌

Backend 3  ✅
```

Traffic is automatically sent only to healthy backends.

---

## 8. Session Affinity (Cookie-Based Affinity)

Keeps a user's requests routed to the same backend server.

Useful for:

- Shopping carts
- Legacy applications
- Session-based authentication

---

## 9. Autoscaling

Application Gateway v2 automatically scales based on traffic.

No manual scaling is required.

---

## 10. Rewrite Rules

Can modify:

- Request headers
- Response headers
- URLs
- Host headers

Example:

```
/old-api
      │
Rewrite
      ▼
/v2/api
```

---

## 11. HTTP to HTTPS Redirection

Automatically redirects:

```
http://example.com

↓

https://example.com
```

---

## 12. Integration with AKS

Application Gateway integrates with AKS using:

- **Application Gateway Ingress Controller (AGIC)** (existing solution)
- **Application Gateway for Containers** (newer Azure-native option)

Ingress resources automatically configure routing rules on the Application Gateway.

---

# Typical Architecture

```
                  Internet
                      │
                 HTTPS (443)
                      │
           Azure Application Gateway
                      │
        ┌─────────────┴─────────────┐
        │                           │
   Frontend Service           API Service
        │                           │
        └─────────────┬─────────────┘
                      ▼
                  AKS Cluster
```

---

# Components of Application Gateway

- Frontend IP
- Listener
- SSL Certificate
- Backend Pool
- Backend HTTP Settings
- Health Probe
- Routing Rules
- Rewrite Rules
- WAF Policy

---

# Request Flow

```
User
 │
 ▼
DNS
 │
 ▼
Application Gateway
 │
 ▼
Listener
 │
 ▼
Routing Rule
 │
 ▼
Backend Pool
 │
 ▼
AKS Pods
```

---

# Application Gateway vs Azure Load Balancer

| Feature | Application Gateway | Azure Load Balancer |
|----------|---------------------|---------------------|
| Layer | Layer 7 | Layer 4 |
| Protocol | HTTP/HTTPS | TCP/UDP |
| SSL Termination | ✅ | ❌ |
| URL Routing | ✅ | ❌ |
| Host-Based Routing | ✅ | ❌ |
| WAF | ✅ | ❌ |
| Session Affinity | ✅ | Limited |
| Health Probes | HTTP/HTTPS | TCP/HTTP |

---

# Application Gateway vs Azure Front Door

| Feature | Application Gateway | Azure Front Door |
|----------|---------------------|------------------|
| Scope | Regional | Global |
| Layer | Layer 7 | Layer 7 |
| WAF | ✅ | ✅ |
| SSL Offloading | ✅ | ✅ |
| Global Load Balancing | ❌ | ✅ |
| VNet Integration | ✅ | No (directly) |
| Multi-region Routing | ❌ | ✅ |

---

# Real Production Architecture

```
                 Internet
                     │
             Azure Front Door
                     │
        ┌────────────┴────────────┐
        │                         │
Region A                    Region B
Application Gateway    Application Gateway
        │                         │
        ▼                         ▼
AKS Cluster                 AKS Cluster
```

- **Azure Front Door** decides which region receives the request.
- **Application Gateway** routes the request to the appropriate service within that region.

---

# Common Interview Questions

### Q1: Why use Application Gateway instead of Azure Load Balancer?

**Answer:**

Application Gateway operates at **Layer 7**, so it understands HTTP/HTTPS traffic and supports features like URL-based routing, host-based routing, SSL termination, and WAF. Azure Load Balancer works at **Layer 4** and only distributes TCP/UDP traffic.

---

### Q2: Can Application Gateway work with AKS?

**Answer:**

Yes. It integrates with AKS using **Application Gateway Ingress Controller (AGIC)** or the newer **Application Gateway for Containers**. Kubernetes Ingress resources automatically configure routing rules on the gateway.

---

### Q3: Can Application Gateway route traffic to multiple backend types?

**Answer:**

Yes. A backend pool can include:

- AKS
- Virtual Machines
- Virtual Machine Scale Sets
- Azure App Service
- Internal IP addresses
- External endpoints (if reachable)

---

### Q4: How does Application Gateway know a backend is unhealthy?

**Answer:**

It sends periodic **health probes** (HTTP/HTTPS) to a configured path, such as `/health`. If a backend fails consecutive health checks, it is marked unhealthy and removed from the load-balancing pool until it becomes healthy again.

---

# One-Line Interview Answer

> **"Azure Application Gateway is a regional Layer 7 load balancer that provides intelligent HTTP/HTTPS routing, SSL termination, Web Application Firewall (WAF), health probes, autoscaling, and seamless integration with AKS through an Ingress Controller."**

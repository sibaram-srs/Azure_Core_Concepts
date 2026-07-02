# Azure Front Door – Interview Notes (Azure DevOps Engineer)

## Interview Question

**What is Azure Front Door?**

### Interview Ready Answer

Azure Front Door is Microsoft's **global Layer 7 (HTTP/HTTPS) load balancer and application delivery service**. It routes user traffic to the nearest healthy backend using Microsoft's global edge network, reducing latency and improving application availability.

Unlike Azure Application Gateway, which works at the **regional VNet level**, Azure Front Door operates at the **global edge**, making it ideal for applications deployed across multiple Azure regions.

---

# Where Does Azure Front Door Sit?

```
                Internet Users
        India      US      Europe
             \      |      /
              \     |     /
          Azure Front Door (Global Edge)
                    │
      ┌─────────────┼─────────────┐
      │                           │
Region 1 (East US)          Region 2 (West Europe)
Application Gateway         Application Gateway
      │                           │
      ▼                           ▼
     AKS                         AKS
```

Front Door receives requests globally and sends them to the best backend.

---

# Key Features

### 1. Global Load Balancing

Routes users to the nearest healthy region.

Example:

- Indian users → Central India
- US users → East US
- European users → West Europe

---

### 2. Health Probes

Continuously checks backend health.

If a region becomes unavailable:

```
India User
     │
Front Door
     │
Central India ❌
     │
Automatically routes to
     ▼
South India ✅
```

No manual intervention is required.

---

### 3. SSL Termination

Front Door terminates HTTPS at Microsoft's edge.

Benefits:

- Reduced backend CPU usage
- Centralized certificate management
- Faster TLS handshakes

---

### 4. Web Application Firewall (WAF)

Provides protection against:

- SQL Injection
- Cross-Site Scripting (XSS)
- OWASP Top 10 attacks
- IP filtering
- Geo-filtering
- Rate limiting

---

### 5. URL Routing

Routes requests based on path.

Example:

```
/api/*
      ↓
AKS Backend

/images/*
      ↓
Azure Storage

/admin/*
      ↓
App Service
```

---

### 6. URL Rewrite & Redirect

Supports:

- HTTP → HTTPS redirect
- URL rewrites
- Custom redirects
- Header manipulation

---

### 7. Session Affinity

Can keep a user's requests routed to the same backend when required.

---

### 8. Caching

Static content can be cached at Microsoft's edge locations.

Example:

```
Images
CSS
JavaScript
Videos
```

This reduces latency and backend load.

---

### 9. DDoS Protection

Benefits from Microsoft's global network protections. For additional VNet-level protection, Azure DDoS Protection can be used with your regional resources.

---

# Front Door vs Application Gateway

| Feature | Azure Front Door | Azure Application Gateway |
|----------|------------------|---------------------------|
| Scope | Global | Regional |
| Layer | Layer 7 | Layer 7 |
| Global Load Balancing | ✅ | ❌ |
| WAF | ✅ | ✅ |
| SSL Offloading | ✅ | ✅ |
| Path-based Routing | ✅ | ✅ |
| Health Probes | ✅ | ✅ |
| CDN/Edge Caching | ✅ | Limited |
| Works Across Multiple Regions | ✅ | ❌ |
| VNet Integration | No (directly) | Yes |
| Backend | AKS, App Service, VM, Storage, Public endpoints | Resources within or reachable from the VNet |

---

# Typical Architecture

```
                 Internet
                     │
          Azure Front Door
                     │
      ┌──────────────┴──────────────┐
      │                             │
East US                      West Europe
Application Gateway      Application Gateway
      │                             │
      ▼                             ▼
AKS Cluster                    AKS Cluster
```

Front Door decides which region should serve the request.

Application Gateway performs regional routing to Kubernetes services.

---

# Difference Between Front Door and Traffic Manager

| Azure Front Door | Azure Traffic Manager |
|------------------|-----------------------|
| Layer 7 | DNS-based (Layer 4/7 independent) |
| Works with HTTP/HTTPS | Works with any protocol (DNS routing) |
| Instant failover after HTTP health checks | DNS failover depends on DNS TTL |
| SSL termination | No SSL termination |
| WAF support | No WAF |
| URL routing | No URL routing |
| Header inspection | No |

---

# Difference Between Front Door and Azure Load Balancer

| Azure Front Door | Azure Load Balancer |
|------------------|---------------------|
| Global | Regional |
| Layer 7 | Layer 4 |
| HTTP/HTTPS only | TCP/UDP |
| WAF | No |
| SSL termination | No |
| URL routing | No |

---

# Common Interview Scenario

### Q: When would you use Azure Front Door?

**Answer:**

I use Azure Front Door when:

- The application is deployed in multiple Azure regions.
- I need global load balancing.
- I need automatic regional failover.
- I want SSL termination at the edge.
- I need a global WAF.
- I want lower latency for users worldwide.
- I want edge caching for static content.

---

### Q: Can Front Door replace Application Gateway?

**Answer:**

Not always.

- **Front Door** is designed for **global traffic management**.
- **Application Gateway** is designed for **regional Layer 7 load balancing** and VNet integration.

In many production environments, they are used **together**:

```
Internet
     │
Azure Front Door
     │
Application Gateway
     │
AKS
```

Front Door selects the best region, and Application Gateway handles routing within that region.

---

# One-Line Interview Answer

> "Azure Front Door is Microsoft's global Layer 7 application delivery service that provides global load balancing, SSL termination, WAF, health-based failover, URL routing, and edge caching to deliver applications with low latency and high availability across multiple regions."

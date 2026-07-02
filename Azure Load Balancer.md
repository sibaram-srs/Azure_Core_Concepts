# Azure Load Balancer – Interview Notes (Azure DevOps Engineer)

## Interview Question

**What is Azure Load Balancer?**

---

# Interview Ready Answer

Azure Load Balancer is a **high-performance, Layer 4 (TCP/UDP) load balancing service** that distributes network traffic across multiple backend resources such as Virtual Machines or AKS nodes within a region.

It operates at the **transport layer**, meaning it does not understand HTTP/HTTPS, URLs, or application-level data. It only works with **IP address + port + protocol**.

---

# Where Does Azure Load Balancer Sit?

```
                Internet / Clients
                        │
                   TCP / UDP
                        │
              Azure Load Balancer
                        │
        ┌───────────────┴───────────────┐
        │                               │
     VM / VMSS                     VM / VMSS
     AKS Nodes                     AKS Nodes
```

---

# Key Features

## 1. Layer 4 Load Balancing

Distributes traffic based on:

- Source IP
- Destination IP
- Port
- Protocol (TCP/UDP)

Example:

```
Client → 10.0.0.1:80 → VM1 / VM2 / VM3
```

---

## 2. High Availability

If one backend instance fails:

```
VM1 ❌
VM2 ✅
VM3 ✅
```

Traffic automatically routes only to healthy instances.

---

## 3. Health Probes

Azure Load Balancer continuously checks backend health using:

- TCP probe
- HTTP probe

If a probe fails, that backend is removed from rotation.

---

## 4. No SSL Termination

Azure Load Balancer does NOT:

- Terminate SSL
- Inspect HTTP traffic
- Perform URL routing

It only forwards encrypted or unencrypted traffic as-is.

---

## 5. Static IP Support

Supports:

- Public Load Balancer (internet-facing)
- Internal Load Balancer (private VNet access)

---

## 6. Low Latency and High Throughput

Designed for:

- Millions of connections per second
- High-performance systems
- Minimal latency routing

---

## 7. Availability Zones Support

Can distribute traffic across zones:

```
Zone 1 VM
Zone 2 VM
Zone 3 VM
```

Ensures fault tolerance.

---

# Types of Azure Load Balancer

## 1. Public Load Balancer

- Exposes services to the internet
- Has public IP

Example:
```
www.company.com → Azure Load Balancer → backend VMs
```

---

## 2. Internal Load Balancer (ILB)

- Used inside a Virtual Network
- Private IP only

Example:

```
Frontend App → ILB → Backend APIs
```

---

# Request Flow

```
Client
 │
 ▼
Public IP
 │
Azure Load Balancer
 │
 ├── VM1
 ├── VM2
 └── VM3
```

---

# Azure Load Balancer vs Application Gateway

| Feature | Azure Load Balancer | Application Gateway |
|----------|---------------------|---------------------|
| Layer | Layer 4 | Layer 7 |
| Protocol | TCP/UDP | HTTP/HTTPS |
| SSL Termination | ❌ | ✅ |
| URL Routing | ❌ | ✅ |
| Host-Based Routing | ❌ | ✅ |
| WAF | ❌ | ✅ |
| Performance | Very High | High |
| Use Case | Non-HTTP traffic, APIs, databases | Web applications |

---

# Azure Load Balancer vs Azure Front Door

| Feature | Azure Load Balancer | Azure Front Door |
|----------|---------------------|------------------|
| Scope | Regional | Global |
| Layer | Layer 4 | Layer 7 |
| Traffic Type | TCP/UDP | HTTP/HTTPS |
| SSL Termination | ❌ | ✅ |
| WAF | ❌ | ✅ |
| Routing Intelligence | Basic | Advanced (URL, host, geo) |

---

# Common Use Cases

- AKS node-level load balancing
- VM-based applications
- Database clustering
- Internal microservices communication
- High-throughput TCP services (e.g., messaging systems)

---

# Azure Load Balancer in AKS

In AKS:

- Each Kubernetes Service of type `LoadBalancer` creates an Azure Load Balancer automatically.
- It exposes services externally.

Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
```

This provisions:

- Public IP
- Load Balancer rule
- Backend pool (AKS nodes)

---

# Components of Azure Load Balancer

- Frontend IP configuration
- Backend pool
- Load balancing rules
- Health probes
- Inbound NAT rules (optional)
- Outbound rules

---

# Health Probe Behavior

If probe fails:

```
VM1 ❌ removed
VM2 ✅ active
VM3 ✅ active
```

Traffic is automatically redistributed.

---

# Real-World Architecture

```
Internet
   │
Azure Load Balancer
   │
 ┌─┴──────────┐
 │            │
AKS Node 1  AKS Node 2
   │            │
Pods         Pods
```

---

# Important Points (Interview Tips)

- It is NOT application-aware
- It does NOT inspect HTTP requests
- It is extremely fast and lightweight
- Works at transport layer only
- Often used together with Application Gateway or Front Door

---

# One-Line Interview Answer

> **"Azure Load Balancer is a regional Layer 4 service that distributes TCP/UDP traffic across healthy backend resources using IP-based routing and health probes, providing high-performance and low-latency load balancing without application-level awareness."**

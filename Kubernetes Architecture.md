# Kubernetes Architecture (Interview Ready Explanation)

---

# Interview Question

**What is Kubernetes architecture?**

---

# Interview Ready Answer

Kubernetes is a **container orchestration platform** that automates deployment, scaling, and management of containerized applications.

Its architecture follows a **master-worker (control plane – data plane)** model.

---

# 1. High-Level Architecture

```
                CONTROL PLANE (Master Node)
        ┌──────────────────────────────────────┐
        │                                      │
        │  API Server                          │
        │  Scheduler                           │
        │  Controller Manager                  │
        │  etcd (Cluster DB)                  │
        │                                      │
        └──────────────┬───────────────────────┘
                       │
                       │ Kubernetes API Communication
                       │
        ┌──────────────┴───────────────────────┐
        │                                      │
        │            WORKER NODES              │
        │                                      │
        │  Kubelet                             │
        │  Kube Proxy                          │
        │  Container Runtime (Docker/containerd)│
        │                                      │
        │  Pods (Containers)                  │
        │                                      │
        └──────────────────────────────────────┘
```

---

# 2. Control Plane Components

## 1. API Server
- Entry point of Kubernetes
- All requests go through API server
- Exposes Kubernetes REST API

---

## 2. etcd
- Distributed key-value store
- Stores entire cluster state
- Source of truth for Kubernetes

---

## 3. Scheduler
- Assigns pods to worker nodes
- Decides placement based on:
  - CPU
  - Memory
  - Affinity rules
  - Taints & tolerations

---

## 4. Controller Manager
- Ensures desired state matches actual state
- Runs controllers like:
  - Node Controller
  - ReplicaSet Controller
  - Deployment Controller

---

# 3. Worker Node Components

## 1. Kubelet
- Agent running on each worker node
- Ensures containers are running as expected
- Communicates with API server

---

## 2. Kube Proxy
- Handles networking inside cluster
- Manages Service load balancing
- Routes traffic to correct pods

---

## 3. Container Runtime
- Runs containers
- Examples:
  - containerd
  - CRI-O

---

## 4. Pods
- Smallest deployable unit in Kubernetes
- Contains one or more containers
- Shares network + storage

---

# 4. Kubernetes Objects

## Pod
- Basic execution unit
- Contains containers

---

## Deployment
- Manages ReplicaSets
- Handles rolling updates

---

## ReplicaSet
- Ensures number of pods are always running

---

## Service
- Exposes pods internally or externally
- Types:
  - ClusterIP
  - NodePort
  - LoadBalancer

---

## Ingress
- Manages HTTP/HTTPS routing
- Works with Ingress Controller

---

# 5. Kubernetes Networking Flow

```
User Request
     │
     ▼
Service (Load Balancer / ClusterIP)
     │
     ▼
Kube Proxy
     │
     ▼
Pod (Container)
```

---

# 6. Kubernetes Deployment Flow

```
1. Developer pushes code
2. CI/CD builds Docker image
3. Image pushed to ACR
4. Deployment YAML applied
5. API Server receives request
6. Scheduler assigns node
7. Kubelet runs pod
8. Application becomes available
```

---

# 7. Key Kubernetes Concepts

## Desired State Management
You define:
```
I want 3 replicas
```

Kubernetes ensures:
```
Actual state = 3 running pods
```

---

## Self-Healing
- If pod crashes → new pod created automatically

---

## Auto Scaling
- HPA (Horizontal Pod Autoscaler)
- Cluster Autoscaler

---

## Rolling Updates
- Zero downtime deployment strategy

---

# 8. Kubernetes in Azure (AKS)

In Azure Kubernetes Service:

- Control plane is managed by Azure
- Worker nodes are your responsibility
- Integrated with:
  - Azure Monitor
  - ACR (Azure Container Registry)
  - Azure Key Vault
  - Load Balancer / Application Gateway

---

# 9. AKS Architecture

```
Azure Control Plane (Managed)
        │
        ▼
AKS Cluster
        │
 ┌──────┴────────┐
 │               │
Node Pool 1   Node Pool 2
 │               │
Pods           Pods
```

---

# 10. Common Interview Questions

---

## Q1: What is the difference between Pod and Container?

**Answer:**
A Pod is the smallest Kubernetes unit and can contain one or more containers that share network and storage.

---

## Q2: What happens if a Pod crashes?

**Answer:**
The ReplicaSet controller detects it and creates a new Pod automatically.

---

## Q3: What is etcd?

**Answer:**
etcd is the distributed key-value store that holds the entire cluster state.

---

## Q4: Why do we need kube-proxy?

**Answer:**
It manages networking rules and ensures traffic is routed to correct pods.

---

# 11. One-Line Interview Answer

> **Kubernetes architecture consists of a control plane (API server, scheduler, controller manager, etcd) and worker nodes (kubelet, kube-proxy, container runtime) that together manage containerized applications in a scalable, self-healing, and automated way.**

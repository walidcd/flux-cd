# 📘 Kubernetes Fundamentals — Core Concepts

## 1️⃣ Kubernetes Overview

Kubernetes is a container orchestration system that ensures applications run reliably and scale efficiently. It continuously compares the **desired state** (defined in YAML configuration) with the **actual state** (current runtime state) and automatically corrects differences. This automated correction process is called **reconciliation**.

---

## 2️⃣ Pods

### Definition

A **Pod** is the smallest deployable and manageable unit in Kubernetes. It encapsulates:

* One or more **containers**
* Shared **network namespace** (same IP and ports)
* Shared **storage volumes**
* Shared **lifecycle**

### Purpose

Pods allow containers that must work closely together to run as a single operational entity.

### Characteristics Table

| Feature         | Description                                                 |
| --------------- | ----------------------------------------------------------- |
| Scheduling unit | Kubernetes places Pods on nodes                             |
| Shared network  | Containers share Pod IP and port space                      |
| Shared storage  | Volumes are accessible by all containers in the Pod         |
| Atomic grouping | All containers are created, deleted, and restarted together |

### Real-world analogy

A Pod is like a **small virtual machine** hosting one or more processes that collaborate tightly.

---

## 3️⃣ Deployments

### Definition

A **Deployment** is a higher-level controller that manages Pods. It defines the **desired number of replicas**, update strategy, and rollback behavior.

### Responsibilities

| Responsibility                  | Benefit                               |
| ------------------------------- | ------------------------------------- |
| Maintain correct number of Pods | Self-healing                          |
| Rolling updates                 | Zero-downtime upgrades                |
| Rollbacks                       | Safe restoration                      |
| Scaling                         | Automatic or manual capacity increase |

### Architecture Relationship

```
Deployment → ReplicaSet → Pods → Containers
```

---

## 4️⃣ Services

### Definition

A **Service** provides a stable network identity and communication method for Pods.

Pods are ephemeral:
✅ They are recreated when they fail
❌ They do not guarantee stable IP addresses

Services solve this by offering:

* A **permanent virtual IP**
* **Load balancing** across Pods
* **DNS name** inside the cluster

### Service Types

| Type             | Purpose                        | Example Use            |
| ---------------- | ------------------------------ | ---------------------- |
| ClusterIP        | Internal only                  | Internal microservices |
| NodePort         | Expose on node ports           | For dev/debug          |
| LoadBalancer     | External exposure via cloud LB | Public APIs            |
| Headless Service | Pod-to-Pod direct routing      | Stateful apps (DBs)    |

---

## 5️⃣ Namespaces

### Definition

A **Namespace** is a logical partition in a Kubernetes cluster used to group and isolate resources.

### Purpose

* Multi-tenant or multi-team isolation
* Separate dev/staging/prod environments
* RBAC (access control) boundaries

### Key Namespaces (default)

| Namespace       | Usage                          |
| --------------- | ------------------------------ |
| default         | User workloads                 |
| kube-system     | Kubernetes internal components |
| kube-public     | Publicly readable resources    |
| kube-node-lease | Node heartbeat tracking        |

---

## 6️⃣ Configuration + Secrets

| Resource      | Purpose                                 | Security Level                      |
| ------------- | --------------------------------------- | ----------------------------------- |
| **ConfigMap** | Store non-sensitive configuration       | 🔓 Plain text                       |
| **Secret**    | Store sensitive data (tokens/passwords) | 🔐 Base64-encoded, can be encrypted |

---

## 7️⃣ Summary Diagram

```
┌──────────────────────────────────────────┐
│                  Cluster                 │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │              Namespace             │  │
│  │                                    │  │
│  │  Deployment                        │  │
│  │     │                              │  │
│  │     ▼                              │  │
│  │  ReplicaSet                        │  │
│  │     │                              │  │
│  │     ▼                              │  │
│  │   Pods (share network/storage)     │  │
│  │      └─ Containers                 │  │
│  │                                    │  │
│  └────────────────────────────────────┘  │
│                                          │
│  Services provide stable access to Pods  │
└──────────────────────────────────────────┘
```
Excellent — continuing in the same clean report style ✅
Here is the requested addition + Kubernetes Theory Part 2.

---
## 8️⃣ Nodes

### Definition

A **Node** is a physical or virtual machine in the Kubernetes cluster that runs Pods. Nodes provide compute resources: CPU, memory, network, and storage. Each node contains the necessary Kubernetes agents (like the **kubelet**) to interact with the control plane.

### Characteristics

| Component         | Purpose                                          |
| ----------------- | ------------------------------------------------ |
| kubelet           | Ensures containers inside Pods are running       |
| container runtime | Executes containers (containerd, CRI-O, Docker*) |
| kube-proxy        | Handles cluster network rules and routing        |

(*Docker itself is no longer the runtime, but Docker Desktop includes a compatible runtime under the hood.)

### Node Role Distinction

* **Control Plane Node(s)** → cluster brain: scheduling, API, orchestration
* **Worker Node(s)** → run workloads (user applications)

---

## 9️⃣ Cluster

### Definition

A **Cluster** is the complete set of nodes managed together by Kubernetes. It includes the control plane and multiple worker nodes, operating as a single logical system.

### Purpose

* High availability
* Horizontal scale-out
* Unified management for multiple applications

### Architecture View

```
Cluster
 ├─ Control Plane (managers)
 └─ Worker Nodes (run apps)
     └─ Pods → Containers
```

The control plane stores and maintains the **source of truth** for the cluster state, primarily inside **etcd** (a distributed key-value store).

---

## 🔟 ReplicaSet

### Definition

A **ReplicaSet** ensures a specified number of identical Pods are always running.

### Purpose

* Maintain the exact count of Pod replicas
* Automatically replace failed Pods

### Relationship with Deployments

Users normally do **not** create ReplicaSets manually. They are created and controlled by **Deployments**.

Hierarchy:

```
Deployment → ReplicaSet → Pods
```

ReplicaSets are responsible for low-level Pod lifecycle; Deployments provide rollout and rollback capabilities.

---

✅ These 3 components complete the **execution model** of Kubernetes:
**Cluster → Nodes → Pods → Containers**

# 📘 Kubernetes Fundamentals — Part 2 (Networking, Workloads, Scheduling)

## 1️⃣ Ingress

### Definition

An **Ingress** exposes HTTP/HTTPS routes from outside the cluster to internal Services.

### Purpose

* URL routing to specific Services
* TLS termination (HTTPS)
* Reverse-proxy / load balancing

Ingress requires an **Ingress Controller** (nginx, Traefik, Istio, etc.)

---

## 2️⃣ Labels & Selectors

### Definition

Labels are **key-value metadata** attached to resources.
Selectors allow objects to **find** each other using labels.

### Example Table

| Object                         | Uses labels for              |
| ------------------------------ | ---------------------------- |
| Deployment → ReplicaSet → Pods | Selecting which Pods belong  |
| Service                        | Load-balancing specific Pods |
| Ingress                        | Routing rules                |
| RBAC                           | Partitioning                 |

📌 Labels are the **glue** connecting Kubernetes components.

---

## 3️⃣ Probes (Health Checks)

| Probe               | Checks                           | Example            |
| ------------------- | -------------------------------- | ------------------ |
| **Liveness Probe**  | If container should be restarted | App stuck? Restart |
| **Readiness Probe** | If Pod can receive traffic       | Delay until ready  |
| **Startup Probe**   | Slow startup prevention          | JVM initialization |

These ensure reliable rollouts and self-healing behavior.

---

## 4️⃣ Scheduling (How Kubernetes Places Pods)

Scheduling decisions based on:

* Node resource availability (CPU/memory)
* Pod resource requests/limits
* Taints & tolerations (node restrictions)
* Affinity/anti-affinity (co-location rules)

The **Scheduler** picks a worker node; **kubelet** runs the Pod.

---

## 5️⃣ Controllers & Operators

### Controller

Software that continuously reconciles the cluster to match the **desired state**. Example: Deployment Controller.

### Operator

A specialized controller that encodes human operational knowledge, often for stateful services (databases). Example: PostgreSQL Operator.

> Flux operates as Controllers — ensuring Git = desired state.

---

## 6️⃣ Control Plane Components (High-Level)

| Component          | Function                           |
| ------------------ | ---------------------------------- |
| API Server         | Gateway for all cluster operations |
| etcd               | Stores cluster state               |
| Scheduler          | Places Pods on nodes               |
| Controller Manager | Runs reconciliation loops          |
| Cloud Controller   | Cloud integration                  |

These maintain the authority of the cluster.

---

# ✅ Combined Architecture Diagram

```
             ┌─────────────────────────────┐
             │         CONTROL PLANE        │
             │ API Server | Scheduler       │
             │ Controller Managers | etcd   │
             └───────────────┬─────────────┘
                             │
        ┌────────────────────┴────────────────────┐
        │                                         │
┌───────▼───────┐                        ┌────────▼───────┐
│   Worker Node │                        │   Worker Node  │
│ kubelet       │                        │ kubelet        │
│ kube-proxy    │                        │ kube-proxy     │
│               │                        │                │
│  Pods         │                        │  Pods          │
│ Containers    │                        │ Containers     │
└───────────────┘                        └────────────────┘

Ingress → Services → Deployments → ReplicaSets → Pods → Containers
```

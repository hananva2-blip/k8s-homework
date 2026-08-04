# Kubernetes Core Resources & RBAC Homework

This repository contains hands-on solutions for Kubernetes fundamentals including Namespaces, Deployments, Services, Ingress, ConfigMaps/Secrets, and RBAC isolation.

## 📄 Repository Structure
- `namespace.yaml` - Logical isolation setup
- `pod.yaml` - Bare pod specification
- `deployment.yaml` - Desired state deployment
- `services.yaml` - ClusterIP, NodePort & LoadBalancer configurations
- `ingress.yaml` - HTTP Routing rules
- `config-secret.yaml` - Decoupled configs & encoded secrets
- `rbac.yaml` - ServiceAccount, Role & RoleBinding isolation
- `full-deploy.yaml` - Complete combined deployment (Bonus)

## ❓ Questions & Answers Summary

### Part 1 - Namespace
* **What is a namespace?** A virtual cluster inside a physical cluster that provides logical isolation for resources.
* **Why logical separation?** Because all namespaces share the underlying physical hardware (CPU, Memory, Nodes) of the cluster.

### Part 2 - Pod
* **What happens if you delete a bare Pod?** It is deleted permanently and not recreated, as it lacks a managing Controller.

### Part 3 & 4 - Deployment & ReplicaSet
* **Which object ensures the number of Pods?** ReplicaSet.
* **Why not manage Pods directly?** Bare Pods lack self-healing, simple scaling, and rolling updates.
* **Why create a new ReplicaSet on update?** To execute zero-downtime Rolling Updates and maintain revision history for rollbacks.

### Part 5 - Services
* **Which Service is internal only?** `ClusterIP`.
* **Which Service is best for production?** `LoadBalancer` (integrated with an Ingress Controller).

### Part 6 - Ingress
* **Does Ingress work without a Controller?** No, the Ingress resource is only a rule definition; a Controller is required to route traffic.
* **Why not expose every Service directly?** Direct exposure via individual LoadBalancers is expensive, insecure, and harder to manage compared to a single Ingress entrypoint.

### Part 7 - ConfigMap & Secret
* **Why separate config from images?** Adheres to 12-Factor App methodology, allowing the same container image to be reused across Dev/Prod environments.
* **Why protect Secrets with RBAC?** Secrets are base64-encoded, not encrypted. Anyone with read access can decode them easily.

### Part 8 - RBAC
* **Why namespace-scoped RBAC?** Prevents security breaches in one environment (e.g., dev) from accessing resources in another (e.g., prod).
* **What security principle does RBAC enforce?** Principle of Least Privilege.

### Part 9 - Production Thinking
* **What changes between dev and prod?** Strict resource limits/requests, high availability (multi-replica/anti-affinity), robust probes, and strict RBAC isolation.
* **Why are limits mandatory?** Prevents "noisy neighbor" issues where a buggy pod consumes all node memory/CPU and crashes other critical workloads.

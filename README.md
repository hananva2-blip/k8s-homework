# ☸️ Kubernetes Core Resources & RBAC Isolation

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326CE5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![NGINX](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

Hands-on implementation of core Kubernetes resources, networking primitives, secret management, and RBAC isolation inside a dedicated namespace.

---

## 📐 Cluster Architecture

```mermaid
graph TD
    Client[Client / Browser] -->|HTTP Request: demo.local| Ingress[Ingress Controller]
    
    subgraph Namespace: dev
        Ingress -->|Routes to| SVC[Service: app-service / ClusterIP]
        
        SVC -->|Selector: app=demo| Pod1[Pod 1: nginx]
        SVC -->|Selector: app=demo| Pod2[Pod 2: nginx]
        SVC -->|Selector: app=demo| Pod3[Pod 3: nginx]
        
        Deployment[Deployment: app-deployment] -.->|Manages Desired State| ReplicaSet[ReplicaSet]
        ReplicaSet -.->|Enforces Replicas| Pod1
        ReplicaSet -.->|Enforces Replicas| Pod2
        ReplicaSet -.->|Enforces Replicas| Pod3
        
        ConfigMap[(ConfigMap: app-config)] -.->|Environment Vars| Pod1
        Secret[(Secret: app-secret)] -.->|Decoded Env Vars| Pod1
        SA[ServiceAccount: app-sa] -.->|RBAC Identity| Pod1
        
        Role[Role: pod-reader] --- RoleBinding[RoleBinding: pod-reader-binding]
        RoleBinding --- SA
    end
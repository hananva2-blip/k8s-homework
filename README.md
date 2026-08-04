# ☸️ Kubernetes Infrastructure & Core Resources

![Kubernetes CI & Security Scan](https://github.com/hananva2-blip/k8s-homework/actions/workflows/ci.yaml/badge.svg)
![Kubernetes Version](https://img.shields.io/badge/kubernetes-v1.30+-blue?logo=kubernetes)
![Security Scan](https://img.shields.io/badge/Security--Scan-Trivy-brightgreen?logo=aquasecurity)

Welcome to the Kubernetes deployment project! This repository contains production-ready Kubernetes manifests designed with security, scalability, and best practices in mind.

---

## 📐 Architecture Overview

Below is the dynamic visual flow of traffic and resource architecture defined in this repository:

```mermaid
graph TD
    Client([🌐 Client / External Traffic]) -->|HTTP Port 80| Ingress[🔀 Ingress / Host: demo.local]
    Ingress -->|Routes to| SVC[🔌 Service: app-service / Port 80]
    
    subgraph K8s Namespace: dev
        SVC -->|Endpoints| Pod1[📦 Pod: app-deployment-1]
        SVC -->|Endpoints| Pod2[📦 Pod: app-deployment-2]
        
        subgraph Config & Secrets
            CM[📄 ConfigMap: app-config] -.->|ENV| Pod1
            CM -.->|ENV| Pod2
            Sec[🔐 Secret: app-secret] -.->|PASSWORD| Pod1
            Sec -.->|PASSWORD| Pod2
        end
      
        subgraph RBAC Security
            SA[👤 ServiceAccount: dev-app-sa]
            Role[🛡️ Role: pod-reader]
            RB[🔗 RoleBinding: read-pods-global]
            Role --- RB --- SA
        end
    end

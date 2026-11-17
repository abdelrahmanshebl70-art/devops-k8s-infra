#  Advanced Kubernetes Microservices Infrastructure

A production-grade Kubernetes project demonstrating advanced cluster management, workload scheduling, high availability, security controls, and resource optimization.

---

##  Architecture & Components Overview

This project implements a complete microservices and database infrastructure setup on Kubernetes, leveraging best practices for Day-2 operations:

* **Workloads & Controllers:**
  * **Deployments:** Manages stateless application tiers with rolling update strategies (`maxSurge`, `maxUnavailable`) and resource requests/limits.
  * **StatefulSets:** Manages stateful components (like PostgreSQL) backed by headless services and persistent volume template claims.
* **Scheduling & Placement (Advanced):**
  * **Taints & Tolerations:** Configured node taints (`key: pro, value: env, effect: NoSchedule`) and matching pod tolerations to ensure dedicated scheduling on specific nodes.
* **Scaling & Autoscaling:**
  * **HPA (Horizontal Pod Autoscaler):** Automatically scales pod replicas based on CPU utilization metrics.
  * **VPA (Vertical Pod Autoscaler):** Manages container resource recommendations and sizing.
* **Networking & Ingress:**
  * **Services:** Utilizes `NodePort` and `Headless` services for internal and external cluster connectivity.
  * **Ingress Controller:** Advanced path routing, regex matching, and rewrite targeting (`nginx.ingress.kubernetes.io/rewrite-target`) for multiple backend services (`shibl.com`, `dashboard.com`).
* **Storage & Configuration:**
  * **PV & PVC:** Persistent Volume and Persistent Volume Claim setup using `hostPath` and local storage classes.
  * **ConfigMaps & Secrets:** Centralized configuration management and secure credential handling (`POSTGRES_USER`, `POSTGRES_PASSWORD`).
* **Security & Reliability:**
  * **RBAC:** Custom `Role` and `RoleBinding` bound to a dedicated `ServiceAccount` following the principle of least privilege.
  * **Probes & Lifecycle:** Comprehensive `startupProbe`, `livenessProbe`, `readinessProbe`, and `preStop` lifecycle hooks (`sleep` / graceful shutdown scripts) to ensure high availability and prevent downtime.

---

##  Project Structure

```text
├── deployment.yml         # Stateless application deployment (with probes, tolerations & resources)
├── deployment2.yml        # Secondary application deployment tier
├── statefulset.yml        # Stateful database tier (PostgreSQL with persistent storage & tolerations)
├── hpa.yml                # Horizontal Pod Autoscaler configuration
├── vpa.yml                # Vertical Pod Autoscaler configuration
├── ingress.yml            # Ingress routing rules & path rewrites
├── svc1.yml / svc2.yml    # NodePort services
├── headless.yml           # Headless service for database discovery
├── pv.yml / pvc.yml       # Persistent Volume & Claim configurations
├── config.yml / config2.yml # Application and Nginx configuration maps
├── secret.yml             # Sensitive environment variables & credentials
├── serviceaccount.yml     # Dedicated service account
├── role.yml               # RBAC role definitions
└── rolebind.yml           # RBAC role binding

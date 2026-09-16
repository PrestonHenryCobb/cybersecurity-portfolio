# CKAD Labs

Hands-on lab writeups from my CKAD (Certified Kubernetes Application Developer) prep, using KodeKloud labs as the practice environment. Folder order follows Mumshad Mareedu's CKAD course on Udemy, which itself mirrors the official CKAD exam curriculum.

Studying toward CKAD → CKA → RHCSA, en route to a Kubernetes/cloud administration role.

## 📚 Labs (in course order)

| # | Section | Status | Notes |
|---|---|---|---|
| 1 | [Core Concepts](./01-core-concepts) | ✅ In progress | Pods, ReplicaSets, Deployments, Namespaces |
| 2 | [Multi-Container Pods](./02-multi-container-pods) | ⬜ Not started | Sidecar, Ambassador, Adapter patterns |
| 3 | [Observability](./03-observability) | ⬜ Not started | Liveness/Readiness/Startup probes, logging, debugging |
| 4 | [Configuration](./04-configuration) | ⬜ Not started | ConfigMaps, Secrets, SecurityContexts, Resource Requirements, ServiceAccounts |
| 5 | [Pod Design](./05-pod-design) | ⬜ Not started | Labels/Selectors, rolling updates, Jobs & CronJobs |
| 6 | [Services & Networking](./06-services-and-networking) | ⬜ Not started | ClusterIP, NodePort, Ingress, NetworkPolicy |
| 7 | [State Persistence](./07-state-persistence) | ⬜ Not started | Volumes, PV/PVC |

More labs get added within each section as I progress through the course.

## Why this repo

Each lab is written up (and sometimes recorded as a short video walkthrough) to reinforce what I learned and give a concrete, hands-on view of my Kubernetes skills beyond just holding the cert.

## Structure

Each section folder contains:
- `README.md` — the scenario, what I did, what tripped me up, takeaways
- `manifests/` — the actual YAML files used
- `screenshots/` — terminal output / diagrams, where relevant

Section order and weighting matches the official CKAD curriculum:
Core Concepts (13%) → Multi-Container Pods (10%) → Observability (18%) → Configuration (18%) → Pod Design (20%) → Services & Networking (13%) → State Persistence (8%)

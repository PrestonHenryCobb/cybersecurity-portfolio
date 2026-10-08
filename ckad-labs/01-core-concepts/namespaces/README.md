# CKAD Lab: Core Concepts — Namespaces

Part of hands-on CKAD prep (KodeKloud labs). Maps to course section: Core Concepts (Pods, ReplicaSets, Deployments, Namespaces)

## Problem

Explore a cluster with several pre-created Namespaces: count the Namespaces, find Pods inside a specific one, create a Pod in a chosen Namespace, locate a Pod by name across the whole cluster, and work out which DNS name an application should use to reach a Service in its own Namespace versus a different one.

Tasks:
- Count the Namespaces on the system.
- Count the Pods in the `research` Namespace.
- Create a Pod named `redis` (image `redis`) in the `finance` Namespace.
- Find which Namespace contains the `blue` Pod.
- Determine the shortest DNS name the `blue` application should use to reach `redis-db-service` in its own Namespace (`marketing`), port 6379.
- Determine the DNS name it should use to reach `redis-db-service` in the `dev` Namespace.

## Solution

`k` is the shell alias for `kubectl` in the lab terminal.

### 1. Count the Namespaces

```bash
kubectl get ns
```

```
NAME              STATUS   AGE
default           Active   16m
dev               Active   3m34s
finance           Active   3m34s
kube-node-lease   Active   16m
kube-public       Active   16m
kube-system       Active   16m
manufacturing     Active   3m34s
marketing         Active   3m34s
prod              Active   3m34s
research          Active   3m34s
```

10 Namespaces: 4 system/default (`default`, `kube-node-lease`, `kube-public`, `kube-system`) and 6 created for the lab.

### 2. Pods in the `research` Namespace

```bash
kubectl get pods -n research
```

```
NAME    READY   STATUS             RESTARTS      AGE
dna-1   0/1     CrashLoopBackOff   6 (2m29s ago) 8m4s
dna-2   0/1     CrashLoopBackOff   6 (2m18s ago) 8m4s
```

2 Pods (`dna-1`, `dna-2`). Both are crash-looping; the lab only asked for the count, not a fix.

### 3. Create the `redis` Pod in `finance`

```bash
kubectl run redis --image=redis -n finance
kubectl get pods -n finance
```

```
NAME      READY   STATUS    RESTARTS   AGE
payroll   1/1     Running   0          11m
redis     1/1     Running   0          53s
```

The declarative equivalent is in `manifests/redis-pod.yaml`. The Pod must be created with `-n finance` (or `metadata.namespace: finance`); otherwise it lands in `default`.

### 4. Find the Namespace of the `blue` Pod

```bash
kubectl get pods --all-namespaces
kubectl get pods --all-namespaces | grep blue
```

```
marketing   blue   1/1   Running   0   19m
```

`blue` is in the `marketing` Namespace. Piping to `grep` cuts the full listing down to the one row needed. `-A` is the short form of `--all-namespaces`.

### 5. Access the Blue web application

Done in the lab's web UI (a separate browser tab), which can ping other Services from inside the `marketing` Namespace.

### 6. DNS name for `redis-db-service` in the same Namespace (`marketing`)

Answer: **`redis-db-service`**

When a Pod and a Service are in the same Namespace, the plain Service name is enough. Cluster DNS resolves it within the caller's own Namespace first.

### 7. DNS name for `redis-db-service` in the `dev` Namespace

Answer: **`redis-db-service.dev.svc.cluster.local`**

For a Service in another Namespace, the Namespace must be part of the name. Using just `redis-db-service` would resolve to the `marketing` copy of the Service, not the `dev` one. See `screenshots/namespace-dns-resolution.svg`.

The fully-qualified form is `<service>.<namespace>.svc.<cluster-domain>`. The shorter `redis-db-service.dev` also resolves from inside the cluster.

## Outcome

- 10 Namespaces, 2 Pods in `research`, `redis` running in `finance`, `blue` located in `marketing`.
- Same-Namespace Service access uses the bare Service name; cross-Namespace access needs `<service>.<namespace>` (or the full `.svc.cluster.local` form).
- The same Service name can exist in several Namespaces without conflict (`redis-db-service` in both `marketing` and `dev`), which is the point of Namespaces: name scoping.

## Commands I Got Wrong

- `kubectl get pods -n all-namespaces` — `-n` takes a Namespace *name*, so kubectl looked for a Namespace literally called `all-namespaces` and returned `No resources found in all-namespaces namespace.` Correct: `kubectl get pods --all-namespaces` (or `-A`).
- `kubectl get pods -n -- all-namespaces` — tried to fix the first attempt by adding `--`, which made kubectl treat `--` as the Namespace name: `Error from server (NotFound): namespaces "--" not found`. The flag is one token: `--all-namespaces`.

## Files

- `manifests/redis-pod.yaml` — declarative equivalent of the `redis` Pod created imperatively in task 3
- `screenshots/namespace-dns-resolution.svg` — diagram of how the `blue` Pod resolves `redis-db-service` in `marketing` versus `dev` (drawn for this writeup, not a terminal capture)

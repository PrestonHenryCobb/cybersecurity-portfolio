# CKAD Lab: Core Concepts — ReplicaSets

Part of hands-on CKAD prep (KodeKloud labs). Maps to course section: Core Concepts (Pods, ReplicaSets, Deployments, Namespaces)

## Problem

Starting state investigation in the default namespace: find the existing Pods and ReplicaSets, how many replicas the ReplicaSet is configured for, which image its containers use, and why none of the Pods are ready.

A pre-existing ReplicaSet (`new-replica-set`) had 4 desired replicas and 0 ready. The lab then required:

- Deleting one of its Pods and observing how the ReplicaSet responds.
- Creating a ReplicaSet from `replicaset-definition-1.yaml`, which contained an error that blocked creation.
- Creating a ReplicaSet from `replicaset-definition-2.yaml`, which contained a different error.
- Deleting `replicaset-1` and `replicaset-2`.
- Fixing `new-replica-set` so its Pods use a valid image.

## Solution

### 1. Inspect the existing ReplicaSet and Pods

```bash
kubectl get pods
kubectl get rs
kubectl describe rs new-replica-set
kubectl get pods
```

`describe` showed selector `name=busybox-pod`, 4 desired / 4 current replicas, 0 Running / 4 Waiting, and container `busybox-container` using image `busybox777`. The Events section showed four `SuccessfulCreate` events from `replicaset-controller`, so the controller did its job. `kubectl get pods` showed all four Pods in `ImagePullBackOff`: the fault was the image name in the Pod template, not the ReplicaSet.

### 2. Delete a Pod and observe the replacement

```bash
kubectl delete pod new-replica-set-f7crd
kubectl get pods
```

Four Pods were listed again, with a new one (`new-replica-set-gctw7`) in place of the deleted one. The controller compares current Pods to the desired count, sees 3 of 4, and creates a replacement from the Pod template. Because the template still referenced `busybox777`, the replacement also failed to pull its image.

### 3. Fix `replicaset-definition-1.yaml` (wrong apiVersion)

```bash
kubectl create -f replicaset-definition-1.yaml
vi replicaset-definition-1.yaml
kubectl create -f replicaset-definition-1.yaml
```

The first attempt failed with `no matches for kind "ReplicaSet" in version "v1"`. The file declared the core API group; ReplicaSet lives in `apps`.

```diff
- apiVersion: v1
+ apiVersion: apps/v1
```

### 4. Fix `replicaset-definition-2.yaml` (selector/label mismatch)

```bash
vi replicaset-definition-2.yaml
kubectl create -f replicaset-definition-2.yaml
```

The Pod template label did not match the ReplicaSet selector, which the API server rejects:

```diff
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
-       tier: nginx
+       tier: frontend
```

### 5. Delete both ReplicaSets

```bash
kubectl delete rs replicaset-1 replicaset-2
```

Deleting a ReplicaSet also deletes the Pods it manages.

### 6. Fix the image on `new-replica-set`

```bash
kubectl edit rs new-replica-set
```

```diff
      containers:
      - name: busybox-container
-       image: busybox777
+       image: busybox
```

Editing the template does not touch Pods that already exist, so the four broken Pods were deleted one by one and the controller recreated each from the corrected template:

```bash
kubectl delete pod <pod-name>
```

## Outcome

```
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         0       17s
```

```
replicaset.apps/replicaset-1 created
replicaset.apps/replicaset-2 created
replicaset.apps "replicaset-1" deleted from default namespace
replicaset.apps "replicaset-2" deleted from default namespace
replicaset.apps/new-replica-set edited
```

Key takeaways:

- A ReplicaSet self-heals: delete a Pod and it is replaced to hit the desired count.
- Self-healing recreates from the *template*, so deleting Pods does not fix a template fault.
- `spec.selector` must match `spec.template.metadata.labels`.
- ReplicaSets use `apiVersion: apps/v1`, not `v1`.

## Commands I Got Wrong

- `get pods` — ran without the `kubectl` prefix. Bash treated `get` as a program name: `-bash: get: command not found`.
- `kubectl delete new-replica-set-f7crd` — left out the resource type. kubectl read the Pod name as a type and returned `the server doesn't have a resource type "new-replica-set-f7crd"`. Correct: `kubectl delete pod new-replica-set-f7crd`.
- `kubectl create rs -f replicaset-definition-1.yaml` — extra `rs` argument. With `-f`, the kind comes from the file, and `create` has no `rs` subcommand. Returned `Unexpected args: [rs]`. Correct: `kubectl create -f replicaset-definition-1.yaml`.
- `kubectl create -f replicaset-definition-1.yaml` (first attempt) — the command was right, the file was not. `apiVersion: v1` produced `no matches for kind "ReplicaSet" in version "v1"`. The "ensure CRDs are installed first" hint is generic; the real cause was the wrong apiVersion.
- `kubectl delete rs replicaset-1, replicaset-2` — the comma became part of the first name. `replicaset-2` was deleted but `replicaset-1,` returned `NotFound`. Names are separated by spaces.
- `edit new-replica-set` — missing `kubectl`, same cause as `get pods`.
- `kubectl edit new-replica-set` — missing resource type, same cause as the delete mistake. Correct: `kubectl edit rs new-replica-set`.

Pattern: every kubectl command here is either `kubectl <verb> <resource-type> <name>` or `kubectl <verb> -f <file>`.

## Files

- `manifests/new-replica-set.yaml` — reconstructed manifest for the corrected `new-replica-set` (4 replicas, selector `name=busybox-pod`, image `busybox`)

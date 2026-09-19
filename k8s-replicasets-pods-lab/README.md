# Kubernetes Lab: ReplicaSets and Pods

Solutions to fixing a mis-configured replicaset with incorrect pod setups. Includes incorrect commands I sent, vi edits and successful commands.

## Contents

| File | Topic |
|------|-------|
| 01-inspect-replicaset-and-pods.md | Reading ReplicaSet and Pod state, identifying why pods are not ready |
| 02-delete-pod-and-observe-replacement.md | Deleting a managed pod and observing the ReplicaSet response |
| 03-fix-replicaset-definition-1.md | Wrong apiVersion in replicaset-definition-1.yaml |
| 04-fix-replicaset-definition-2.md | Selector and template label mismatch in replicaset-definition-2.yaml |
| 05-delete-replicasets.md | Removing replicaset-1 and replicaset-2 |
| 06-fix-new-replica-set-image.md | Correcting the image on new-replica-set |

## Commands entered incorrectly

### 1. `get pods`

```
controlplane ~ ➜ get pods
-bash: get: command not found
```

The command was entered without `kubectl`. The shell treated `get` as a program name and found none. `get` is a kubectl subcommand, not a shell command.

Correct:

```
kubectl get pods
```

### 2. `kubectl delete new-replica-set-f7crd`

```
error: the server doesn't have a resource type "new-replica-set-f7crd"
```

`kubectl delete` expects the resource type first, then the name. With only one argument, kubectl read the pod name as a resource type and asked the API server for a type with that name, which does not exist.

Correct:

```
kubectl delete pod new-replica-set-f7crd
```

### 3. `kubectl create rs -f replicaset-definition-1.yaml`

```
error: Unexpected args: [rs]
```

With `-f`, kubectl takes the resource kind from the `kind` field in the file, so no resource type is passed on the command line. `kubectl create` also has no `rs` or `replicaset` subcommand, so the extra argument was rejected.

Correct:

```
kubectl create -f replicaset-definition-1.yaml
```

### 4. `kubectl create -f replicaset-definition-1.yaml` (first attempt)

```
error: resource mapping not found for name: "replicaset-1" namespace: "" from
"replicaset-definition-1.yaml": no matches for kind "ReplicaSet" in version "v1"
ensure CRDs are installed first
```

The command was correct. The file was not. It declared `apiVersion: v1`, which is the core API group (Pod, Service, and similar). ReplicaSet is in the `apps` group and requires `apiVersion: apps/v1`. The "ensure CRDs are installed first" text is a generic hint in this error; the actual cause was the wrong apiVersion. See 03-fix-replicaset-definition-1.md.

### 5. `kubectl delete rs replicaset-1, replicaset-2`

```
replicaset.apps "replicaset-2" deleted from default namespace
Error from server (NotFound): replicasets.apps "replicaset-1," not found
```

The shell splits arguments on whitespace, not commas. The first name was passed to kubectl as `replicaset-1,` with the comma included, which does not match any ReplicaSet. `replicaset-2` was a clean name and was deleted. Multiple names are separated by spaces.

Correct:

```
kubectl delete rs replicaset-1 replicaset-2
```

### 6. `edit new-replica-set`

```
-bash: edit: command not found
```

Same cause as command 1: `kubectl` was omitted, so the shell looked for a program named `edit`.

### 7. `kubectl edit new-replica-set`

```
error: the server doesn't have a resource type "new-replica-set"
```

Same cause as command 2: the resource type was omitted, so kubectl interpreted the name as a resource type.

Correct:

```
kubectl edit rs new-replica-set
```

## Pattern

Every kubectl command in this lab follows one of two forms:

```
kubectl <verb> <resource-type> <name>
kubectl <verb> -f <file>
```

Commands 2, 3, 5, and 7 failed by breaking one of these forms. Commands 1 and 6 failed by dropping `kubectl` entirely.

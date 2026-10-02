# CKAD Lab: Core Concepts — ReplicaSets

Part of hands-on CKAD prep (KodeKloud labs). Maps to course section: Core Concepts (Pods, ReplicaSets, Deployments, Namespaces)

## Problem

Describe the lab scenario in your own words — don't copy KodeKloud's question text verbatim.

Example: "Create a ReplicaSet that maintains N replicas of a pod running [image]. Then simulate a failure and observe how the ReplicaSet self-heals."

Objectives:
- Fill in objective 1 (e.g., create a ReplicaSet manifest with X replicas)
- Fill in objective 2 (e.g., update the replica count)
- Fill in objective 3 (e.g., delete a pod and observe self-healing)

## Solution

See `manifests/replicaset.yaml`.

```bash
kubectl apply -f manifests/replicaset.yaml
```

Verification:

```bash
kubectl get rs
kubectl get pods -l app=<fill-in-label>
```

Scaling / failure simulation:

```bash
# e.g. kubectl delete pod <pod-name>
# e.g. kubectl scale rs <rs-name> --replicas=5
```

## Outcome

Paste actual terminal output here. Describe what happened — did a new pod get scheduled automatically? How fast? Did labels/selectors cause any mismatch errors?

```
# paste actual terminal output
```

## Commands I Got Wrong

List any command, flag, or manifest field that didn't work the first time, what the error was, and why it was wrong.

Example format:
- `selector.matchLabels: app: web` vs pod template `labels: app: webapp` — ReplicaSet came up with 0 pods, no error. Fixed by matching the labels exactly. `kubectl describe rs` is what surfaced it.

## Files

- `manifests/replicaset.yaml` — the ReplicaSet manifest used in this lab

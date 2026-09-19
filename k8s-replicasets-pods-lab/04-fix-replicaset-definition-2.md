# 04 - Fix replicaset-definition-2.yaml

## Problem

Create a ReplicaSet from `replicaset-definition-2.yaml`. The file contained an error that prevented creation.

## Solution

The file was corrected in `vi` before the create command was run:

```
vi replicaset-definition-2.yaml
```

The Pod template labels did not match the ReplicaSet selector. The template label was changed to match the selector:

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

Then:

```
kubectl create -f replicaset-definition-2.yaml
```

## Outcome

```
replicaset.apps/replicaset-2 created
```

A ReplicaSet's `spec.selector` must match the labels in `spec.template.metadata.labels`. The selector is how the ReplicaSet finds the Pods it owns, and the template labels are what the Pods it creates will carry. If they differ, the ReplicaSet would create Pods it could never count as its own, so the API server rejects the object. With matching labels, the ReplicaSet was accepted and can track its Pods.

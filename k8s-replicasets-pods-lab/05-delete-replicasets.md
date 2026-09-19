# 05 - Delete replicaset-1 and replicaset-2

## Problem

Remove the two ReplicaSets created from the definition files.

## Solution

First attempt, with a comma between names:

```
kubectl delete rs replicaset-1, replicaset-2
```

Result:

```
replicaset.apps "replicaset-2" deleted from default namespace
Error from server (NotFound): replicasets.apps "replicaset-1," not found
```

The comma became part of the first name (`replicaset-1,`), which matched nothing. Names are separated by spaces. The remaining ReplicaSet was deleted on its own:

```
kubectl delete rs replicaset-1
```

## Outcome

```
replicaset.apps "replicaset-1" deleted from default namespace
```

Both ReplicaSets were removed. Deleting a ReplicaSet also deletes the Pods it manages, so no Pods from either definition file remained. The single-command form is:

```
kubectl delete rs replicaset-1 replicaset-2
```

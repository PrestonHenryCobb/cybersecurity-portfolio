# 02 - Delete a Pod and observe the ReplicaSet response

## Problem

Delete one of the Pods managed by `new-replica-set` and observe how the ReplicaSet responds.

## Solution

```
kubectl delete pod new-replica-set-f7crd
kubectl get pods
```

## Outcome

The Pod was deleted:

```
pod "new-replica-set-f7crd" deleted from default namespace
```

The next `kubectl get pods` showed four Pods again, with a new one, `new-replica-set-gctw7` (age 31s, `ErrImagePull`), in place of the deleted one. The other three Pods were unchanged.

The ReplicaSet controller compares current Pods to the desired count (4). When a Pod is removed, the count drops to 3 and the controller creates a replacement from the Pod template. Because the template still referenced `busybox777`, the replacement failed to pull the image in the same way. Deleting Pods does not fix a fault that lives in the template.

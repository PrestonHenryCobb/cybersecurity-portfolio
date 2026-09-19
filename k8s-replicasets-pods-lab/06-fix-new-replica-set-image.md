# 06 - Fix the image on new-replica-set

## Problem

All four Pods of `new-replica-set` were stuck in `ImagePullBackOff` because the Pod template referenced the image `busybox777`. Correct the ReplicaSet so its Pods use a valid image.

## Solution

Open the live object for editing:

```
kubectl edit rs new-replica-set
```

In the `vi` session, change the image in the Pod template:

```diff
      containers:
      - name: busybox-container
-       image: busybox777
+       image: busybox
```

Save and exit. Then:

```
kubectl apply -f new-replica-set.yaml
```

## Outcome

```
replicaset.apps/new-replica-set edited
```

The Pod template on `new-replica-set` now references a valid image. Pods the ReplicaSet creates from this point use the corrected template. A ReplicaSet does not update Pods that already exist, so the four Pods still carrying `busybox777` are not changed by the edit. They must be deleted, after which the controller recreates them from the corrected template (see 02-delete-pod-and-observe-replacement.md for the replacement behavior).

For this task, the following command is used
```
kubectl delete pod <pod-name>
```
Each pod deleted will be replaced automatically to satisfy the desired scale of the replica set.

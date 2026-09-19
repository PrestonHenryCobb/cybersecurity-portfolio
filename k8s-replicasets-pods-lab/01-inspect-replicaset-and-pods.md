# 01 - Inspect the existing ReplicaSet and Pods

## Problem

Determine which Pods and ReplicaSets exist in the default namespace, how many replicas the ReplicaSet is configured for, which image its containers use, and why none of the Pods are ready.

## Solution

```
kubectl get pods
kubectl get rs
kubectl describe rs new-replica-set
kubectl get pods
```

## Outcome

The first `kubectl get pods` and `kubectl get rs` returned `No resources found in default namespace.` The ReplicaSet appeared on the fourth query with an age of 17 seconds:

```
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         0       17s
```

`kubectl describe rs new-replica-set` showed:

- Selector: `name=busybox-pod`
- Replicas: 4 current / 4 desired
- Pods Status: 0 Running / 4 Waiting
- Container: `busybox-container`, image `busybox777`, command `sh -c "echo Hello Kubernetes! && sleep 3600"`
- Events: four `SuccessfulCreate` events from `replicaset-controller`

The events show the ReplicaSet controller did its job and created all four Pods. The Pods themselves were not ready. `kubectl get pods` showed all four in `ImagePullBackOff`, meaning the kubelet could not pull the image `busybox777`. The fault was in the image name in the Pod template, not in the ReplicaSet controller.

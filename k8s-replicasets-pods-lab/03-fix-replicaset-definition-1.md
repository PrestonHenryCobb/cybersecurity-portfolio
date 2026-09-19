# 03 - Fix replicaset-definition-1.yaml

## Problem

Create a ReplicaSet from `replicaset-definition-1.yaml`. The file contained an error that prevented creation.

## Solution

The first attempt, `kubectl create -f replicaset-definition-1.yaml`, failed:

```
error: resource mapping not found for name: "replicaset-1" namespace: "" from
"replicaset-definition-1.yaml": no matches for kind "ReplicaSet" in version "v1"
```

The file declared the wrong API version. Edit in `vi`:

```
vi replicaset-definition-1.yaml
```

```diff
- apiVersion: v1
+ apiVersion: apps/v1
```

Then create again:

```
kubectl create -f replicaset-definition-1.yaml
```

## Outcome

```
replicaset.apps/replicaset-1 created
```

`v1` is the core API group and does not contain ReplicaSet. ReplicaSet is served from the `apps` group, so the manifest must use `apps/v1`. With the correct apiVersion, the API server could map `kind: ReplicaSet` to a resource and accepted the object.

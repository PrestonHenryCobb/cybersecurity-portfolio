# CKAD Lab: ReplicaSets

Part of my hands-on CKAD prep (KodeKloud labs), rewritten in my own words as portfolio documentation.

## 🎯 Scenario

> Describe the lab scenario in your own words — don't copy KodeKloud's question text verbatim.
>
> Example: "Create a ReplicaSet that maintains N replicas of a pod running [image]. Then simulate a failure and observe how the ReplicaSet self-heals."

**What I was asked to do:**
- [ ] Fill in objective 1 (e.g., create a ReplicaSet manifest with X replicas)
- [ ] Fill in objective 2 (e.g., update the replica count)
- [ ] Fill in objective 3 (e.g., delete a pod and observe self-healing)

## 🧠 Key Concepts Covered

- What a ReplicaSet is and how it differs from a bare Pod / a Deployment
- `selector` / `matchLabels` and why they must match the pod template's labels
- How the ReplicaSet controller reconciles desired vs. actual state

## 🛠️ What I Did

### 1. Wrote the manifest

See [`manifests/replicaset.yaml`](manifests/replicaset.yaml).

```bash
kubectl apply -f manifests/replicaset.yaml
```

### 2. Verified it came up

```bash
kubectl get rs
kubectl get pods -l app=<fill-in-label>
```

**Output:**
```
# paste your actual terminal output here
```

### 3. Simulated failure / scaling

```bash
# e.g. kubectl delete pod <pod-name>
# e.g. kubectl scale rs <rs-name> --replicas=5
```

**What happened:**
> Describe what you observed — did a new pod get scheduled automatically? How fast? Did labels/selectors cause any mismatch errors?

## 🐛 What Tripped Me Up

> This is the most valuable section for anyone reading — be specific and honest.
>
> Example: "I initially set the selector's `matchLabels` to `app: web` but the pod template's labels were `app: webapp` — the ReplicaSet came up with 0 pods and no error, just silently failed to match. Took me a few minutes of `kubectl describe rs` to catch the mismatch."

## ✅ What I'd Do Differently / Takeaways

- Takeaway 1
- Takeaway 2

## 🎥 Video Walkthrough

> [Link to unlisted YouTube video, if you recorded one]

## 📁 Files

- [`manifests/replicaset.yaml`](manifests/replicaset.yaml) — the ReplicaSet manifest used in this lab

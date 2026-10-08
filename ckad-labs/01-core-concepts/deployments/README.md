# CKAD Lab: Core Concepts — Deployments

Part of hands-on CKAD prep (KodeKloud labs). Maps to course section: Core Concepts (Pods, ReplicaSets, Deployments, Namespaces)

## Problem

Starting state investigation in the default namespace: count existing Pods, ReplicaSets, and Deployments, then recheck those counts after a new Deployment is introduced.

A pre-existing Deployment (`frontend-deployment`) was already running and not reaching Ready state. The lab required inspecting it to determine the image used and why it wasn't ready.

Two creation tasks followed:
- Create a Deployment named `deployment-1` from a provided file, `deployment-definition-1.yaml` (located at `/root/`), which contained an error that needed to be fixed before it would apply.
- Create a new Deployment from scratch with: name `httpd-frontend`, 3 replicas, image `httpd:2.4-alpine`.

## Solution

Checked existing resource counts:

```bash
kubectl get pods
kubectl get rs
kubectl get deploy
```

Inspected the broken pre-existing deployment:

```bash
kubectl get deploy
kubectl get rs
kubectl get pods
kubectl describe deploy frontend-deployment
```

`kubectl describe deploy` showed the container image was `busybox888`, which doesn't exist — explaining the `ImagePullBackOff` status on all 4 pods and 0/4 ready.

Fixed and applied the first provided manifest:

```bash
vi deployment-definition-1.yaml
kubectl create -f deployment-definition-1.yaml
```

The file had `kind: deployment` (lowercase) instead of `kind: Deployment`. Fixing the casing let it apply, creating `deployment-1`.

Created the second Deployment imperatively:

```bash
kubectl create deploy httpd-frontend --image=httpd:2.4-alpine
kubectl edit deploy httpd-frontend
```

`kubectl edit` was used afterward to bring the deployment in line with the full required spec (replicas, etc.) beyond what the imperative create command alone sets.

## Outcome

```
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment-1          2/2     2            2           14s
frontend-deployment   0/4     4            0           21m
```

```
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment-1          2/2     2            2           12m
frontend-deployment   0/4     4            0           33m
httpd-frontend        3/3     3            3           98s
```

`deployment-1` and `httpd-frontend` both reached full ready state. `frontend-deployment` was left at 0/4 — that one was the pre-existing broken deployment being investigated (bad image name), not something the lab asked to be fixed.

## Commands I Got Wrong

- `get pods` — ran without the `kubectl` prefix. Bash doesn't know `get` as a command on its own.
- `kubectl vi deployment-definition-1.yaml` — tried to open the editor through kubectl. `vi` isn't a kubectl subcommand; it's a separate shell command (`vi deployment-definition-1.yaml`).
- `kubectl create deploy - deployment-definition-1.yaml` — malformed flag, missing `-f`. Returned `required flag(s) "image" not set` because without `-f`, `create deploy` was being read as the imperative command, which needs `--image`.
- `kubectl create deployment-definition-1.yaml` — passed the filename as a bare argument with no `-f` and no `deploy` subcommand. Returned `Unexpected args`.
- `kubectl create deploy -f deployment-definition-1.yaml` — `-f` isn't a valid flag for the `create deploy` imperative subcommand. That syntax is for `kubectl create -f <file>` directly (declarative, no `deploy` keyword).
- `kubectl get deplot` — typo for `deploy`. Server returned `the server doesn't have a resource type "deplot"`.
- `kubectl create deploy --image=httpd:2.4-alpine` — left out the deployment name. `create deploy` requires `NAME` as a positional argument before any flags. Returned `exactly one NAME is required, got 0`.

## Files

- `manifests/deployment-definition-1.yaml` — reconstructed version of the provided file (original not saved; `kind` fixed to `Deployment`, other values representative)
- `manifests/httpd-frontend-deployment.yaml` — equivalent declarative manifest for the imperatively-created `httpd-frontend` deployment, for reference

# Kubernetes RollingUpdate Deployment

This project demonstrates a **Deployment** in Kubernetes with multiple replicas and a **RollingUpdate strategy**. It shows how to safely update applications with zero downtime.

---

## Goals
- Learn how to create a Deployment with replicas
- Understand label selectors and how Deployments manage Pods
- Implement and observe a RollingUpdate for smooth updates
- Monitor deployment rollouts using `kubectl`

---

## Prerequisites
- A Kubernetes cluster (Minikube, Kind, or KillerCoda)
- `kubectl` installed and configured

---

## Deployment Manifest

The manifest (`manifests/deployment.yaml`) contains:
- Namespace: `example-ns`
- Deployment: `rolling-deployment` with 3 replicas
- Container: `nginx:1.25`
- RollingUpdate strategy:
  - `maxUnavailable: 1` → at most 1 Pod can be unavailable during update
  - `maxSurge: 1` → at most 1 extra Pod is created during update

---

## Quickstart

1. **Apply the Deployment**
kubectl apply -f manifests/deployment.yaml

--------------------------------------------------------------
2. Verify Pods

kubectl get pods -n example-ns -o wide

--------------------------------------------------------------
3. Perform a Rolling Update
Update the image version to nginx:1.28:

kubectl set image deployment/rolling-deployment nginx=nginx:1.28 -n example-ns

------------------------------------------
4. Monitor rollout

kubectl rollout status deployment/rolling-deployment -n example-ns
Pods will be replaced one at a time according to maxUnavailable and maxSurge.

-------------------------------------------
5. Check rollout history

kubectl rollout history deployment/rolling-deployment -n example-ns

--------------------------------------------------------------
6. Undo last rollout (if needed)


kubectl rollout undo deployment/rolling-deployment -n example-ns

---------------------------------------------------------------
7. Optional: Access Deployment
Use port-forwarding to test locally:

kubectl port-forward svc/rolling-deployment 8080:80 -n example-ns
curl http://localhost:8080
Note: A Service may need to be created for external access.
-------------------------------------------------------------------
8. Cleanup
kubectl delete -f manifests/deployment.yaml
kubectl delete namespace example-ns

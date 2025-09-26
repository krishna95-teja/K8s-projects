# Hello-World Kubernetes Pod + Service

A beginner-friendly Kubernetes project demonstrating:
- Pod creation with Nginx
- Namespace usage
- ClusterIP Service

---

## Goals
- Learn the basics of Pods, Services, and namespaces in Kubernetes.
- Understand labels and selectors for connecting Pods to Services.

---

## Prerequisites
- Kubernetes cluster (minikube, kind, or KillerCoda)
- `kubectl` installed and configured

---

## Quickstart

1. **Apply the manifest**

kubectl apply -f manifests/hello-world.yaml
Verify the namespace, Pod, and Service


kubectl get ns
kubectl get pods -n example-ns
kubectl get svc -n example-ns
Access the Nginx server

Option 1: Port-forward

kubectl port-forward svc/hello-clusterip 8080:80 -n example-ns
Open browser or use curl:

curl http://localhost:8080
You should see the default Nginx welcome page.

Optional: Check Pod logs
kubectl logs hello-world-pod -n example-ns

Cleanup
kubectl delete -f manifests/hello-world.yaml
kubectl delete namespace example-ns

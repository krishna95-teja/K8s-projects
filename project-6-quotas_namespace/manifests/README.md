Project 6: Resource Quotas Across Multiple Namespaces


📝 Overview

This project demonstrates how to apply ResourceQuotas across two namespaces (ns1-project and ns2-project) in Kubernetes.
ResourceQuotas allow cluster administrators to limit resource usage per namespace, preventing a single namespace from consuming excessive cluster resources.

------------------------------------------------------------------------------------------------------------------------

⚙️ Key Concepts

Namespace isolation – Each namespace (ns1-project and ns2-project) acts as a separate resource boundary.

ResourceQuota object – Defines hard limits on compute, storage, and object counts inside a namespace.

Differing workloads – ns1-project has smaller quotas for lighter workloads (dev/test), while ns2-project has slightly larger quotas to simulate staging or small-scale production.

-----------------------------------------------------------------------------------------------------------------------

📂 Resources Defined

1. Namespaces
apiVersion: v1
kind: Namespace
metadata:
   name: ns1-project    # dev/test namespace

apiVersion: v1
kind: Namespace
metadata:
   name: ns2-project    # staging/small prod namespace

2. ResourceQuota for ns1-project
apiVersion: v1
kind: ResourceQuota
metadata:
   name: ns1-rq
   namespace: ns1-project
spec:
   hard: 
      requests.cpu: 4           # Max CPU requests
      requests.storage: 8Gi     # Max storage requests
      limits.cpu: 10            # Max CPU allowed
      limits.storage: 16Gi      # Max storage allowed
      pods: 10                  # Max pods allowed
      services: 5               # Max services allowed
      configmaps: 3             # Max ConfigMaps allowed
      secrets: 3                # Max Secrets allowed

3. ResourceQuota for ns2-project
apiVersion: v1
kind: ResourceQuota
metadata:
   name: ns2-rq
   namespace: ns2-project
spec:
   hard: 
      requests.cpu: 5           # Max CPU requests
      requests.storage: 10Gi    # Max storage requests
      limits.cpu: 12            # Max CPU allowed
      limits.storage: 24Gi      # Max storage allowed
      pods: 12                  # Max pods allowed
      services: 6               # Max services allowed
      configmaps: 4             # Max ConfigMaps allowed
      secrets: 4                # Max Secrets allowed

--------------------------------------------------------------------------------------------------

✅ Best Practices Followed

Created separate namespaces to isolate workloads.

Applied ResourceQuota per namespace to enforce resource governance.

Used different quotas to simulate real-world environment differences:

ns1-project → Dev/Test → Restricted quotas

ns2-project → Staging/Small Prod → Looser quotas

----------------------------------------------------------------------------------------------------

🚀 Learning Outcomes

Understand how ResourceQuotas enforce fair resource distribution.

Learn to manage CPU, storage, pods, services, and objects per namespace.

See the practical difference between lightweight and heavier workloads in a cluster.

---------------------------------------------------------------------------------------------------

🔧 Verification Commands

Run the following to check applied quotas:

kubectl get namespaces
kubectl describe quota ns1-rq -n ns1-project
kubectl describe quota ns2-rq -n ns2-project

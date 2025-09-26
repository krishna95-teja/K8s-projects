Project 3 – ConfigMaps and Secrets


1. Objective

To demonstrate how to use ConfigMaps and Secrets in Kubernetes Pods, both as:

Environment variables

Mounted files (via volumes)

------------------------------------
------------------------------------

 2. Resources Created

Namespace – example-ns (for isolation)

ConfigMap – Stores application configuration (APP_NAME)

Secret – Stores sensitive data (DB_USER)

Pod – Runs an Nginx container and consumes the ConfigMap & Secret in two ways:

As environment variables

As mounted files inside the container

---------------------------------------
---------------------------------------

 3. How to Run

Apply the YAML file:

kubectl apply -f project-3-configmap-secret.yaml


Verify resources:

kubectl get all -n example-ns


Check environment variables inside Pod:

kubectl exec -it pod-project -n example-ns -- printenv | grep APP_DETAILS
kubectl exec -it pod-project -n example-ns -- printenv | grep DB_USER


Check mounted files inside Pod:

kubectl exec -it pod-project -n example-ns -- cat /etc/cmap/APP_NAME
kubectl exec -it pod-project -n example-ns -- cat /etc/secret/DB_USER

🚀 Project 11 – Horizontal Pod Autoscaler (HPA) with Resource Quota

------------------------------------------------------------------------------------------------------------------------------------
📘 Objective

This project demonstrates how to configure a Horizontal Pod Autoscaler (HPA) that automatically scales a deployment based on CPU utilization, while enforcing resource consumption limits using a ResourceQuota at the namespace level.

⚙️ Components Created
Resource Type	Name	Purpose
ResourceQuota	rq-project	Restricts total CPU and memory requests/limits in the example-ns namespace.
Deployment	dep-project	Runs an Nginx app with defined CPU and memory requests for autoscaling.
HorizontalPodAutoscaler	hpa-project	Automatically adjusts the number of replicas based on CPU utilization metrics.

-------------------------------------------------------------------------------------------------------------------------------------
🧩 YAML Explanation
1️⃣ ResourceQuota
apiVersion: v1
kind: ResourceQuota
metadata:
   name: rq-project
   namespace: example-ns
spec:
   hard:
      limits.cpu: 5
      limits.memory: 10Gi
      requests.cpu: 2
      requests.memory: 3Gi

Controls total resource allocation in the namespace.

Prevents HPA or new pods from exceeding defined cluster limits.

Ensures fair usage and stability in shared environments.


2️⃣ Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
   name: dep-project
   namespace: example-ns
spec:
   replicas: 3
   selector:
      matchLabels:
         tier: backend
   template:
      metadata:
         labels:
            tier: backend
      spec:
         containers:
            - name: nginx
              image: nginx:1.25
              ports:
                 - containerPort: 80
              resources:
                 requests:
                    cpu: 100m
                    memory: 128Mi
                 limits:
                    cpu: 200m
                    memory: 256Mi

Runs an Nginx web app with predefined CPU/memory requests and limits.

These values are essential for:

HPA metrics (which use CPU utilization to trigger scaling)

Quota enforcement (ensuring namespace limits aren’t breached).


3️⃣ Horizontal Pod Autoscaler
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
   name: hpa-project
   namespace: example-ns
spec:
   scaleTargetRef:
      apiVersion: apps/v1
      kind: Deployment
      name: dep-project
   minReplicas: 1
   maxReplicas: 5
   metrics:
      - type: Resource
        resource:
           name: cpu
           target:
              type: Utilization
              averageUtilization: 70

Links directly to the dep-project Deployment.

Monitors CPU usage across its pods.

When average CPU > 70%, it scales up; when below 70%, scales down.

Ensures optimal resource utilization within the quota boundaries.

-----------------------------------------------------------------------------------------------------------------------------------

🧠 How It Works

Initial Setup:
The deployment starts with 3 replicas of Nginx.

Load Increases:
When incoming traffic or CPU load rises, HPA monitors the metrics.

HPA Scales Up:
New pods are added—up to 5 replicas—to maintain target CPU utilization.

ResourceQuota Enforces Limits:
If total requested resources exceed 2 CPU or 3 Gi memory, scaling stops, protecting cluster stability.


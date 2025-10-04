PROJECT 5: Kubernetes Probes (Startup, Liveness & Readiness)

📌 Objective

This project demonstrates how to use startupProbe, livenessProbe, and readinessProbe in Kubernetes to monitor the health of a containerized application. The probes ensure the app starts correctly, stays alive, and is ready to serve traffic.

🛠️ Resources Created
1. Namespace

Name: example-ns

Purpose: Uses the same namespace as previous projects to maintain consistency and isolate resources.

2. Pod

Name: probes-project

Namespace: example-ns

Image: nginx:1.28 (version specified as best practice)

Ports: 80

--------------------------------------------------------------------------------------------------------------------

🔹 Probes
1. Startup Probe

Checks if the container has started properly before other probes begin.

Config:

failureThreshold: 10  # 10 consecutive failures allowed
periodSeconds: 5      # check every 5s


Total wait before failure: 50 seconds (10 × 5s).

2. Liveness Probe

Monitors if the container is still running. If it fails, Kubernetes restarts the container.

Config:

initialDelaySeconds: 15
periodSeconds: 10

3. Readiness Probe

Determines if the container is ready to accept traffic.

Config:

initialDelaySeconds: 5
periodSeconds: 5

-------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------

✅ Workflow

Pod created in example-ns.

StartupProbe ensures container is fully started before liveness/readiness probes are checked.

LivenessProbe monitors the container continuously and restarts it if it fails.

ReadinessProbe ensures traffic is only sent to fully ready containers.

--------------------------------------------------------------------------------------------------
📂 Use Case Example

Ensures that critical apps like web servers or APIs do not serve traffic until ready.

Automatically restarts containers that become unhealthy, improving application reliability.

--------------------------------------------------------------------------------------------------
📝 Best Practices Used

Explicit namespace usage (example-ns).

Versioned container image (nginx:1.28).

Realistic probe thresholds to avoid long unnecessary waits.

Clear separation of startup, liveness, and readiness checks for robust monitoring.

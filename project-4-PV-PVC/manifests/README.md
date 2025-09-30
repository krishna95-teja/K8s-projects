 Resources Created

1. Namespace

Name: example-ns

Purpose: Isolates resources for better organization and management.

2. PersistentVolume (PV)

Name: pv-project

Scope: Cluster-scoped (not bound to any namespace).

Access Mode: ReadWriteOnce → Storage can be mounted as read/write by a single node.

Capacity: 1Gi.

Reclaim Policy: Retain → Data is not deleted even if PVC is released.

Backed by: hostPath: /mnt/data (local path on the cluster node).


3. PersistentVolumeClaim (PVC)

Name: pvc-project.

Namespace: example-ns.

Access Mode: ReadWriteOnce.

Requests: 500Mi storage.

Behavior: Will bind to pv-project since it satisfies the requested capacity and access mode.


4. Pod

Name: pod-project.

Namespace: example-ns.

Image: nginx:1.28 (explicit version as best practice).

Mounts:

PVC pvc-project is mounted at /mnt/storage/project inside the container.

Data written here persists because it’s backed by the PV (/mnt/data on host).


----------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------

✅ Workflow

Namespace created → isolates all resources.

PV created → cluster-level storage resource.

PVC created → requests storage, binds automatically to PV.

Pod created → mounts PVC → uses persistent storage path.

-----------------------------------------------------------------------------

📂 Use Case Example

A web server (nginx) writing logs or uploaded files.

Application state/data is retained even if Pod is deleted and recreated.

-----------------------------------------------------------------------------

📝 Best Practices Used

Explicit namespace specification (example-ns).

Versioned image (nginx:1.28).

Reclaim policy set to Retain for safety.

Proper resource requests in PVC (500Mi < 1Gi of PV).

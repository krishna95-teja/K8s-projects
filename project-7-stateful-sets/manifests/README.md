Project 8 – Headless Service with StatefulSet, PersistentVolume & SECRET


Description

This project demonstrates the use of a headless service to manage stateful applications using StatefulSet, along with persistent storage and sensitive data management using PersistentVolume and Secret. The setup ensures pod identity is stable, storage persists across restarts, and database credentials are securely injected.


--------------------------------------------------------------------------------------------------------

Components

1. Namespace

example-ns is used for isolation of resources in the cluster.


2. Headless Service

Name: headlesssvc-project

ClusterIP: None → Marks the service as headless.

Selector: tier: database-project → Associates the service with StatefulSet pods.

Purpose: Enables direct pod-to-pod communication for stateful workloads without load balancing.


3. PersistentVolume (PV)

Name: pv-project

Cluster-scoped.

Access Mode: ReadWriteOnce

Capacity: 1Gi

Reclaim Policy: Retain → PV data persists even after deletion of PVC.


4. Secret

Name: secret-db

Namespace: example-ns

Type: Opaque

Stores sensitive information such as the MySQL root password.


5. StatefulSet

Name: sset-project

Namespace: example-ns

Replicas: 3

Service Name: headlesssvc-project → Links StatefulSet to headless service.

Selector: tier: database-project → Matches the labels of pods.

Containers: MySQL 8.0 with 3306 exposed.

Volume Mount: /mnt/storage → Mounted using volume claim template.

Secret Environment Variable: db-username → Injected from secret-db.

Volume Claim Template:

Provides each pod with its own PVC of 500Mi.

--------------------------------------------------------------------------------------------------------

Key Points

Headless Service allows pods to communicate directly with stable network identities internally.

StatefulSet ensures pods are created in order with persistent identities.

PersistentVolume keeps data intact even if pods restart or are deleted.

Secret keeps sensitive credentials secure and injects them as environment variables into containers.

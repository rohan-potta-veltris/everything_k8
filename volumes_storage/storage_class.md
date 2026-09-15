Static Provisioning as a storage admin we are pre-provisioning the stroage volume via PV and the PVC can take a slice

Dynamic Provisioning is where we just make a storage class (SC) and then have a PVC created it will automatically make a PV and bind it

The SC has the follwoing that needs to be decalred in the manifest file:
1. Provisioner => EBS , azure files, GCP
2. Parameters
3. Reclaim ploicy 
    Delete =>When the PVC is deleted, Kubernetes can delete the PV and the dynamically provisioned storage behind it.
    Retain => When the PVC is deleted, Kubernetes does NOT automatically delete the underlying storage.


Dynamic Provisioning will also make the PV with the same mode as the one asked in the PVC as well


Volume Binding Mode:
Immediate: As soon as you create the PVC, The Pod doesn't need to exist yet. => use this when it doesnt depend on pod 

WaitForFirstConsumer: This delays the volume binding/provisioning until a Pod actually uses the PVC.


PVC
 │
 ├── Does it specify storageClassName?
 │       │
 │       ├── Yes → use that StorageClass
 │       │           ↓
 │       │        Provisioner
 │       │           ↓
 │       │          New PV
 │       │
 │       └── No → check for a default StorageClass
 │                   │
 │                   ├── Default SC exists → use it
 │                   │                       ↓
 │                   │                     New PV
 │                   │
 │                   └── No default SC → try to bind
 │                       to an existing suitable PV
 │
 └── Existing matching PV may be used

 Kubernetes StorageClass: Immediate vs WaitForFirstConsumer

1. Immediate

PVC → StorageClass → PV/Storage is provisioned immediately

* Storage is created/bound as soon as the PVC is created.
* The Pod may not have been scheduled yet.
* Problem: for topology-dependent storage, the volume may be created in a different zone/node from where the Pod is eventually scheduled.
* Example: EBS volume is created in Zone A, but the Pod is scheduled in Zone B → volume may not be attachable.

Use when:

* Storage is not tied to a specific node or zone.
* Example: NFS/shared network storage.

2. WaitForFirstConsumer

PVC → Wait → Pod scheduled → Storage provisioned/bound

* PVC does not immediately provision/bind storage.
* Kubernetes first determines where the Pod should run.
* Then storage is provisioned/bound according to the Pod's node/zone/topology.
* Prevents topology mismatch.

Use when:

* Storage is tied to a node or zone.
* Examples: local PVs, AWS EBS, Azure Disk.

Key Difference:

Immediate = provision storage immediately.

WaitForFirstConsumer = wait until the Pod's location is known, then provision/bind storage appropriately.


In Kubernetes, topology = the location/placement of a resource within the cluster.
Region: us-east-1
│
├── Zone A
│   ├── Node 1
│   └── Node 2
│
└── Zone B
    ├── Node 3
    └── Node 4

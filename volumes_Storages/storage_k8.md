We dont want to use the volume of the emtpy_dir becasue this exists till the pod is live and if the pod is removed we loose that value as well.

So to make storage outside , we use persistent volume(PV) this is created before allocation , for example we make a PV for 100Gi , and then now the pod will use a PVC (persistent volume claim) and they will say this pod will need like 10Gi and what mode (maybe r/w mode) and then to that particulat volume and we match it and if all good we get a binding created.

now for a deployment all the pods will share the the pvc , menaing if 50Gi all the replicas will share that, 
If we want individual pod to have its own pvc we will use "statefulset"

The access modes are as follows:

RWO — ReadWriteOnce
    PV can be mounted as read/write by one node.
    Multiple Pods on that same node can potentially use it.
    Common with AWS EBS, Azure Managed Disk, etc.
ROX — ReadOnlyMany
    PV can be mounted read-only by multiple nodes.
    Pods can read the data but cannot modify it.
RWX — ReadWriteMany
    PV can be mounted read/write by multiple nodes.
    Useful when multiple Pods on different nodes need to share the same storage.
    Common with NFS, Azure Files, EFS, etc.


Reclaim policy is for what happens when the PVC is deleted , then retain means the PV will exist but no-one can use , delete PV delete then PVC also delete , and for recycle , PVC delete then the PV can be re-used 

PV and PVC is a 1:1 mapping , PVC can have a 1:many pod mappings 

Now we need to mention which node this volume is on else in multi-pod how will it know where it exists.

Now how does this overcome the taints , because we can explicitly say to place it on the master and it will do so and override the taints 

Storage class is basically when we need to provision the storage outside the cluster an nfs or cloud or something , we use this for that 

The SC is also dynamically allocates and this sits on the external storage of and NFD or AWS cloud storage and to find their capacity we use kubectl get sc and then investigate the path , once we find the path in the kubectl describe sc sc_name 

This sc is used as a way to dynamically allocate sotrage with manually making a PV , and sc has a multiple mapping to PVC , and then this gives exactly what is required.


PVC
 ↓
StorageClass
 ↓
CSI Driver / Provisioner
 ↓
Actual storage
 ↓
PV automatically created
 ↓
PVC binds to PV

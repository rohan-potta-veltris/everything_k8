how to take a backup of all the serivces?
kubectl get all -A -o yaml > backup.yaml

But this doestn have the detals for the following  as they dont show up at the all command stage 
ConfigMaps
Secrets
Ingresses
PersistentVolumeClaims
PersistentVolumes
StorageClasses
ServiceAccounts
Roles
RoleBindings
ClusterRoles
ClusterRoleBindings
NetworkPolicies
Custom Resources
CRDs

so the main thing that we need to take a backup of is the etcd, and this is the source of everything.

we take the backup during the upgrade , or any rollback and so on.
for cloud tools we need third part tools as we dont have access to control plane so only in managed clusters do we do this.

when we go in the manifest for the etcd , we will see that there is a path for the --data-dir at /var/lib/etcd and it is this backup that is needed 

to takt the backup 
https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/

ETCDCTL_API=3 , this is a env variable to use this version 
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>

To see the status of the backup
etcdutl --write-out=table snapshot status <backup-file-location>

To restore the data, 
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot restore <backup-file-location>

etcdutl --data-dir <data-dir-location> snapshot restore snapshot.db

ETCDUTL is the new one cli for this ,
this --data-dir is the new location for the new etcd data 
and the mountPath in the volument path and the hostpath as well

you might have to restart kubelet and the dameon as well 

then we need to go to the manifest file for the etcd and point it to the new data folder location
and also restart the api-server as well



How to upgrade the kubernetes cluster , via kubeadm

kubcetl node node_name drain means that it will remove the pods present in the node 
this also means that it is unscheuable
drain = evict all pods + cordon meaning its be marked as unschduable

to make it working again , kubectl node node_name uncordon

They way to upgrade,

1.30.2 => major_release.minor_release.patch

We can only upgrade one minor version at a time 

1.28.1 -> 1.30.2 (not possible)
1.28.1->1.29.2->1.30.2 (have to go every minor version)

this is needed because , Kubernetes changes its APIs, behavior, and internal components between minor releases, and the project only guarantees that an upgrade across one minor version will preserve the compatibility needed for a safe transition.

Note: k8 only keeps support of the 3 latest versions from the k8 side 

Updation methods for the worker nodes:
1. update all at once
2. rolling update => do it one at a time 
3. Blue Green => create a whole new k8 env 

For the master node , its needed to have more than 1 master node else we will have a downtime ,


For the componenets if api server is 1.30.X
controller and schedular can be x-1, kubelet and kubectl can be x-2

Kubeadm first , then kubelet needs to be upgraded seperately and kubectl as well

Note ideally we wont have to re-deploy the application as everythgin will be on teh deplyments and they get shared all across 





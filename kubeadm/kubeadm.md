Types of ways to install k8 

1. for POC and stuff we use kind , minikube and so on 
2. do we have self managed service, we handle the control plane  
    A) Vagrant/virtualbox 
    B) multipass
    C) VMs on Cloud 
    D) Baremetal/vmware
3. managed service (cloud stuff) the control plane is managed by the cloud provider.


https://github.com/piyushsachdeva/CKA-2024/tree/main/Resources/Day27
Creating a kubeadm
kubeadm = a tool that sets up the Kubernetes control plane and joins worker nodes to the cluster.

The key thing to remember is:

kubeadm init → first control plane

kubeadm join --control-plane → additional control planes

kubeadm join  → worker node


Why do we need to disable the swap?
What is swap , this is when the RAM is full and moves some memory onto the disk , the main reason is because k8 needs predictive memory behaviour and this is because it needs it for the allocation of pods , scaling in or scaling out hence needs to be predictive 


Now the default container run time is containerD, instead of the docker as it has been removed , this is basically to make the pods and handle the containers.

so now runc is the low level that makes the container based on the namespace mounts linux for isolation and etc 
containerd sits above runc and manages the container , handles like pulling images creating containers by calling runc 
containerd
    ↓
"I need to start this container"
    ↓
runc
    ↓
"Okay, I'll create the isolated process"

Componenets to install on each node:
kubectl => not needed on every node just done for convinice 
kubeadm => this is a bootstrap to help with setup , weather it is a CP or WN
kubelet => this is for communication , needed on every node 
runc&contained => needed on every node as each componenet would run as a pod or a contianer

and then crictl will be needs to run on-top of containerd , and this makes the entire same of docker 
like docker ps , docker pull and all that we would use critctl ps and etc

Also make sure to copy the kubeconfig file after setting up the CP

now to get the default , we need to copy the same kubeconfig file in the path of HOME/.kube

Kind is a tool to allow for k8 via the local machine , via docker container nodes

Kind => Kubernetes in Docker 


how to create a cluster:

we can find the type of image we want at this link 
https://github.com/kubernetes-sigs/kind/releases

For the here we use kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8

kind create cluster --image kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8 --name cluster1
If we don't mention the name it will create with the default cluster name of "kind"

we can then use the cluster with the command 
kubectl cluster-info --context kind-cluster1

(base) PS D:\Devops\everything_k8\kind> kind create cluster --image kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8 --name cluster1
Creating cluster "cluster1" ...
 ✓ Ensuring node image (kindest/node:v1.29.4) 🖼 
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-cluster1"
You can now use your cluster with:

kubectl cluster-info --context kind-cluster1

Have a nice day! 👋
(base) PS D:\Devops\everything_k8\kind> kubectl cluster-info --context kind-cluster1
Kubernetes control plane is running at https://127.0.0.1:57280
CoreDNS is running at https://127.0.0.1:57280/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
(base) PS D:\Devops\everything_k8\kind> 


To create a cluster with multiple nodes
we mention what we want via the config file , say 1 control plane and 2 worker nodes 
kind create cluster --image kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8 --name cluster --config config.yml  

(base) PS D:\Devops\everything_k8\kind> kind create cluster --image kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8 --name cluster --config config.yml  
Creating cluster "cluster" ...
 ✓ Ensuring node image (kindest/node:v1.29.4) 🖼 
 ✓ Preparing nodes 📦 📦 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-cluster"
You can now use your cluster with:

kubectl cluster-info --context kind-cluster

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂


To show the list of clusters:
kind get clusters

To delete the cluster 
kind delete cluster --name cluster1 
Deleting cluster "cluster1" ...
Deleted nodes: ["cluster1-control-plane"]


If you have multiple clusters and we need to set the context

kubectl config get-contexts #to display all the contexts available

and the * in front shows the present context

to switch the context 
kubectl config use-context context-name 

Context is really IMPORTANT 

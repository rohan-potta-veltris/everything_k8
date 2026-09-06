Q) Why are we using Kubernetes?

We use Kubernetes as an orchestration tool for all the containers we have. For example, if we have 100 containers and one goes down, how can we manually bring it back up and make sure it is healthy?
If we need to upgrade the application, how will that be done? Kubernetes handles these tasks and more.
Kubernetes also handles making our application accessible to the public.


Q) Is Kubernetes always the solution?
Kubernetes is not always the solution because it can be expensive and waste resources, depending on the number of clusters and nodes we have.

Q) What should be the difference between the Kubectl and your K8 cluster version?
A) The best practice is to always have them at the same version , but if now +/- of the 1 minor version is good
Command:
(base) PS D:\Devops\everything_k8\Selectors> kubectl version                                                                                       
Client Version: v1.32.2                  
Kustomize Version: v5.5.0                                                                                       
Server Version: v1.29.4
WARNING: version difference between client (1.32) and server (1.29) exceeds the supported minor version skew of +/-1
(base) PS D:\Devops\everything_k8\Selectors> 
Shows the server version as well


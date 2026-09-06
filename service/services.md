Services => ClusterIP, NodePort, LoadBalancer And External

to give an endpoint we make use of services.


1. NodePort:
To expose our application externally we use nodeport , range is 13,000 to 32,767
target port is the one where the application runs on example 80
port is the port of the service that we use

Service can be called svc as well

(base) PS D:\Devops\everything_k8\service> kubectl get service
NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1     <none>        443/TCP        32h
nginx-svc    NodePort    10.96.92.60   <none>        80:30001/TCP   2m20s
(base) PS D:\Devops\everything_k8\service> 


(base) PS D:\Devops\everything_k8\service> kubectl get pods -o wide
NAME                                READY   STATUS    RESTARTS   AGE   IP            NODE              NOMINATED NODE   READINESS GATES
nginx-deployment-8565945546-4llf5   1/1     Running   0          42m   10.244.1.12   cluster-worker    <none>           <none>
nginx-deployment-8565945546-92xlh   1/1     Running   0          42m   10.244.1.13   cluster-worker    <none>           <none>
nginx-deployment-8565945546-cbmct   1/1     Running   0          42m   10.244.2.14   cluster-worker2   <none>           <none>
(base) PS D:\Devops\everything_k8\service> kubectl get nodes -o wide
NAME                    STATUS   ROLES           AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION                     CONTAINER-RUNTIME
cluster-control-plane   Ready    control-plane   32h   v1.29.4   172.19.0.4    <none>        Debian GNU/Linux 12 (bookworm)   6.6.87.2-microsoft-standard-WSL2   containerd://1.7.15
cluster-worker          Ready    <none>          32h   v1.29.4   172.19.0.3    <none>        Debian GNU/Linux 12 (bookworm)   6.6.87.2-microsoft-standard-WSL2   containerd://1.7.15
cluster-worker2         Ready    <none>          32h   v1.29.4   172.19.0.5    <none>        Debian GNU/Linux 12 (bookworm)   6.6.87.2-microsoft-standard-WSL2   containerd://1.7.15
(base) PS D:\Devops\everything_k8\service> 
We see where the pod is running on which node and then <node-ip>:30001


But this wont work due to the kind cluster , we would have to add extra kind steps , so make sure to add the cluster details in the file kind-service.yaml


-------------------------
ClusterIP
for every ip that gets re-started or recreated the ip of the said pods changes 
this will have the endpoint of the pods for example frontend so it knows where to go
this is internal to the cluster so no external-ip

-------------
Load Balancer:

say we need to expose 172.168.9.1:30001 , 172.168.9.2:30001 ,172.168.9.3:30001 which are like on different nodes and my application , and this would be done with loadbalancer 


Q) How are LB used for on premise services , and like when do we use it and why?
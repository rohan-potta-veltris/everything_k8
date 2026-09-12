Why do we use namespace?
The namespace is used to like clearly and logically isolate objects and services, by default they are used in the default namespace

(base) PS D:\Devops\everything_k8\namespace> kubectl get namespace
NAME                 STATUS   AGE
default              Active   15h
kube-node-lease      Active   15h
kube-public          Active   15h
kube-system          Active   15h #the k8 default services are made in here
local-path-storage   Active   15h
(base) PS D:\Devops\everything_k8\namespace> 

kubectl get all --namespace namespace_name to get the k8 objects the namespace 
kubectl get all -n namespace_name


A Service in one namespace can access the services inside the same namespace with the hostname.
But for services with different namespaces would need to be accessed via FQDN (Fully Qualified Domain Name) and not only with the hostname 

Creating a new namespace
we created with the help of a yaml file , and applied it 

(base) PS D:\Devops\everything_k8\namespace> kubectl apply -f .\namespace.yaml
namespace/demo created
(base) PS D:\Devops\everything_k8\namespace> kubectl get namespaces
NAME                 STATUS   AGE
default              Active   15h
demo                 Active   <invalid>
kube-node-lease      Active   15h
kube-public          Active   15h
kube-system          Active   15h
local-path-storage   Active   15h
(base) PS D:\Devops\everything_k8\namespace> 

kubectl delete -f namespace.yaml
(base) PS D:\Devops\everything_k8\namespace> kubectl get ns        
NAME                 STATUS   AGE
default              Active   15h
demo                 Active   <invalid>
kube-node-lease      Active   15h
kube-public          Active   15h
kube-system          Active   15h
local-path-storage   Active   15h
(base) PS D:\Devops\everything_k8\namespace> 

kubectl create ns namespace_name 

(base) PS D:\Devops\everything_k8\namespace> kubectl get pods --namespace demo
NAME                           READY   STATUS    RESTARTS   AGE
nginx-deploy-d845cc945-hx7mk   1/1     Running   0          2m10s
(base) PS D:\Devops\everything_k8\namespace> 

(base) PS D:\Devops\everything_k8\namespace> kubectl get pods -n demo         
NAME                           READY   STATUS    RESTARTS   AGE
nginx-deploy-d845cc945-hx7mk   1/1     Running   0          3m11s
(base) PS D:\Devops\everything_k8\namespace> 


To check if a pod in one ns reach the other , so in this case from ns demo to "default"
to do this we would exec into the pod of ns demo and then check the connectivity


(base) PS D:\Devops\everything_k8\namespace> kubectl get pods -o wide 
NAME                           READY   STATUS    RESTARTS   AGEIP           NODE              NOMINATED NODE   READINESS GATES
nginx-deploy-d845cc945-8xxdw   1/1     Running   0          3m31s10.244.1.4   cluster-worker2   <none>           <none>
(base) PS D:\Devops\everything_k8\namespace> 


(base) PS D:\Devops\everything_k8\namespace> kubectl get pods -o wide -n demo
NAME                           READY   STATUS    RESTARTS   AGE   IP           NODE             NOMINATED NODE   READINESS GATES
nginx-deploy-d845cc945-hx7mk   1/1     Running   0          16m   10.244.2.3   cluster-worker   <none>           <none>
(base) PS D:\Devops\everything_k8\namespace> 


(base) PS D:\Devops\everything_k8\namespace> kubectl exec -it nginx-deploy-d845cc945-hx7mk -n demo -- sh 

if i run the curl the respective ips it works and i get the Hello Nginx page

Now we scale the deployment to 3 pods and then add a service 

kubectl scale --replicas=3 deployment nginx-deploy -n demo
kubectl expose deployment nginx-deploy --name=svc-demo --port=80 -n demo

(base) PS D:\Devops\everything_k8\namespace> kubectl get svc
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
cluster-svc   ClusterIP   10.96.230.193   <none>        80/TCP         16h
kubernetes    ClusterIP   10.96.0.1       <none>        443/TCP        16h
nginx-svc     NodePort    10.96.25.170    <none>        80:30001/TCP   16h
svc-demo      ClusterIP   10.96.151.126   <none>        80/TCP         32s
   

Okay so if i exec into the pod in the -ns demo and try to curl via the service name 
# curl svc-demo
curl: (6) Could not resolve host: svc-demo
#     

hence why you will need to do it via the dns mapping for intra service call , because if i use the curl hostip it will work 

# cat /etc/resolv.conf
search demo.svc.cluster.local svc.cluster.local cluster.local
nameserver 10.96.0.10
options ndots:5
# 
so within this we will need to mention the FQDN here.

curl svc-demo.default.svc.cluster.local and this will work as we mention the svc name and then the FQDN
Imperative is where we use the command line 
Declarative is the way where we write a JSON or YAML and then say that this is the way to mention the desired state.

- Creating an nginx pod imperatively 
(base) PS D:\Devops\everything_k8\pods> kubectl run nginx-pod --image=nginx:latest
pod/nginx-pod created
(base) PS D:\Devops\everything_k8\pods> kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          14s
(base) PS D:\Devops\everything_k8\pods> 

The containers first go into creating and then running 

In the ready 1/1 , means we have 1 container and 1 is ready

To verify the type or apiVersion
(base) PS D:\Devops\everything_k8\pods> kubectl explain pod
KIND:       Pod
VERSION:    v1
This is for all the services like deployment , services and configs etc.


To delete the imperative pod 
kubectl delete pod nginx-pod


To declarative create the pod we make a yaml file 

To create the pod 
kubectl create -f .\declarative.yml 

to create or update 
kubectl apply -f .\declarative.yml 

a way to directly update the pod would be to 

kubectl edit pod pod_name and this opens a vi editor and we can directly make the changes 

kubectl run nginx --image=nginx --dry-run=client -o yaml , this will print the output and shows a dry output of the yaml

kubectl run nginx --image=nginx --dry-run=client -o yaml > pod-new.yaml and this will redirect it to a file and we can see the yaml file and apply it accordingly.

To get information about all the pods 

kubectl describe pod pod-name

to get information as to which node the pod is present 

kubectl get pods -o wide

labels are a way to retrieve them 
kubectl get pods pod_name --show-labels

similarly to get the nodes
kubectl get nodes -o wide
What is multi-container Pod?
we can have a main container which is the app , and with it can be an "init" to help with initialization or a "sidecar/helper" which helps the main app

for example if we need to make a new pod , this will make a init container first and then makes the app container.

These multi-containers are inside the pod and share the resources totally.

There is an image called busybox and sort of like a debugging container


----
(base) PS D:\Devops\everything_k8\multi_container_pod> kubectl get pods             
NAME                  READY   STATUS     RESTARTS   AGE
multi-container-pod   0/1     Init:0/1   0          2m51s
(base) PS D:\Devops\everything_k8\multi_container_pod> 
1 pod is not ready and the init 0/1 is available 


(base) PS D:\Devops\everything_k8\multi_container_pod> kubectl describe pod multi-container-pod 
Name:             multi-container-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             cluster-worker2/172.19.0.3
Start Time:       Sun, 06 Sep 2026 15:22:53 +0530
Labels:           env=demo
Annotations:      <none>
Status:           Pending
IP:               10.244.1.8
IPs:
  IP:  10.244.1.8
Init Containers:
  init-myservice:
    Container ID:  containerd://76220919f387cda8012d573961fbbd3a7d734a0fb189753ebdd987f7bed8b51b
    Image:         busybox:1.28
    Image ID:      docker.io/library/busybox@sha256:141c253bc4c3fd0a201d32dc1f493bcf3fff003b6df416dea4f41046e0f37d47
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
    Args:
      until nslookup myservice.default.svc.cluster.local; do echo waiting for myservice to be up; sleep 2; done
    State:          Running
      Started:      Sun, 06 Sep 2026 15:22:54 +0530
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-dkl2p (ro)
Containers:
  my-app-container:
    Container ID:  
    Image:         busybox:1.28
    Image ID:      
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      echo the app is running && sleep 3600
    State:          Waiting
      Reason:       PodInitializing
    Ready:          False
    Restart Count:  0
    Environment:
      FIRST_NAME:  Rohan
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-dkl2p (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 False 
  Ready                       False 
  ContainersReady             False 
  PodScheduled                True 
Volumes:
  kube-api-access-dkl2p:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  41s   default-scheduler  Successfully assigned default/multi-container-pod to cluster-worker2
  Normal  Pulled     40s   kubelet            Container image "busybox:1.28" already present on machine
  Normal  Created    40s   kubelet            Created container init-myservice
  Normal  Started    40s   kubelet            Started container init-myservice

stuck in waiting due to pod initialization and this is because the service is still not ready and without the init the main pod won't be created at all.


(base) PS D:\Devops\everything_k8> kubectl get pods                                                                                       
NAME                  READY   STATUS     RESTARTS   AGE
multi-container-pod   0/1     Init:0/1   0          4m57s
(base) PS D:\Devops\everything_k8> kubectl logs multi-container-pod
Defaulted container "my-app-container" out of: my-app-container, init-myservice (init)
Error from server (BadRequest): container "my-app-container" in pod "multi-container-pod" is waiting to start: PodInitializing
(base) PS D:\Devops\everything_k8> 

And this is the log of the main container , to see the logs of the init container

(base) PS D:\Devops\everything_k8> kubectl logs multi-container-pod -c init-myservice #the init pod name is init-myservice 
nslookup: can't resolve 'myservice.default.svc.cluster.local'
Server:    10.96.0.10
Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local

(base) PS D:\Devops\everything_k8> kubectl expose deploy nginx-deploy --name myservice 
service/myservice exposed

(base) PS D:\Devops\everything_k8> kubectl get pods -w
NAME                           READY   STATUS     RESTARTS   AGE
multi-container-pod            0/1     Init:0/1   0          10m
nginx-deploy-54754f6bb-kff9v   1/1     Running    0          92s
multi-container-pod            0/1     PodInitializing   0          10m
multi-container-pod            1/1     Running           0          10m


(base) PS D:\Devops\everything_k8> kubectl logs multi-container-pod -c init-myservice
waiting for myservice to be up
Server:    10.96.0.10
Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local

Name:      myservice.default.svc.cluster.local
Address 1: 10.96.116.201 myservice.default.svc.cluster.local
(base) PS D:\Devops\everything_k8> 


To check the env , we need to go into run the printenv for the container
(base) PS D:\Devops\everything_k8> kubectl exec -it multi-container-pod -- printenv                                        
Defaulted container "my-app-container" out of: my-app-container, init-myservice (init)
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=multi-container-pod
FIRST_NAME=Rohan
CLUSTER_SVC_SERVICE_PORT=80

We cant enter the init container because it doesn't exist cause it has finished its job , but if it was still existed we can enter with the command 
kubectl exec -it multi-container-pod -c init-myservice -- sh


Note we cannot add or remove the init containers for an already running pod

now adding a second init container 

(base) PS D:\Devops\everything_k8\multi_container_pod> kubectl get pods -w                               
NAME                           READY   STATUS     RESTARTS   AGE
multi-container-pod            0/1     Init:1/2   0          2s

This means that unless all the init containers are ready the main pod won't come up 
(base) PS D:\Devops\everything_k8\multi_container_pod> kubectl exec -it multi-container-pod -c init-mydb -- sh      
/ # 
I am able to exec into this as the pod hasnt been initialized yet

(base) PS D:\Devops\everything_k8\multi_container_pod> kubectl expose deployment redis-deploy --name mydb --port 80
service/mydb exposed
(base) PS D:\Devops\everything_k8\multi_container_pod> kubectl get pods -w    
NAME                            READY   STATUS     RESTARTS   AGE
multi-container-pod             0/1     Init:1/2   0          3m35s
nginx-deploy-54754f6bb-kff9v    1/1     Running    0          18m
redis-deploy-67df7dd58b-k69zd   1/1     Running    0          79s
multi-container-pod             0/1     PodInitializing   0          3m48s
multi-container-pod             1/1     Running           0          3m49s


After making the redis svc for the mydb the multi-pod container has been created.


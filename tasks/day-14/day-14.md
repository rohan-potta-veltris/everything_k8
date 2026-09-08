Taint both of your worker nodes as below

worker01--> gpu=true:NoSchedule , worker02--> gpu=false:NoSchedule
(base) PS D:\Devops\everything_k8\tasks\day-14> kubectl taint node cluster-worker gpu=true:NoSchedule   
node/cluster-worker tainted
(base) PS D:\Devops\everything_k8\tasks\day-14> kubectl taint node cluster-worker2 gpu=true:NoSchedule
node/cluster-worker2 tainted
(base) PS D:\Devops\everything_k8\tasks\day-14> kubectl taint node cluster-worker2 gpu=false:NoSchedule --overwrite
node/cluster-worker2 modified
(base) PS D:\Devops\everything_k8\tasks\day-14> 
To overwrite an already present taint

  Type     Reason            Age        From               Message
  ----     ------            ----       ----               -------
  Warning  FailedScheduling  <invalid>  default-scheduler  0/3 nodes are available: 1 node(s) had untolerated taint {gpu: false}, 1 node(s) had untolerated taint {gpu: true}, 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }. preemption: 0/3 nodes are available: 3 Preemption is not helpful for scheduling.
(base) PS D:\Devops\everything_k8\tasks\day-14> 
This is upon creating a new pod without any label , and due to taint it is not being assigned 


Create a new pod with the image nginx and see why it's not getting scheduled on worker nodes and control plane nodes.

Create a toleration on the pod gpu=true:NoSchedule to match with the taint on worker01

The pod should be scheduled now on worker01

Delete the taint on the control plane node
(base) PS D:\Devops\everything_k8\tasks\day-14> kubectl taint node cluster-control-plane node-role.kubernetes.io/control-plane:NoSchedule-
this removes the taint on the control plane and now i can scheule work load on this 
(base) PS D:\Devops\everything_k8\tasks\day-14> kubectl get pod redis -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE                    NOMINATED NODE   READINESS GATES
redis   1/1     Running   0          26s   10.244.0.5   cluster-control-plane   <none>           <none>

(base) PS D:\Devops\everything_k8\tasks\day-14> kubectl get pod redis -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE                    NOMINATED NODE   READINESS GATES
redis   1/1     Running   0          26s   10.244.0.5   cluster-control-plane   <none>           <none>

Create a new pod with the image redis , it should be scheduled on control plane node

Add the taint back on the control plane node(the one that was removed)


A taint repels Pods, while removing the taint makes the node available to Pods that don't have a toleration.


(base) PS D:\Devops\everything_k8\tasks\day-14> kubectl taint node cluster-control-plane node-role.kubernetes.io/control-plane:NoSchedule
error: node cluster-control-plane already has node-role.kubernetes.io/control-plane taint(s) with same effect(s) and --overwrite is false
(base) PS D:\Devops\everything_k8\tasks\day-14> kubectl get pod redis -o wide                                                            
NAME    READY   STATUS    RESTARTS   AGE     IP           NODE                    NOMINATED NODE   READINESS GATES
redis   1/1     Running   0          2m14s   10.244.0.5   cluster-control-plane   <none>           <none>
(base) PS D:\Devops\everything_k8\tasks\day-14> 


Adding a taint doesnt evict the already exisitng and running pods on the node 
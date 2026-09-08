Task details
create a pod with nginx as the image and add the node affinity with property requiredDuringSchedulingIgnoredDuringExecution and condition disktype = ssd
after created 


check the status of the pod and see why it is not scheduled

(base) PS D:\Devops\everything_k8\tasks\day-15> kubectl get pod nginx-pod -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP       NODE     NOMINATED NODE   READINESS GATES
nginx-pod   0/1     Pending   0          19s   <none>   <none>   <none>           <none>
(base) PS D:\Devops\everything_k8\tasks\day-15> 

(base) PS D:\Devops\everything_k8\tasks\day-15> kubectl describe pod nginx-pod      
Name:             nginx-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             <none>
Labels:           run=nginx-pod
Annotations:      <none>
Status:           Pending
IP:               
IPs:              <none>
Containers:
  nginx-pod:
    Image:        nginx:latest
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-fpkjx (ro)
Conditions:
  Type           Status
  PodScheduled   False 
Volumes:
  kube-api-access-fpkjx:
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
  Type     Reason            Age        From               Message
  ----     ------            ----       ----               -------
  Warning  FailedScheduling  <invalid>  default-scheduler  0/3 nodes are available: 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }, 2 node(s) didn't match Pod's node affinity/selector. preemption: 0/3 nodes are available: 3 Preemption is not helpful for scheduling.
(base) PS D:\Devops\everything_k8\tasks\day-15> 


add the label to your worker01 node as disktype=ssd and then check the status of the pod
(base) PS D:\Devops\everything_k8\tasks\day-15> kubectl label node cluster-worker disktype=ssd
node/cluster-worker labeled
(base) PS D:\Devops\everything_k8\tasks\day-15> 

It should be scheduled on worker node 1
(base) PS D:\Devops\everything_k8\tasks\day-15> kubectl get pods -o wide
NAME        READY   STATUS    RESTARTS   AGE     IP           NODE             NOMINATED NODE   READINESS GATES
nginx-pod   1/1     Running   0          2m53s   10.244.2.4   cluster-worker   <none>           <none>
(base) PS D:\Devops\everything_k8\tasks\day-15> 

create a new pod with redis as the image and add the nodeaffinity with property requiredDuringSchedulingIgnoredDuringExecution and condition disktype without any value
Schedule the Pod only on a node where the disktype label exists, regardless of whether its value is ssd, hdd, or anything else.


add the label to worker02 node with disktype and no value
ensure that pod2 should be scheduled on worker02 node


Node affinity determines which nodes are eligible; it doesn't necessarily guarantee which eligible node the scheduler will choose.



Say we have one node saying gpu=true and this is like a taint , and when a new pod comes in that we want to schedule it , but only those pods that have gpu=true will be allocated in that node 
the way we make the pod go into that tainted node is by using toleration.
If a pod has a toleration gpu=true then it will be scheduled into that node.
essentially we are only allowing certain some pods into a node.

TAINT is on NODE
TOLERATION is on POD

Toleration has something called "effect" , which happens after it 
1. NoSchedule => works for the newer pods ONLY
2. prefer No Schedule => No guarantee
3. No-Execute => works on all existing and new pods


How to taint a node 

(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl get nodes                                    
NAME                    STATUS   ROLES           AGE   VERSION
cluster-control-plane   Ready    control-plane   2d    v1.29.4
cluster-worker          Ready    <none>          2d    v1.29.4
cluster-worker2         Ready    <none>          2d    v1.29.4
(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl taint node cluster-worker gpu=true:NoSchedule
node/cluster-worker tainted
(base) PS D:\Devops\everything_k8\taints_tolerations> 

node/cluster-worker2 tainted
(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl describe node cluster-worker2                                                
Name:               cluster-worker2         
Roles:              <none>                                       
Labels:             beta.kubernetes.io/arch=amd64                
                    beta.kubernetes.io/os=linux                  
                    kubernetes.io/arch=amd64                     
                    kubernetes.io/hostname=cluster-worker2                                                    
                    kubernetes.io/os=linux
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: unix:///run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sat, 05 Sep 2026 20:44:21 +0530
Taints:             gpu=true:NoSchedule



(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl get pods       
NAME   READY   STATUS    RESTARTS   AGE
pod    0/1     Pending   0          11s
(base) PS D:\Devops\everything_k8\taints_tolerations> 

because there are no available nodes 


(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl get pods       
NAME   READY   STATUS    RESTARTS   AGE
pod    0/1     Pending   0          11s
(base) PS D:\Devops\everything_k8\taints_tolerations> 

  tolerations:
    - key: "gpu"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule" # why is this mentioned?
      this is how the toleration is written 


      (base) PS D:\Devops\everything_k8\taints_tolerations> kubectl get pods             
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          4s
(base) PS D:\Devops\everything_k8\taints_tolerations> 

To undo a taint in a node 
(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl taint node cluster-worker2 gpu=true:NoSchedule- 
node/cluster-worker2 untainted
(base) PS D:\Devops\everything_k8\taints_tolerations> 

add a - at the end

Note this does not guarantee that it won't be on a particular pod, meaning if gpu=true was put on worker node 2, and the scheduler sends it there, then it would directly be placed on that node 
So to better proof this we use "selectors"

NodeSelector is like label and give the pod the choice to match the label to pod-label


to label the nodes 

(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl label node cluster-worker2 gpu=true
node/cluster-worker2 labeled
(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl get nodes --show-labels
NAME                    STATUS   ROLES           AGE   VERSION   LABELS
cluster-control-plane   Ready    control-plane   2d    v1.29.4   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=cluster-control-plane,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node.kubernetes.io/exclude-from-external-load-balancers=
cluster-worker          Ready    <none>          2d    v1.29.4   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=cluster-worker,kubernetes.io/os=linux
cluster-worker2         Ready    <none>          2d    v1.29.4   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,gpu=true,kubernetes.io/arch=amd64,kubernetes.io/hostname=cluster-worker2,kubernetes.io/os=linux
(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl label node cluster-worker2 gpu=false
error: 'gpu' already has a value (true), and --overwrite is false
(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl label node cluster-worker2 gpu=false --overwrite
node/cluster-worker2 labeled
(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl get nodes --show-labels                         
NAME                    STATUS   ROLES           AGE   VERSION   LABELS
cluster-control-plane   Ready    control-plane   2d    v1.29.4   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=cluster-control-plane,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node.kubernetes.io/exclude-from-external-load-balancers=
cluster-worker          Ready    <none>          2d    v1.29.4   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=cluster-worker,kubernetes.io/os=linux
cluster-worker2         Ready    <none>          2d    v1.29.4   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,gpu=false,kubernetes.io/arch=amd64,kubernetes.io/hostname=cluster-worker2,kubernetes.io/os=linux
(base) PS D:\Devops\everything_k8\taints_tolerations> 


(base) PS D:\Devops\everything_k8\taints_tolerations> kubectl get pods -w
NAME            READY   STATUS    RESTARTS   AGE
nginx-pod       1/1     Running   0          15m
nginx-pod-new   1/1     Running   0          4m52s

And now the pod is labelled


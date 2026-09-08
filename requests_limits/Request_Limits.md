Requests and Limits

When the nodes are full with all the pods, then any new future pods will have the error called "insufficient resources"

When some pods dont have a limit , and the load increases the said pod can exceed what the node can handle and this will throw and error of OOM (out of memory)

and all future pods will also get the same oom 

And for this reason we give the pods or containers limits


This metrics.yaml gets like some metrics from teh objects 

(base) PS D:\Devops\everything_k8\requests_limits> kubectl get pods -n kube-system
NAME                                            READY   STATUS    RESTARTS      AGE
coredns-76f75df574-wh7ct                        1/1     Running   2 (12h ago)   3d1h
coredns-76f75df574-z5r5s                        1/1     Running   2 (12h ago)   3d1h
etcd-cluster-control-plane                      1/1     Running   0             12h
kindnet-ct6lg                                   1/1     Running   4 (12h ago)   3d1h
kindnet-x5w2f                                   1/1     Running   4 (12h ago)   3d1h
kindnet-zvq9m                                   1/1     Running   4 (12h ago)   3d1h
kube-apiserver-cluster-control-plane            1/1     Running   0             12h
kube-controller-manager-cluster-control-plane   1/1     Running   3 (8h ago)    3d1h
kube-proxy-59cxc                                1/1     Running   2 (12h ago)   3d1h
kube-proxy-nvrfn                                1/1     Running   2 (12h ago)   3d1h
kube-proxy-pzbj4                                1/1     Running   2 (12h ago)   3d1h
kube-scheduler-cluster-control-plane            1/1     Running   3 (8h ago)    3d1h
metrics-server-67fc4df55-wk85s                  1/1     Running   0             69s
(base) PS D:\Devops\everything_k8\requests_limits> 

And they run here and for example

(base) PS D:\Devops\everything_k8\requests_limits> kubectl top node
NAME                    CPU(cores)   CPU(%)   MEMORY(bytes)   MEMORY(%)   
cluster-control-plane   138m         1%       598Mi           7%          
cluster-worker          32m          0%       156Mi           2%          
cluster-worker2         39m          0%       200Mi           2%          
(base) PS D:\Devops\everything_k8\requests_limits> 

This is done by the metrics.yaml

kubectl create namespace memory-example

Limits and requests come for the contianer of the pod 

They can be for memory or CPU , the limit or request

Mi=> almost MB


requests: #this is what will be allocated to pod 
limits: #this is the max size and if exceeded it will kill the node


its better to fail the pod than the entire node all togther

and when a pod asks for more limit than what a node can give , the node will be left in pending.


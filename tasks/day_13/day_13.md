Create a pod and try to schedule it manually without the scheduler.

| Method              | Scheduler involved? | How placement happens              |
| ------------------- | ------------------- | ---------------------------------- |
| Normal Pod          | ✅ Yes               | Scheduler chooses node             |
| Pod with `nodeName` | ❌ No                | You directly specify the node      |
| Static Pod          | ❌ No                | Kubelet creates it on its own node |


nodeName is directly passing the scheduler, whereas the nodeSelector still uses the scheduler

Login to the control plane node and go to the directory of default static pod manifests and try to restart the control plane components
To log into the node, we can't use exec as it is only used for pods, so to log into the control plane we will use SSH, or in kind we would have to use docker exec.

(base) PS D:\Devops\everything_k8\tasks\day-13> docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED      STATUS          PORTS                                                 NAMES
3b7f644a265d   kindest/node:v1.29.4   "/usr/local/bin/entr…"   2 days ago   Up 35 minutes   0.0.0.0:30001->30001/tcp, 127.0.0.1:50436->6443/tcp   cluster-control-plane
864c68ff0e05   kindest/node:v1.29.4   "/usr/local/bin/entr…"   2 days ago   Up 35 minutes                                                         cluster-worker2
ec8f68fa9b24   kindest/node:v1.29.4   "/usr/local/bin/entr…"   2 days ago   Up 35 minutes     

(base) PS D:\Devops\everything_k8\tasks\day-13> docker exec -it cluster-control-plane sh    
# 


# pwd
/etc/kubernetes
# ls
admin.conf  controller-manager.conf  kubelet.conf  manifests  pki  scheduler.conf  super-admin.conf
# pwd
/etc/kubernetes/manifests
# ls
etcd.yaml  kube-apiserver.yaml  kube-controller-manager.yaml  kube-scheduler.yaml
# 

So these files are being constantly monitored. We don't have to run apply; even a small empty line change will restart the component, and moving them outside will also take the pod/object down.


Create 3 pods with the name as pod1, pod2 and pod3 based on the nginx image and use labels as env:test, env:dev and env:prod for each of these pods respectively.



Then using the kubectl commands, filter the pods that have labels dev and prod.


labels have the following conditions expected: in, notin, =, ==, !=, gt, lt
(base) PS D:\Devops\everything_k8\tasks\day-13> kubectl get pods -l 'env in (dev,prod)'
NAME   READY   STATUS    RESTARTS   AGE
pod2   1/1     Running   0          88s
pod3   1/1     Running   0          65s

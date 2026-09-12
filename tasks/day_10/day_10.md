(base) PS D:\Devops\everything_k8\tasks\day-10> kubectl create ns ns1
namespace/ns1 created
(base) PS D:\Devops\everything_k8\tasks\day-10> kubectl create ns ns2
namespace/ns2 created
(base) PS D:\Devops\everything_k8\tasks\day-10> kubectl get ns           
NAME                 STATUS   AGE
default              Active   44h
demo                 Active   29h
kube-node-lease      Active   44h
kube-public          Active   44h
kube-system          Active   44h
local-path-storage   Active   44h
ns1                  Active   5s
ns2                  Active   3s
(base) PS D:\Devops\everything_k8\tasks\day-10> 


(base) PS D:\Devops\everything_k8\tasks\day-10> kubectl get deployment -n ns1
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
deployment-ns1   1/1     1            1           40s
(base) PS D:\Devops\everything_k8\tasks\day-10> kubectl get deployment -n ns2
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
deployment-ns2   1/1     1            1           51s
(base) PS D:\Devops\everything_k8\tasks\day-10> 


How to save the context of the namespace?

(base) PS D:\Devops\everything_k8\tasks\day-10> kubectl get pods -o wide -n ns1
NAME                            READY   STATUS    RESTARTS   AGE    IP            NODE              NOMINATED NODE   READINESS GATES
deployment-ns1-bc7dd9d4-85d58   1/1     Running   0          119s   10.244.1.24   cluster-worker2   <none>           <none>
(base) PS D:\Devops\everything_k8\tasks\day-10> kubectl get pods -o wide -n ns2
NAME                            READY   STATUS    RESTARTS   AGE     IP            NODE              NOMINATED NODE   READINESS GATES
deployment-ns2-bc7dd9d4-lvb5n   1/1     Running   0          2m12s   10.244.1.23   cluster-worker2   <none>           <none>
(base) PS D:\Devops\everything_k8\tasks\day-10> 


FQDN Names= <service-name>.<namespace>.svc.cluster.local
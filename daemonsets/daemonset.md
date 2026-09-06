We will be looking at daemonsets ,jobs ,chron jobs

Daemon set will make the replicas across the nodes

Example a monitoring-agent (MA)
This would need to be in every node , so if we have 3 nodes , it will make 3 MA, and when we add a new node it will automatically make a new MA 
and again once the node is deleted the replica will also be deleted again.

Some kube-proxy CNI => weave-net , flannel and calico are all like deployed as daemonset , cause each and every one of them would need to have the kube-proxy.

they are similar to the deployment

ITS IMPORTANT TO NOTE THAT ONLY ONE POD PER DAEMON SET ON EACH NODE 

also note that contorl plan is not counted, so daemon set will only be on the worker nodes

(base) PS D:\Devops\everything_k8\daemonsets> kubectl get ds -A 
NAMESPACE     NAME               DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
default       nginx-deployment   2         2         2       2            2           <none>                   81s
kube-system   kindnet            3         3         3       3            3           kubernetes.io/os=linux   19h
kube-system   kube-proxy         3         3         3       3            3           kubernetes.io/os=linux   19h
(base) PS D:\Devops\everything_k8\daemonsets> 

To get from all the name spaces -A

Chorn Job => when we wanto shedule at a specific time period repeatedly 

under the spec we add 
schedule = "* * * * *" (min, hour, day_of_month, month,day_of_month)
day_of_month = sun(0) to sat(6)

so say we want to run every 5 mins => */5 * * * *
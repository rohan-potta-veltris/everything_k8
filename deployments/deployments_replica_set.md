Replication is the spins up multiple identical instances of a pod

So say for example if a pod fails or if we need high availability via scaling, the ReplicaSet will make more pods


The ReplicationController is in charge of ensuring that the replicas are running and nothing is crashing, even if there is a pod failure.

The replication controller also acts as a Load Balancer and this directs the traffic based on the replicas it has created.

The only way to make a pod self healing , by that i mean if a pod fails to make another one is by replica set.

By default, if a pod I make dies or is deleted, it won't make a new one; the most Kubernetes would do is restart the pod.

REPLICATION CONTROLLER:
Again using kubectl explain rc (ReplicationController), we get the apiVersion and kind

(base) PS D:\Devops\everything_k8\deployments> kubectl apply -f .\replication_controller.yaml
replicationcontroller/nginx-rc created
(base) PS D:\Devops\everything_k8\deployments> kubectl get pods
NAME             READY   STATUS              RESTARTS   AGE
nginx-rc-59dx9   1/1     Running             0          3s
nginx-rc-l6jqr   0/1     ContainerCreating   0          3s
nginx-rc-nf4f2   0/1     ContainerCreating   0          3s
(base) PS D:\Devops\everything_k8\deployments> 


(base) PS D:\Devops\everything_k8\deployments> kubectl get rc
NAME       DESIRED   CURRENT   READY   AGE
nginx-rc   3         3         3       23s
(base) PS D:\Devops\everything_k8\deployments> 

base) PS D:\Devops\everything_k8\deployments> kubectl get rc
NAME       DESIRED   CURRENT   READY   AGE
nginx-rc   3         3         3       23s
(base) PS D:\Devops\everything_k8\deployments> kubectl describe rc
Name:         nginx-rc
Namespace:    default
Selector:     env=demo,type=nginx
Labels:       env=demo
Annotations:  <none>
Replicas:     3 current / 3 desired
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  env=demo
           type=nginx
  Containers:
   nginx:
    Image:         nginx:latest
    Port:          80/TCP
    Host Port:     0/TCP
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:
  Type    Reason            Age        From                    Message
  ----    ------            ----       ----                    -------
  Normal  SuccessfulCreate  <invalid>  replication-controller  Created pod: nginx-rc-nf4f2
  Normal  SuccessfulCreate  <invalid>  replication-controller  Created pod: nginx-rc-l6jqr
  Normal  SuccessfulCreate  <invalid>  replication-controller  Created pod: nginx-rc-59dx9
(base) PS D:\Devops\everything_k8\deployments> 


-----------
Replication Controller is the old version, and the ReplicaSet is the new way. The controller is used to manage the pods as part of that controller, but with the set we can manage already existing and created pods, and this is done with the help of the "Selector" and called "matchLabels" that will be inside the selector and will be managed by the set
The point is they needed a way to have in control of the MatchLabel and MatchExpression so they added this feature 

We learned that a ReplicaSet maintains the number of Pods based on its replica count and selector.

Initially, we had 3 Pods created by the ReplicationController and 3 Pods created by the ReplicaSet, so there were 6 Pods.

All of them had the label env=demo, so kubectl get pods -l env=demo showed all 6.

Then we created another Pod manually with the same label env=demo. Since it matched the ReplicaSet selector, the ReplicaSet considered it as part of the Pods it manages. The ReplicaSet wanted only 3 Pods, so it adjusted the number back to 3.

After deleting the old ReplicationController, we had only the 3 ReplicaSet Pods.

Then we deleted those Pods and created a normal Pod with env=demo first. After that, we created the ReplicaSet with replicas: 3. The ReplicaSet saw that one matching Pod already existed, so it created only 2 more Pods.

The main thing we learned is:

Labels identify Pods.
The selector tells the ReplicaSet which Pods match.
Replicas tells the ReplicaSet how many matching Pods should exist.
The ReplicaSet continuously tries to maintain that number.

This showed us the self-healing behavior of a ReplicaSet.

We had 3 Pods running.

Then we manually deleted one Pod using:

kubectl delete -f ..\pods\declarative.yaml

Now only 2 Pods were left.

The ReplicaSet noticed that it wanted 3 Pods, so it automatically created a new Pod.

Now we had 3 Pods again.

Simple way to remember:

replicas: 3 → ReplicaSet always wants 3 Pods.

If one Pod is deleted → only 2 Pods remain.

ReplicaSet notices this → creates 1 new Pod.

Now there are 3 Pods again.

Main point: A ReplicaSet automatically replaces a deleted or failed Pod to maintain the required number of Pods.


--------------------
make the replicas 3 to 5
(base) PS D:\Devops\everything_k8\deployments> kubectl edit rs nginx-rs
replicaset.apps/nginx-rs edited
(base) PS D:\Devops\everything_k8\deployments> kubectl get pods        
NAME             READY   STATUS              RESTARTS   AGE
nginx-rs-4v4wx   1/1     Running             0          3m50s
nginx-rs-b9tzt   1/1     Running             0          3s
nginx-rs-hmh6g   0/1     ContainerCreating   0          3s
nginx-rs-q8bg7   1/1     Running             0          113s
nginx-rs-rxhkb   1/1     Running             0          3m50s
(base) PS D:\Devops\everything_k8\deployments> 
manual edit and update

(base) PS D:\Devops\everything_k8\deployments> kubectl scale --replicas=5 rs/nginx-rs
replicaset.apps/nginx-rs scaled
(base) PS D:\Devops\everything_k8\deployments> kubectl get pods                      
NAME             READY   STATUS    RESTARTS   AGE
nginx-rs-27b88   1/1     Running   0          3s
nginx-rs-4v4wx   1/1     Running   0          5m47s
nginx-rs-q8bg7   1/1     Running   0          3m50s
nginx-rs-ql2sb   1/1     Running   0          3s
nginx-rs-rxhkb   1/1     Running   0          5m47s
(base) PS D:\Devops\everything_k8\deployments> 


--------------------------------
Deployment:
this provides some additional features on-top of rs

So Deployment -> replica-set -> pods

say we need to update from 1.1 -> 1.2 , the replica set would directly delete everything and up them again so we get a downtime , so this is one of the reasons with deployment , and we mention the number of replicas in the deployment.

this will have like rolling out methods and also rollback and stuff


(base) PS D:\Devops\everything_k8\deployments> kubectl apply -f .\deployment.yaml                 
deployment.apps/nginx-deployment unchanged
(base) PS D:\Devops\everything_k8\deployments> kubectl get pods                 
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-8565945546-cnr64   1/1     Running   0          17s
nginx-deployment-8565945546-djjwq   1/1     Running   0          17s
nginx-deployment-8565945546-dsdgn   1/1     Running   0          17s
(base) PS D:\Devops\everything_k8\deployments> kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           23s
(base) PS D:\Devops\everything_k8\deployments> kubectl get rc    
No resources found in default namespace.
(base) PS D:\Devops\everything_k8\deployments> kubectl get rs
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-8565945546   3         3         3       29s
(base) PS D:\Devops\everything_k8\deployments> 

We can see that the deployment made the replicaset

(base) PS D:\Devops\everything_k8\deployments> kubectl set image deploy/nginx-deploy nginx=nginx:1.9.1
to manually change the image type 
(base) PS D:\Devops\everything_k8\deployments> kubectl set image deploy/nginx-deployment nginx=nginx:1.9.1
deployment.apps/nginx-deployment image updated
(base) PS D:\Devops\everything_k8\deployments> kubectl describe deploy nginx-deployment
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Sat, 05 Sep 2026 19:38:39 +0530
Labels:                 env=demo
Annotations:            deployment.kubernetes.io/revision: 2
Selector:               env=demo
Replicas:               3 desired | 1 updated | 4 total | 4 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  env=demo
           type=nginx
  Containers:
   nginx:
    Image:         nginx:1.9.1
    Port:          80/TCP
    Host Port:     0/TCP
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  nginx-deployment-8565945546 (2/2 replicas created)
NewReplicaSet:   nginx-deployment-6d7475c7d8 (2/2 replicas created)
Events:
  Type    Reason             Age        From                   Message
  ----    ------             ----       ----                   -------
  Normal  ScalingReplicaSet  <invalid>  deployment-controller  Scaled up replica set nginx-deployment-8565945546 to 3
  Normal  ScalingReplicaSet  <invalid>  deployment-controller  Scaled up replica set nginx-deployment-6d7475c7d8 to 1
  Normal  ScalingReplicaSet  <invalid>  deployment-controller  Scaled down replica set nginx-deployment-8565945546 to 2 from 3
  Normal  ScalingReplicaSet  <invalid>  deployment-controller  Scaled up replica set nginx-deployment-6d7475c7d8 to 2 from 1
(base) PS D:\Devops\everything_k8\deployments> 
We can see the image change

To see the deploy changes:
(base) PS D:\Devops\everything_k8\deployments> kubectl rollout history deploy nginx-deployment
deployment.apps/nginx-deployment
REVISION  CHANGE-CAUSE
1         <none>
2         <none>

(base) PS D:\Devops\everything_k8\deployments> 


rollout:

(base) PS D:\Devops\everything_k8\deployments> kubectl rollout undo deploy/nginx-deployment   
deployment.apps/nginx-deployment rolled back
(base) PS D:\Devops\everything_k8\deployments> kubectl rollout history deploy nginx-deployment            
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
2         <none>
3         <none>

(base) PS D:\Devops\everything_k8\deployments> kubectl describe deploy nginx-deployment                   
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Sat, 05 Sep 2026 19:38:39 +0530
Labels:                 env=demo
Annotations:            deployment.kubernetes.io/revision: 3
Selector:               env=demo
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  env=demo
           type=nginx
  Containers:
   nginx:
    Image:         nginx:latest
    Port:          80/TCP
    Host Port:     0/TCP
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  nginx-deployment-6d7475c7d8 (0/0 replicas created)
NewReplicaSet:   nginx-deployment-8565945546 (3/3 replicas created)
Events:
  Type    Reason             Age                            From                   Message
  ----    ------             ----                           ----                   -------
  Normal  ScalingReplicaSet  75s                            deployment-controller  Scaled up replica set nginx-deployment-8565945546 to 3
  Normal  ScalingReplicaSet  <invalid>                      deployment-controller  Scaled up replica set nginx-deployment-6d7475c7d8 to 1
  Normal  ScalingReplicaSet  <invalid>                      deployment-controller  Scaled down replica set nginx-deployment-8565945546 to 2 from 3
  Normal  ScalingReplicaSet  <invalid>                      deployment-controller  Scaled up replica set nginx-deployment-6d7475c7d8 to 2 from 1
  Normal  ScalingReplicaSet  <invalid>                      deployment-controller  Scaled down replica set nginx-deployment-8565945546 to 1 from 2
  Normal  ScalingReplicaSet  <invalid>                      deployment-controller  Scaled up replica set nginx-deployment-6d7475c7d8 to 3 from 2
  Normal  ScalingReplicaSet  <invalid>                      deployment-controller  Scaled down replica set nginx-deployment-8565945546 to 0 from 1
  Normal  ScalingReplicaSet  <invalid>                      deployment-controller  Scaled up replica set nginx-deployment-8565945546 to 1 from 0
  Normal  ScalingReplicaSet  <invalid>                      deployment-controller  Scaled down replica set nginx-deployment-6d7475c7d8 to 2 from 3
  Normal  ScalingReplicaSet  <invalid> (x4 over <invalid>)  deployment-controller  (combined from similar events): Scaled down replica set nginx-deployment-6d7475c7d8 to 0 from 1
(base) PS D:\Devops\everything_k8\deployments> 

rolling out made a whole new revision , but the image is still the same and as expected



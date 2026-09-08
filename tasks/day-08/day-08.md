How to scale out to replica size for a replicaset 
(base) PS D:\Devops\everything_k8\tasks\day-08> kubectl scale rs/nginx-rs --replicas=6


Day-08
So for a deployment it is necessary to have a match labels condition just like how it is in for the replica set

Updating the image

Change the image in the yaml and applied and see a new rollout is added 
(base) PS D:\Devops\everything_k8\tasks\day-08> kubectl rollout history  deploy/nginx
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>

(base) PS D:\Devops\everything_k8\tasks\day-08> kubectl rollout status deployment/nginx
deployment "nginx" successfully rolled out
Status is also successful 
And describe the pod and see the updated image

the only way to see the pods of a deployment is to use the labels 


(base) PS D:\Devops\everything_k8\tasks\day-08> kubectl annotate deployment nginx kubernetes.io/change-cause="Pick up patch version"
deployment.apps/nginx annotated
(base) PS D:\Devops\everything_k8\tasks\day-08> kubectl rollout history deployment/nginx
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
1         <none>
2         Pick up patch version

(base) PS D:\Devops\everything_k8\tasks\day-08> 
And this will only be there for the most recent revision, or the top most one 


(base) PS D:\Devops\everything_k8\tasks\day-08> kubectl scale deploy/nginx --replicas=6
deployment.apps/nginx scaled
used to scale the deployments

(base) PS D:\Devops\everything_k8\tasks\day-08> kubectl rollout undo deployment/nginx
deployment.apps/nginx rolled back
(base) PS D:\Devops\everything_k8\tasks\day-08> 
undo the revision by 1 

kubectl rollout undo deployment/nginx --to-revision=1
To go to a specific revison
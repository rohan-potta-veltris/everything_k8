Scaling is by changing the workload based on the demand.

autoscaling based on the load of the server we would increase or decrease the workload of ours.

Horizontal Pod Scaling is adding a new or removing pods (Scale Out/In)

Vertical Pod Scaling is increasing or decreasing the resources of the pod, this will require some downtime such as a restart as we increase the size of the workload, the memory or the CPU(Scale Up/Down)

Horizontal Workload Pods => HPA (horizontal pod autoscaling , takes metrics from the metrics server)
Only HPA is k8 native


Horizontal Infra Nodes => Cluster Autoscaler
Vertical Workload Pods => VPA (vertical pod autoscaling , takes metrics from the metrics server)
Vertical Infra Nodes => Node AutoProvisioning 
These are 3rd party or like a cloud tool


imperative 

kubectl autoscale deploy deployment_name --cpu-percent=50 --min=1 --max=3

How to set the interval it checks and what are all the metrics i can use for the scaling?



(base) PS D:\Devops\everything_k8\autoscaling> kubectl get hpa -w
NAME         REFERENCE               TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
php-apache   Deployment/php-apache   <unknown>/50%   1         10        0          15s
php-apache   Deployment/php-apache   0%/50%          1         10        1          16s

To increase the load we can use 

kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"


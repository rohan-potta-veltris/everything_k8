Nodeport and LoadBalancer are two service types to expose the application to the public 

Nodeport is usually for internal purposes
For customers we used loadbalancer 

Now the problems of loadbalancer is that it depends on the cloud provider,
this is costly , and also depends on the cloud provider , for example if it cant be integrated with the cluster , then problem
security issues will be present , as we have to use the cloud provider choice 
cant use like nginx, paloalto etc

This is why we use ingress , this means the inward traffic 

The ingress controller , will read the ingress rules written in yaml manifest 
This ingress controller will be read by the loadbalancer , like nginx 

Note service only provides a Round Robin load balancing 

ingress controller is made with the helm charts or a manifest file.

So the ingress controller works once the ingress rules is set and then the ingress controller will make and set the rules for the load-balancing 

crictl is used to like troubleshoot the errors in the container runtime itself
 
Ingress class name is needed in the ingress controller part
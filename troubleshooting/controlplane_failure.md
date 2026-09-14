so say for example , kubectl get nodes and you get connection refused?
chances are that the api server is down , this is a static pod , so we go into the master node , and we check if the pod is running and we can check with crictl ps , and see if api-server is running 
the manifest will be present in /etc/kubernetes/manifests

we can check the logs of the exited container as well.
there could be a chance there is an error in the kubeconfig file as well.

and we can find it at the path of /etc/kubernetes/admin.conf

If a pod is left at pending chances are the reason is because of the scheduler


So now say for example we have a deployment with 2 pods , but then i delete one and then it doesn't get recreated , but the deployment shows 2/2 , this is a fault with the controller as this handles all the controller manager of the cluster 
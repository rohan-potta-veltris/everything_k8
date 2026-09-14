CoreDNS in k8

there is a pod deployed via coredns , and if we can reach with ip and not hostname that means there is something wrogn with this 

(base) PS D:\Devops\everything_k8\DNS> kubectl get deploy -n=kube-system
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
coredns   2/2     2            2           6h44m
(base) PS D:\Devops\everything_k8\DNS> clear

and this handles the DNS resolution
coredns is the deployment name , kube-dns is the servive name for teh dns 

there is a configmap as well which has a few plugins and so on

This only works on the network add on and depends on that, basically the CNI 

So now when i make a service the dns is already added to the dns system of the cluster and then now when it comes to the need for this is because the ip of the pod is temperary hence why i would need an internal dns system 

and when i expose my application to the public what i would do is that i need a dns mapping that comes to my loadbalancer or the nodeport being exposed.

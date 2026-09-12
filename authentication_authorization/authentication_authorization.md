when we do kubectl get pods , how doest he autorization happen?
There is a file called kubeconfig , which has the details of like the server the permissions who am i and stuff

kubectl get pods --kubeconfig config_file

the default file is in the home diretctory 

(base) PS C:\Users\Rohan\.kube> pwd

Path                
----                
C:\Users\Rohan\.kube


(base) PS C:\Users\Rohan\.kube> ls


    Directory: C:\Users\Rohan\.kube


Mode                 LastWriteTime         Length Name                                                                                    
----                 -------------         ------ ----                                                                                    
d-----        26-05-2026  12:33 PM                cache                                                                                   
-a----        05-09-2026  08:41 PM           5616 config                                                                                  


This is the config file being taken in by default 


Clusters → Which Kubernetes cluster do I want to connect to?
Users → Who am I / what credentials should I use?
Contexts → Which user should I use to connect to which cluster?
A context connects a cluster + user + optionally a namespace.

ideally we would want multiple configs when we want to logically isolate , for example project wise and within it we can mention dev , qa and prod and so on

Authntication is who are you , done via certificates and encrytpion
Authorization is what can you do , done via ABAC RBAC NODE Webhook

ABAC → Attribute-Based Access Control => this requires the API server to be restarted after the plicy has been assigned 
RBAC → Role-Based Access Control => this dynamic like iam in aws 
NODE => used for nodes to interact with each other 


In the APIServer there is a term called -authorization-strings in the manifest file and if this is not mentioned it will default to AlwaysAllow, not the apiserver manifest file is in the control plane in the path of /etc/kubernetes/manifests and this 

and in /etc/kubernetes/pki have all the certificates mentioned
we can have multiple certificates for the api-server , because this acts as client and server
server when we make a request to other components like kubeproxy kubelet etc
And client when we as user hit the api-server/




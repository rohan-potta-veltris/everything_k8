Now the scheduler is the one that handles the creation of other pods , but now the scheduler itself is a pod , now how does this get created.
This is done via the "Static Pods". These are the first few initially created ones and usually the control plane component pods.

So the way these static pods are made is that in the control plane VM, the path /etc/kubernetes/manifests/
the manifest files must be mentioned and the ones in there will be created as pods , 
if the manifest file is moved then in that case the pods will also be terminated 

To tell the pod on which node it has to go on is that in the spec by the tag 

spec:
  containers:
  - image: nginx:latest
    name: nginx-pod
  nodeName: cluster-worker and it will go into that node

this essentially by-passes the scheduler and will make the pod despite it.

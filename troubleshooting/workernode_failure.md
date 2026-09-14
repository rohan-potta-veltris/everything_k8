so say if all the nodes are not-ready it's possible it's a problem with the networking of CNI 
to check kubectl get pods in the namespace of the CNI


if the worker nodes are not ready then it's a problem for the specific node , maybe it's possible there is something wrong with the kubelet 
service kubelet status     
systemctl status kubelet

journalctl -u kubelet this is used to check the logs of the systemctl service.


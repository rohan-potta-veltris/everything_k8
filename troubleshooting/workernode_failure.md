so say if all the nodes are not-ready its possible its a probelme with the networking of CNI 
to check kubectl get pods in the namespace of the CNI


if the worker nodes are not ready then its a problem for the specific node , maybe its poswible there is something wrong with the kubelet 
service kubelet status     
systemctl status kubelet

journalctl -u kubelet this is used to checkt the logs of the systemctl service.


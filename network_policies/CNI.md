Kubernetes Netowkring Explained:


Docker also uses contanierd , and this just checkst the life-cycle of the containers

Docker Run -> Daemon -> containerd -> shim (connectivity is maintained) -> runc (starts the container)

runc is the OCI compliant which the k8 uses 

so for k8

user -> apiserver/scheduler to make node -> kubelet (to make the container) -> calls Crio -> crio -> continerd -> shim -> runc nad makes the pod 

CNI -> is the network implementation used as a plug and play for what networking we want and thats why it doesnt come default with the kubeadm 
CRI -> container runtime interface 

CRI-O is a container runtime specifically designed for Kubernetes.



So like within a pod the reason why things are like able to communicate with localhost depsite the contianers being changed is because of the presence of a new pod called 'pause' which allows contianers in pod communicate with "localhost"
pause holds the network namespace
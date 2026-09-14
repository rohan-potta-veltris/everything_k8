This doesn't have monitoring , so we make a pod for metric_server and can be done with applying their manifest file 
https://github.com/kubernetes-sigs/metrics-server

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml


So the container -> sends metrics to cAdvisor -> talks to kubelet -> sends to metric-server -> sends to api-Server -> handles the HPA

HPA = Horizontal Pod Autoscaler.
Also note there will be certificates needed if not we will need to use -kubelet-insecure-tls in the edit of the metric-server deployment
what is json?
JSON stands for JavaScript Object Notation

where is this used?
used in etcd , because this is a key value pair database , via json

user -> kubectl -> api server -> response in json -> kubectl makes it human readable -> our nice output 

to see the entire json method
kubectl get nodes -o json # shows the full output , same even if we used -o yaml

So we need to query with the jsons

so to parse it ,

now when we need to parse it we need to use the output of the get pods, cause it might be different from what is in the run pod .

(base) PS D:\Devops\everything_k8\jsons> kubectl get pods -o=jsonpath='{$.items[*].metadata.labels.run}'
pod
(base) PS D:\Devops\everything_k8\jsons> 
and to format it (base) PS D:\Devops\everything_k8\jsons> kubectl get pods -o=jsonpath='{$.items[*].metadata.labels}{"\n"}'



in k8 we don't have to explicitly mention the $


JSONPath is basically a way to query/filter Kubernetes API output and extract specific fields. this is the only point and reason to use it


another thing would be is to use the sort-by 

kubectl get pods --sort-by=.metadata.creationTimestamp

kubectl get pods --sort-by=.metadata.creationTimestamp | Sort-Object -Descending


kubectl get nodes --sort-by=.status.capacity.cpu
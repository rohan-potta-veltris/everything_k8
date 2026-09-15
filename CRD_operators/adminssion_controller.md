An Admission Controller is a component in Kubernetes that checks and/or modifies a request after authentication/authorization done by the apiserver but before the object is actually stored in etcd.


So for example if checks if the modification is valid or not , for example if i change the image to latest , the admission controller will check if thats valid or not and maybe the company doesnt allow this so it should allow such a change 

user -> apiserver -> authentication/authorization -> admission controller -> if yes/pass -> etcd 

Types:
1. mutating admission controller => makes changes before it is stored
2. validating => makes sure the policy is valid yes/no

WEbhook is basically when one is triggered it will call something else 

for example it will make a ns if the ns doestn exist

These are added at the manifest file for the api-server


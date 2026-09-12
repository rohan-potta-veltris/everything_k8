What is RBAC?
RBAC is back is the role based access control
this is for the authorization in the folder of ssl_certificates we made use of the authentication , so this wont mean they can run all commands 

to see if i have access to do something 

(base) PS D:\Devops\everything_k8\authentication_authorization> kubectl auth can-i get pods
yes
(base) PS D:\Devops\everything_k8\authentication_authorization> 

(base) PS D:\Devops\everything_k8\authentication_authorization> kubectl auth whoami
ATTRIBUTE   VALUE
Username    kubernetes-admin
Groups      [kubeadm:cluster-admins system:authenticated]
(base) PS D:\Devops\everything_k8\authentication_authorization> 
To show you the value as to which user am i using.

(base) PS D:\Devops\everything_k8\authentication_authorization> kubectl auth can-i get pods --as adam
no                      
(base) PS D:\Devops\everything_k8\authentication_authorization> 
This can be done by checking if this user has that permission , note if the user doesnt exist it comes ad a no

Groups => v1 means these are core groups 
Named groups => like apps/v1 ,  k8.io/v1 is a named group 
This is found in the apiVersion hence called api groups 

then once we build the role , we need to use the apibinding to make the role binding

(base) PS D:\Devops\everything_k8\authentication_authorization> kubectl config set-credentials user_name --client.key = user_name.key --client-certificate = user_name.crt                   

This is used to add the user into our config and then like this is from the authenticaion details set
and then we need to set the context with 

kubectl config set-context context_name --cluster=cluster-name --user=user_name

kubectl config get-contexts # to get all the contexts

kubectl config use-context context_name #to switch the context

if we get errors for whoami , chances are its because the certificate might have expired.

for kubeadm it would be different 
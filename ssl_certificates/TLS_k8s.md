There is a term called mTLS(m is mutual) where both the client and the server need to validate each other via the certificates
in k8 this is done 

A)
    why do we need to have a secure connection between the client and the master node having the api-server?
    user is the client 
    api-server is the server

B)
    we would also need secure communication between the master nodes and the worker nodes
    api-server is the client
    kubelet is the server (or any component of the worker node) 

So for mTLs we will need a certificate for every component on each individual node

We have components of .crt or .pem will be part of the certificate
If anything in .key or -key is part of the private key 

How this is done in k8

https://kubernetes.io/docs/tasks/tls/certificate-issue-client-csr/

1. user needs to create a key and a certificate

# Create a private key and certificate
openssl genrsa -out myuser.key 3072
openssl req -new -key myuser.key -out myuser.csr -subj "/CN=myuser"


2. now the admin will need to make a CSR.yaml and mention the request and name and stuff 
in the CSR.yaml the .csr file needs to be base64 encoded before adding it to the CSR.yaml file and this needs to be without any line breaks meaning one single line using the command "cat myuser.csr | base64 | tr -d "\n"

this .csr is made by the certificate of the second command

once applied we use kubectl get csr

and this will be in pending till approved, and needs to be authorized by a CA (either internal or universal)
usually since this is internal this CA is hosted on the master plane , and certain role is needed to approve it , and this comes pre-built 

to approve it we use kubectl certificate approve csr_name 
this csr_name will come from kubectl describe csr csr_name


To share this with the user 

kubectl get csr csr_name  -o yaml > user_csr.yaml #this will have the certificates and stuff and all will be encoded and hence needs to be decoded 

certificate_value from the yaml | base64 -d 

then this certificate will be added to kubeconfig and they can add it, and it will be secure




How to verify without the certificate and after certificate?

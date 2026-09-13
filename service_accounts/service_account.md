What is a service account?
This is something an application would use , this is used to run and monitor something 
For example jenkins needs access to the cluster this would be done by the service account 
Same thing for prometheus 

There is a default service account (sa)

(base) PS D:\Devops\everything_k8> kubectl get sa
NAME      SECRETS   AGE
default   0         7d1h
(base) PS D:\Devops\everything_k8> kubectl get sa -A
NAMESPACE            NAME                                     SECRETS   AGE
default              default                                  0         7d1h
kube-node-lease      default                                  0         7d1h
kube-public          default                                  0         7d1h
kube-system          attachdetach-controller                  0         7d1h
kube-system          bootstrap-signer                         0         7d1h
kube-system          certificate-controller                   0         7d1h
kube-system          clusterrole-aggregation-controller       0         7d1h
kube-system          coredns                                  0         7d1h
kube-system          cronjob-controller                       0         7d1h
kube-system          daemon-set-controller                    0         7d1h
kube-system          default                                  0         7d1h
kube-system          deployment-controller                    0         7d1h
kube-system          disruption-controller                    0         7d1h
kube-system          endpoint-controller                      0         7d1h
kube-system          endpointslice-controller                 0         7d1h
kube-system          endpointslicemirroring-controller        0         7d1h
kube-system          ephemeral-volume-controller              0         7d1h
kube-system          expand-controller                        0         7d1h
kube-system          generic-garbage-collector                0         7d1h
kube-system          horizontal-pod-autoscaler                0         7d1h
kube-system          job-controller                           0         7d1h
kube-system          kindnet                                  0         7d1h
kube-system          kube-proxy                               0         7d1h
kube-system          legacy-service-account-token-cleaner     0         7d1h
kube-system          metrics-server                           0         4d
kube-system          namespace-controller                     0         7d1h
kube-system          node-controller                          0         7d1h
kube-system          persistent-volume-binder                 0         7d1h
kube-system          pod-garbage-collector                    0         7d1h
kube-system          pv-protection-controller                 0         7d1h
kube-system          pvc-protection-controller                0         7d1h
kube-system          replicaset-controller                    0         7d1h
kube-system          replication-controller                   0         7d1h
kube-system          resourcequota-controller                 0         7d1h
kube-system          root-ca-cert-publisher                   0         7d1h
kube-system          service-account-controller               0         7d1h
kube-system          service-controller                       0         7d1h
kube-system          statefulset-controller                   0         7d1h
kube-system          token-cleaner                            0         7d1h
kube-system          ttl-after-finished-controller            0         7d1h
kube-system          ttl-controller                           0         7d1h
local-path-storage   default                                  0         7d1h
local-path-storage   local-path-provisioner-service-account   0         7d1h
memory-example       default                                  0         4d
(base) PS D:\Devops\everything_k8> 


To create a service account , for example jenkins

(base) PS D:\Devops\everything_k8> kubectl create sa build-sa
serviceaccount/build-sa created
(base) PS D:\Devops\everything_k8> kubectl describe sa build-sa
Name:                build-sa
Namespace:           default
Labels:              <none>
Annotations:         <none>
Image pull secrets:  <none>
Mountable secrets:   <none>
Tokens:              <none>
Events:              <none>
(base) PS D:\Devops\everything_k8> 


(base) PS D:\Devops\everything_k8\service_accounts> kubectl apply -f .\secret.yaml
secret/build-robot-secret created
(base) PS D:\Devops\everything_k8\service_accounts> kubectl describe secret
Name:         build-robot-secret
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: build-sa
              kubernetes.io/service-account.uid: f4d3b59b-bd95-4559-8f10-bc6dbfec52e2

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1107 bytes
namespace:  7 bytes
token:      <redacted-token>
(base) PS D:\Devops\everything_k8\service_accounts> 


service account is no different than a user where we have to check and use the permission with commands like 
kubectl auth can-i get pods service_name, and so on


Discuss: Can you expose the Pods as a service without a deployment?

Discuss: Under what condition would you use the service types LoadBalancer, node port, clusterIP, and external?


Services don't use match labels because the selector is used by default for that

(base) PS D:\Devops\everything_k8\tasks\day-09> wget 10.96.183.150
wget : Unable to connect to the remote server
At line:1 char:1
+ wget 10.96.183.150
+ ~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand
 
(base) PS D:\Devops\everything_k8\tasks\day-09> 
This doesnt work from outside the cluster because it is a clusterip and only works within it.


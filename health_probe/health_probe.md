What are health Probes?
Probe is basically something that monitors the system and necessary actions to ensure that everything is working fine.

Health Probes are so that the stuff are healthy 

1. Startup => This will make it active first and then makes sure the readiness and liveness comes up and then the traffic is served.
2. Readiness => This makes sure the application is ready before we can serve the traffic to it.
3. Liveness => if the application fails or crashes due to network or application , then it would restart the application, until it brings the application back up , and after a few tries if it still fails it will thorw and error and tells us with the error.

They can all check 3 types
1. HTTPS => sends a request to the endpoint 
2. TCP => Open a port at the particular endpoint
3. command => runs a particular command and if it works the probe is success

These probes are for the containers


So for the liveness pod 
(base) PS D:\Devops\everything_k8\health_probe> kubectl apply -f .\command-liveness.yaml                                                   
pod/liveness-exec created                                        
(base) PS D:\Devops\everything_k8\health_probe> kubectl get pods -w                                                                        
NAME            READY   STATUS    RESTARTS   AGE                                                                                           
liveness-exec   1/1     Running   0          12s                         
liveness-exec   1/1     Running   1 (1s ago)   79s                     
liveness-exec   1/1     Running   2 (3s ago)   2m38s
liveness-exec   1/1     Running   3 (1s ago)   3m57s


It comes up as restart every 30 seconds and then restarts the file which makes it back to healthy

CrashLoopBackOff can be due to the liveness or the probes are not healthy

kubectl apply --force -f manifest.yaml # this will like forcefully apply my chnages 
this is done by delete and then apply the file

failure threshold => how many times to check for concecutive fail to mark as failure
successthreshold => how many time to confirm as success

And due to any failure it will add a toleration which gets the unhealthy pods evicted from the healthy nodes 

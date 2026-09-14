There is no docker , so we use crictl in place of docker to help troubleshoot the contianers 
docker ps => crictl ps
This comes installed with the kubeadm ,

Now we would use this when the apiserver is down and this wont allow us to run the kubectl commands , so we will use the crictl and trouble shoot the api server or any such situation 

To run a pod the steps are different cause we need to pull image and make a json and then run using crictl runp config.json


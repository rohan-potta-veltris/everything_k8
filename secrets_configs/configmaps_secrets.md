How to re-use the env variables , naturally we cant keep adding it at the pod level , so we dont include it in the pod , 
we use a config map and we injesct the value to the pod 



imperative way of cm => kubectl create cm app-cm --from-literal=name=value \ --fronm-literal=name1=value1

kubectl get cm 
kubectl describe cm cm-name , this will show the data of the config map

The best way is to mount it as a volume and then read it from there

to pass the entire file as a cm

kubectl create cm cm-app --from-file=app.config

to updat the cm for a pod is to force it meaning delete and then re-create



Now how do i use secrets ? cause cm can still be read.

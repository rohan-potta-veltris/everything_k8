How to re-use the env variables; naturally, we can't keep adding them at the pod level, so we don't include them in the pod.
we use a config map and inject the value into the pod



imperative way of cm => kubectl create cm app-cm --from-literal=name=value \ --from-literal=name1=value1

kubectl get cm 
kubectl describe cm cm-name , this will show the data of the config map

The best way is to mount it as a volume and then read it from there

to pass the entire file as a cm

kubectl create cm cm-app --from-file=app.config

to update the cm for a pod, force it by deleting and then re-creating it



Now how do i use secrets ? cause cm can still be read.

So deployment and replica-sets are for stateless application , meaning the data is not that important 
but stateful sets is for application where the state and data is important 

in deployment we have the pods as:
name-random_id
name-random_id 
as so on , so we dont have any control over this 

But in stateful set the pod will be created as follows;
name-0
name-1
name-2 
and they have ordinal ordering

the most recently created pod is delete first , follows stack method

What is a headless service?

In Kubernetes, a headless Service is a Service that does not get a single ClusterIP. It is commonly used with StatefulSets because you want to reach each individual Pod, rather than having traffic load-balanced across all Pods.

Point is via service i could potentially connect to any one of these but with headless , i can be specific about which one to connect to 

This kinda depends if we want the load balancing or not hence why we use the stateful

So now we need to make a pv , and not directly use the sc and this is because we want the static control so we make the sc as non dynamic and then make our pv to that sc and then have an individual pvc to each of the stateful sets we make 

                 StorageClass
                  mongo-sc
                     │
                     │
              manually created
                     ↓
                    PV
               mongo-pv-01
                     ↑
                     │ binds to
                     │
                    PVC
            mongo-data-mongodb-0
                     ↑
                     │ created by
                     │
               StatefulSet
                  mongodb
                     │
                     ↓
                 mongodb-0


you manually create:

StorageClass
PV
DONT make PVC manually — StatefulSet creates it through volumeClaimTemplates
DONT make Pod manually — StatefulSet creates it


now the pods will be created one by one , and 


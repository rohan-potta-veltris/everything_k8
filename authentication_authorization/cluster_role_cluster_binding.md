namespace scoped resources
pods , deployments, replicaset , services

These are created via the namespace scoped resources.

Cluster-scoped resources:
namespaces and nodes


So for cluster-scoped resources are called cluster roles to get like list nodes , watch nodes get nodes and they are bound by cluster role binding


What happens if i forget to add the namespace in the Role ?

kubectl api-resources --namespaced=true
shows the roles depending on namespace

kubectl api-resources --namespaced=false
will show the cluster roles at cluster level, that dont depend on the namespace




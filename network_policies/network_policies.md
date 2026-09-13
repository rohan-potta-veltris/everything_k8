What is the network flow?
Flow coming to the application is Incoming called Ingress
User -> FE -> BE-> DB

Flow coming out of the application is Outcoming called Egress
DB->BE->FE->User

All the FE BE DB are on separate nodes 

All the intercommunication in the cluster is handled by CNI = Container Network Interface.
Example Weave-net , flannel , calico , this needs to be installed and this will make a pod on every node via the daemon set and makes the networking.

Now for example , the FE should not connect to database and vice versa , this is all controlled / restricted with the help of network policies.

Flannel and kindnet dont support network policies

(base) PS D:\Devops\everything_k8> kubectl get pods -n kube-system      
NAME                                            READY   STATUS    RESTARTS      AGE
coredns-76f75df574-wh7ct                        1/1     Running   3 (24h ago)   7d13h
coredns-76f75df574-z5r5s                        1/1     Running   3 (24h ago)   7d13h
etcd-cluster-control-plane                      1/1     Running   1 (24h ago)   5d
kindnet-ct6lg                                   1/1     Running   5 (24h ago)   7d13h
kindnet-x5w2f                                   1/1     Running   5 (24h ago)   7d13h
kindnet-zvq9m 
This kindnet is how the kind cluster makes it

Now when i create the cluster , it will be in a not ready state and this is because the CNI is not installed and was disabled.


to install https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/ here is the list with network policies 

How is this network policy different from ingress and egress?

Why the nodes were NotReady to begin with

Your kind config almost certainly had disableDefaultCNI: true — that's the standard setup when you want to install Calico yourself. Without a CNI plugin, kubelet reports the NetworkReady=false condition, which surfaces as NotReady on the node. The scheduler refuses to place ordinary pods on a NotReady node, which is why coredns and local-path-provisioner sat Pending for the entire session.

That's not a failure state. It's the expected condition of a cluster waiting for its network. Everything that was running — etcd, apiserver, kube-proxy — runs with hostNetwork: true and so doesn't need pod networking at all.

The bootstrap paradox, and how calico-node gets around it

There's an obvious circularity: pods can't schedule until CNI is installed, and CNI is installed by a pod. Calico escapes it three ways. calico-node is a DaemonSet, so the DaemonSet controller assigns it directly to nodes rather than going through normal scheduling. It carries tolerations for the NotReady taints. And it runs host-networked, so it needs no pod IP.

Its init containers are what you watched progress through Init:1/3 → 2/3 → 3/3: they write the CNI binary and config into /opt/cni/bin and /etc/cni/net.d on the host. The moment that lands, kubelet detects a valid CNI config, flips NetworkReady to true, and the node goes Ready. That's why your Pending pods all moved to ContainerCreating within seconds of the first calico-node reaching Running.

The actual error: version mismatch

The one genuine failure was isCIDR failing to compile. That's a CEL function from Kubernetes' IP/CIDR extension library, added in 1.31. Calico v3.32.2 ships a CRD whose validation schema uses it. On your 1.29 cluster the API server's CEL compiler didn't recognize the identifier and rejected the schema outright.

So clusternetworkpolicies.policy.networking.k8s.io never got created — and then the tigera-operator crashlooped. I told you those were probably connected, but we never actually read the logs before you rebuilt the cluster, so that link is an educated guess rather than something either of us verified. What's certain is that moving to 1.31 fixed both at once.

The two non-errors

Installing tigera-operator.yaml doesn't install Calico. The operator is a controller that watches for an Installation resource; with none present it runs happily and does nothing. custom_resources.yaml is what supplies that resource and triggers the actual deployment. Easy step to miss because the operator looks healthy either way.

And the resource mapping not found errors were kubectl's local discovery cache, not the cluster. kubectl caches the API resource list per-server for 10 minutes; you'd run commands against the fresh cluster before the Calico CRDs existed, so the cached list had no operator.tigera.io group. The CRD was genuinely there — kubectl get crd proved it — kubectl just didn't know to look. Deleting ~/.kube/cache fixed it instantly. You'll hit this again with any operator install, so it's worth filing away.


ingress and egress are defined from the perspective of a Pod when we're talking about Kubernetes NetworkPolicy.

Think of the Pod as the center:
             INGRESS
                ↓
        ┌──────────────┐
        │     Pod      │
        └──────────────┘
                ↓
             EGRESS
             

Initially there will be a default deny and then we will only have the rules allow as what is needed 

Q) how exactly does this policy implemented undergroud? like what prevents this?


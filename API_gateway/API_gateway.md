This is the newer form of the ingress

Ingress only allowed the HTTPS routing 

but now with api-gateway we can use the rules for the HTTP, TCP, UDP and so on 
allows header and query based routing as well 
allows canary splitting (70-30 splits and so on)

This allows the CRD (Custom Resource Definition) to also be distributed

In ingress the routing worked on the annotation , and they would depend on the ingress controller we would use , like nginx and so on.
To generalize this we use the api-gateway 


Components:
1. gateway class -> the one incharge of the load balancing implementation 
2. gateway -> Actual Load balance instance
3. http route -> has the routing rules to map the traffic 



To install the gateway controller with the CRD:
# Install Gateway API resources
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v1.5.1" | kubectl apply -f -

# Verify installation
kubectl get crd | grep gateway

To get the nginx controller:
# Deploy NGINX Gateway Fabric CRDs
kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/crds.yaml

# Deploy NGINX Gateway Fabric
kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/nodeport/deploy.yaml

# Verify the deployment
kubectl get pods -n nginx-gateway


Note api-gateway is a custom-resource , hence why the crd is needed so that it understand that kubectl get gateway it understands what the gateway is itself 

The resource is called "gateway"



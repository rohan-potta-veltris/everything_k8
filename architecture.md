Q) Can you explain the architecture of Kubernetes?
A) Kubernetes mainly has two major components:
1. The control plane, which acts as the brain of Kubernetes.
2. The worker node, where the main application runs.

Components of the Control Plane:
1. API server => This is the tool used to communicate with the cluster. It ensures that the user is authenticated and authorized to execute the requested command. Users communicate with the API server through kubectl commands.
2. etcd => This is essentially the database of the cluster. It stores information such as the number of nodes and deployments as key-value pairs.
3. Controller manager => This manages the individual controllers for components such as services, deployments, and namespaces, and ensures the desired state is equal to the actual state.
4. Scheduler => This finds a suitable node for a pod based on criteria such as availability and resource utilization. It receives information from the API server.

Components of the Worker Node:
1. Kubelet => This is the communicator between the control plane and the worker node. It ensures that the pods on the node are running.
2. Kube-proxy => This is the component that handles networking within the node.
3. Pod => This is the smallest deployable unit in Kubernetes and can contain one or more containers. Containers run in pods through the container runtime.

Q) What is a pod?
A pod is the smallest deployable unit in Kubernetes. A pod can have multiple containers within it, although it usually contains one container.

Q) Why does k8s use a pod?
A) This is because a pod is the smallest deployable unit in k8s and it needs a wat ti group container to be treated as a single application unit 

Q) What is the flow for updating a deployment from one pod to three pods?
1. Through kubectl, we make the request and send it to the API server. The API server then authenticates, authorizes, and validates the request.
2. The API server updates the key-value pair in etcd to indicate that we want three pods instead of one.
3. etcd sends a response back to the API server confirming that the update is complete.
4. The controller manager detects the change through its controllers.
5. The ReplicaSet controller creates two new pod objects through the API server to bring the total from one to three.
6. The scheduler detects the new pods that do not have a node assigned and decides which worker node each pod should run on.
7. The API server then communicates with the kubelet on the selected node, and the kubelet creates the pods.
8. The API server updates etcd to indicate that the pods have been created.
9. The API server sends a response to the client confirming that the pods have been created.


Q) etcd stores the key value pair as how and why?
A) it essentially stores it like a json and this is mainly done as a key pair as k8 has different resource types and their schemas are evolve and allows flexibility rather than a fixed schema
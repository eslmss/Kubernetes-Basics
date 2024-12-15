<u>Pod Networking</u>> in Kubernetes using Calico as the CNI (Container Network Interface). Each pod is assigned a unique IP address (e.g., 10.0.10.34 and 10.0.10.37), and Calico manages the routing of traffic between pods across nodes. The Kubelet runs on each worker node to manage pods, while Calico configures the network routes (ip route) that allow pods to communicate seamlessly, even if they are on different nodes. The communication is routed at Layer 3 (L3) without the need for NAT, and etcd stores the cluster's network configuration and state. This ensures transparent pod-to-pod communication across the cluster.

Primary types of Kubernetes services managed by kube-proxy:

<u>ClusterIP</u>: This is the default service type, which exposes the service on a virtual IP address accessible only within the Kubernetes cluster. It allows internal communication between pods.
<u>NodePort</u>: This type exposes the service on a specific port of each node in the cluster, enabling external access to the service via the node’s IP and port.
<u>LoadBalancer</u>: This service type works with cloud providers to provision an external load balancer, distributing traffic across the pods and providing a single external IP for accessing the service.

1. **Start a local cluster with Minikube with 3 nodes and check status**
   -`minikube delete` (to delete the last cluster created with 3 nodes)
   -`kubectl config current-context`
   -`minikube start --nodes <nodes_quant>`   (with 1 node if not specified)
   -`minikube status`
   -`kubectl config current-context`

2. **Lists all cluster nodes (by default there will only be one in Minikube)**
   -`kubectl get nodes`

3. **Lists logical spaces to manage resources**
   -`kubectl get namespaces`

4. **Lists pods executing in all namespaces (we can also get the pods from a specific namespace)**
   -`kubectl get pods -A`
   -`kubectl get pods --namespace <ns-name>`

5. **Apply <u>ClusterIP</u> manifest (and check if it's running)**
   -`kubectl apply -f 06-randompod.yaml` (creates a pod that launches a container using the Ubuntu image and keeps it alive)
   -`kubectl apply -f 07-hello-deployment-svc-clusterIP.yaml` (creates a Deployment to run 3 pods of a sample application and a Service to expose these pods on port 8080. This service expose the same IP for all pods and works like a simple load balancer for the app)
   -`kubectl get pods`
   -`kubectl get all`  (to see all the key resources in the cluster)
   -`kubectl get service`  (to see only the svcs in the cluster)
   -`kubectl describe svc hello`  (it shows the 3 endpoints for our 3 nodes, Kubernetes works as a simple load balancer for them)
   -`kubectl get pods -o wide` (to check that each of the pods runs on a different node)

6. **Deleting a pod**
   -`kubectl delete pod <pod-name>` (another pod will automatically be created to replace the deleted one)
   -`kubectl get pods -o wide` (to check that each of the pods runs on a different node)
   -`kubectl describe svc hello`  (now it shows the new endpoint ip)

6. **How to run a shell command within the pod "ubuntu"**
   -`kubectl get pods -o wide` (to check that each of the pods runs on a different node)
   -`kubectl exec -it ubuntu -- bash`
   -`apt update` (to install ping command)
   -`apt install -y iputils-ping` (to install ping command)
   -`ping hello` (this will resolve: <svc-name>.<ns-name>.svc.cluster.local with allways the next ip)
   -`kubectl get svc hello` (in cmd)
   -`apt update` (to install curl command)
   -`apt install -y curl` (to install curl command)
   -`curl http://hello:8080` (we can see that we get a different hostname -pod- every time we run it)

   
7. **Return the detailed info about the pod "ubuntu" in YAML format**
   -`kubectl get pod <pod-name> -o yaml`

8. **Deleting resources from the ClusterIP to go on with the <u>NodePort</u>**
   -`kubectl get pods -o wide` (to check that each of the pods runs on a different node)
   -`kubectl delete -f 07-hello-deployment-svc-clusterIP.yaml`
   -`kubectl get pods -o wide`
   -`kubectl apply -f 08-hello-deployment-svc-nodePort.yaml`
   -`kubectl get pods -o wide`
   -`kubectl get all`  (to see all the key resources in the cluster)
   -`kubectl get service`  (to see only the svcs in the cluster: its now using the port 30000 as specified in the manifest)
   -`kubectl describe svc hello`  (it shows the 3 endpoints for our 3 nodes, Kubernetes works as a simple load balancer for them)
   -`kubectl get pods -o wide`

9. **To Access "hello" service in Minikube, we can do it by redirecting the traffic with minikube service {svc-name}:**
   - Doc: https://minikube.sigs.k8s.io/docs/handbook/accessing/#nodeport-access
   - `minikube service hello` (from another cmd: it will be opened on our navigator and this will expose which hostname we are accessing)
   - `curl http://127.0.0.1:<tunnel-port>` (run it multiple times to see how the balancing works)
   
10. **Deleting resources from the NodePort to go on with the <u>LoadBalancer</u>**
   - Doc: https://minikube.sigs.k8s.io/docs/handbook/accessing/#loadbalancer-access
   -`kubectl get pods -o wide` (to check that each of the pods runs on a different node)
   -`kubectl delete -f 08-hello-deployment-svc-nodePort.yaml`
   -`kubectl get pods -o wide`
   -`kubectl apply -f 09-hello-deployment-svc-loadBalancer.yaml`
   -`kubectl get pods -o wide`
   -`kubectl get service`  (to see only the svcs in the cluster: now it shows EXTERNAL-IP as 'pending')
   -`kubectl describe svc hello`  (it shows the 3 endpoints for our 3 nodes, Kubernetes works as a simple load balancer for them)

11. **To Access "hello" service in Minikube, we can do it via tunnel (a persistent channel)**
   -`minikube tunnel` (from another cmd to create a tunnel between our local machine and minikube and keep it running)
   -`kubectl get service`  (to see only the svcs in the cluster: now it shows EXTERNAL-IP as 127.0.0.1)
   -`curl http://127.0.0.1:8080/`  (run it multiple times to see how the load balancer works)

12. **Delete the Minikube's cluster**
   -`minikube delete`
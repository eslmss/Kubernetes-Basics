This manifest creates a <u>DaemonSet</u> (resource that ensures one pod runs on each node) named "nginx-deployment." It deploys a pod running an nginx:alpine container. The pod includes environment variables (e.g., MI_VARIABLE, DD_AGENT_HOST), resource requests and limits, readiness and liveness probes, and exposes port 80. The DaemonSet ensures one pod is created per node, which is suitable for node-level tasks.

Use Cases: Monitoring, Networking, Logging

1. **Start a local cluster with Minikube with 3 nodes and check status**
   - `minikube delete` (to delete the existent cluster created by default with 1 node)
   - `kubectl config current-context`
   - `minikube start --nodes 3`
   - `minikube status`
   - `kubectl config current-context`

2. **Lists all cluster nodes (by default there will only be one in Minikube)**
   - `kubectl get nodes`

3. **Lists logical spaces to manage resources**
   - `kubectl get namespaces`

4. **Lists pods executing in all namespaces (we can also get the pods from a specific namespace)**
   - `kubectl get pods -A`
   - `kubectl get pods --namespace <ns-name>`

5. **Apply manifest (and check if it's running)**
   - `kubectl apply -f 04-daemonset.yaml`
   - `kubectl get pods`
   - `kubectl get pods -o wide` (to check that each of the pods runs on a different node)

6. **How to run a shell command within the pod "nginx"**
   - `kubectl exec -it nginx -- sh`
   - `ls`
   - `ps fax` (this will list all the processes in execution in the pod's container)

7. **Return the detailed info about the pod "nginx" in YAML format**
   - `kubectl get pod nginx -o yaml `

8. **Check how a pod responds to a deletion (in this case, for DaemonSet, after a pod deletion, another one will be automatically created on the same node)**
   - `kubectl get pods -o wide`
   - `kubectl delete pod <pod-name>`
   - `kubectl get pods -o wide`

9. **Delete the Minikube's cluster**
   - `minikube delete`
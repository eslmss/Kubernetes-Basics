This manifest deploys a <u>pod</u> named "nginx" which contains one container. This container uses the nginx:alpine image.
It also sets environment variables, assigns resource requests and limits, and configures readiness and liveness probes to ensure the container is healthy and ready to serve traffic.

1. **Start a local cluster with Minikube and check status**
   - `kubectl config current-context`
   - `minikube start`
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
   - `kubectl apply -f 02-pod.yaml`
   - `kubectl get pods`

6. **How to run a shell command within the pod "nginx"**
   - `kubectl exec -it nginx -- sh`
   - `ls`
   - `ps fax` (this will list all the processes in execution in the pod's container)

7. **Return the detailed info about the pod "nginx" in YAML format**
   - `kubectl get pod nginx -o yaml `

8. **Check how a pod responds to a deletion (this will simply kill the pod, because there's no order to keep always active an "nginx" pod)**
   - `kubectl get pods`
   - `kubectl delete pod nginx`
   - `kubectl get pods`

9. **Delete the Minikube's cluster**
   - `minikube delete`
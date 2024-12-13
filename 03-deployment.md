This manifest creates a <u>Deployment</u> (template for creating pods) named "nginx-deployment" that manages 2 replicas (for HA) of a Pod running an nginx:alpine container. It includes environment variables, resource requests and limits, readiness and liveness probes, and exposes port 80.

Use Cases: Web Apps, Stateless APIs

1. **Start a local cluster with Minikube and check status**
   - `kubectl config current-context`
   - `minikube start`   (with 1 node by default)
   - `minikube status`
   - `kubectl config current-context`

2. **Lists all cluster nodes (there will only be one in Minikube)**
   - `kubectl get nodes`

3. **Lists logical spaces to manage resources**
   - `kubectl get namespaces`

4. **Lists pods executing in all namespaces (we can also get the pods from a specific namespace)**
   - `kubectl get pods -A`
   - `kubectl get pods --namespace <ns-name>`

5. **Apply manifest (and check if it's running)**
   - `kubectl apply -f 03-deployment.yaml`
   - `kubectl get pods`

6. **How to run a shell command within the pod "nginx"**
   - `kubectl exec -it nginx -- sh`
   - `ls`
   - `ps fax` (this will list all the processes in execution in the pod's container)

7. **Return the detailed info about the pod "nginx" in YAML format**
   - `kubectl get pod nginx -o yaml `

8. **Check how a pod responds to a deletion (now, after killing one pod, another one will be automatically created in order to keep always active an "nginx" pod)**
   - `kubectl get pods`
   - `kubectl delete pod <pod-name>`
   - `kubectl get pods`

   9. **To delete the Deployment**
   - `kubectl delete -f 03-deployment.yaml` or `kubectl delete -f <deployment-name>`
   - `kubectl get pods` 

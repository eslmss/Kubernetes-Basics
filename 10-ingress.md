1) 10 manifest deploys two versions (v1 and v2) of a "hello-app" application, each managed by its own deployment and exposed via separate services.
2) 11 manifest defines an Ingress resource in Kubernetes that routes HTTP traffic to different backend services based on the URL path (v1 is routed to the hello-v1 service on port 8080, and v2 is routed to the hello-v2 service on port 8080).

1. **Start a local cluster with Minikube with 3 nodes and check status**
   -`minikube delete` (to delete the last cluster created with 3 nodes)
   -`kubectl config current-context`
   -`minikube start --nodes 4`   (with 1 node if not specified)
   -`minikube status`
   -`kubectl config current-context`

2. **Lists all cluster nodes (by default there will only be one in Minikube)**
   -`kubectl get nodes`

3. **Lists logical spaces to manage resources**
   -`kubectl get namespaces`

4. **Lists pods executing in all namespaces (we can also get the pods from a specific namespace)**
   -`kubectl get pods -A`
   -`kubectl get pods --namespace <ns-name>`

5. **Running the manifest 10**
   -`kubectl apply -f 10-hello-v1-v2-deployment-svc.yaml` (deploys v1 and v2 for "hello-app", each exposed via separate services)
   -`kubectl get all`  (we can see that we have 3 containers/pods and 2 services for each version)

6. **Running the manifest 11 for <u>Ingress</u>'s resource on Minikube**
   -`kubectl get ns`
   - Doc: https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/#enable-the-ingress-controller
   -`kubectl get ns`  (now we can see namespace "ingress-nginx")
   -`kubectl get pods -n ingress-nginx`  (we can see that Nginx controller is running)
   -`kubectl apply -f 11-hello-ingress.yaml` (deploys v1 and v2 for "hello-app", each exposed via separate services)
   -`kubectl get ingress` (we must wait some minutes to see our public IP ADDRESS) NOTE: this is exposing our "hello-app" in the port 80
   -`kubectl -n ingress-nginx get services`
   
9. **To Access "hello-app" service in Minikube, we can do it via tunnel (a persistent channel)**
   -`minikube tunnel` (from another cmd to create a tunnel between our local machine and minikube and keep it running)
   -`kubectl get service`  (to see only the svcs in the cluster: now it shows EXTERNAL-IP as 127.0.0.1)
   -`curl http://127.0.0.1:80/`  (404: nothing defined for /) NOTE: we can ignore the port number
   -`curl http://127.0.0.1/v1`   (it will balance between pods in /v1)
   -`curl http://127.0.0.1/v2`   (it will balance between pods in /v2)

10. **Delete the Minikube's cluster**
   -`minikube delete`
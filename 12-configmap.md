12 and 13 manifests: 
<u>ConfigMap</u> game-demo stores configuration data, including simple key-value pairs and file-like data. A Pod with an nginx:alpine container uses this ConfigMap in two ways: environment variables (PLAYER_INITIAL_LIVES and UI_PROPERTIES_FILE_NAME) are set from specific keys, while file-like data (game.properties and user-interface.properties) is mounted as read-only files in the /config directory. This setup decouples application configuration from the container image, enabling easy updates to settings without modifying the Pod or rebuilding images.

<u>ConfigMaps</u> are a Kubernetes-native way to handle application configuration, promoting separation of concerns and enabling more flexible, dynamic configurations for containerized applications.

Benefits: Modularity, Reuse, Scalability, Reduce Hard-Coding

1. **Start a local cluster with Minikube with 3 nodes and check status**
   -`minikube delete` (to delete the last cluster created with 3 nodes)
   -`kubectl config current-context`
   -`minikube start --nodes 3`   (with 1 node if not specified)
   -`minikube status`
   -`kubectl config current-context`

2. **Lists all cluster nodes (by default there will only be one in Minikube)**
   -`kubectl get nodes`

3. **Lists logical spaces to manage resources**
   -`kubectl get namespaces`

4. **Lists pods executing in all namespaces (we can also get the pods from a specific namespace)**
   -`kubectl get pods -A`
   -`kubectl get pods --namespace <ns-name>`

5. **Running the manifest 12 and 13**
   -`kubectl apply -f 12-configmap.yaml` (first we apply the config map)
   -`kubectl get configmaps`  
   -`kubectl apply -f 13-pod-configmap.yaml` (first we apply the config map)
   -`kubectl get pods`

6. **We go into the nginx pod**
   -`kubectl exec -it nginx -- sh`
   -`env` (this will show us the env variables that we created on our 13-manifest: PLAYER_INITIAL_LIVES and UI_PROPERTIES_FILE_NAME)
   -`ls /config` (here we can see that this path now stores the files that we also created in the 13-manifest: game.properties and user-interface.properties)
   -`exit`

5. **Running the manifest 14 and 15**
   -`kubectl apply -f 12-configmap.yaml` (first we apply the config map)
   -`kubectl get configmaps`  
   -`kubectl apply -f 13-pod-configmap.yaml` (first we apply the config map)
   -`kubectl get pods`











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
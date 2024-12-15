14 and 15 manifests: 
<u>Secrets</u> in Kubernetes is an object used to store sensitive information, such as passwords, API keys, or TLS certificates. Unlike ConfigMaps, Secrets are designed specifically for confidential data and are stored in base64-encoded format. They allow applications to securely access sensitive data without embedding it directly into container images or code. Secrets can be used as environment variables, mounted as files, or referenced in other resources, providing a secure and flexible way to manage sensitive information in a Kubernetes cluster.

NOTE: base64-encoded format it's not 100% secure, it can easily be decoded


1. **Start a local cluster with Minikube with 3 nodes and check status:**
   - `minikube delete` (to delete the last cluster created with 3 nodes)
   - `kubectl config current-context`
   - `minikube start --nodes 3`   (with 1 node if not specified)
   - `minikube status`
   - `kubectl config current-context`

2. **Lists all cluster nodes (by default there will only be one in Minikube):**
   - `kubectl get nodes`

3. **Lists logical spaces to manage resources**
   - `kubectl get namespaces`

4. **Lists pods executing in all namespaces (we can also get the pods from a specific namespace):**
   - `kubectl get pods -A`
   - `kubectl get pods --namespace <ns-name>`

5. **Running the manifest 14 and 15:**
   - `kubectl apply -f 14-secret.yaml ` (first we apply the config map)
   - `kubectl get configmaps`  
   - `kubectl apply -f 15-pod-secret.yaml` (first we apply the config map)
   - `kubectl get pods`

6. **We go into the nginx pod:**
   - `kubectl exec -it nginx -- sh`
   - `env` (this will show us the decoded env variables that we created and referenced on our 14 and 15 manifests: MYSQL_USER and MYSQL_PASSWORD)
   - `exit`

6. **Extra: Kustomization:**:
Doc: https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/ 
   kustomization.yaml manifest defines a custom setup using Kustomize, a tool for managing Kubernetes resources. It applies a common label app: ejemplo to all resources, allowing easy identification. The resources section specifies that the 15-pod-secret.yaml resource should be included in the configuration. The secretGenerator creates a Secret named db-credentials with two key-value pairs, username=admin and password=secr3tpassw0rd!. Lastly, it modifies the nginx image to use the latest tag. This setup automates the generation of a Secret and the modification of container images in a Kubernetes environment.
   - `kubectl kustomize` (this search for a "kustomization.yaml" file in our path and run it to apply those changes)

7. **Delete the Minikube's cluster:**
   - `minikube delete`
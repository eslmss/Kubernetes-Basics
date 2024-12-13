This manifest creates a <u>StatefulSet</u> (to manage stateful apps, with data persistance and with pods created in a specific order) named "my-csi-app-set" that ensures a single pod (replicas: 1) with persistent storage is deployed. The pod runs a busybox container executing sleep infinity (to run indefinitely). 
It includes a volume mounted at /data using a dynamically provisioned PersistentVolumeClaim (5Gi of storage) with access mode ReadWriteOnce and storage class do-block-storage.

Use Cases: Databases, Stateful Systems

1. **Start a local cluster with Minikube with 3 nodes and check status**
   - `minikube delete` (to delete the last cluster created with 3 nodes)
   - `kubectl config current-context`
   - `minikube start --nodes 3`   (with 1 node if not specified)
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
   - `kubectl apply -f 05-statefulset.yaml`
   - `kubectl get statefulsets`
   - `kubectl get pods`
   - `kubectl get pods -o wide` (to check that each of the pods runs on a different node)

6. **How to run a shell command within the pod "nginx"**
   - `kubectl exec -it nginx -- sh`
   - `ls`
   - `ps fax` (this will list all the processes in execution in the pod's container)

7. **Return the detailed info about the pod "nginx" in YAML format**
   - `kubectl get pod <pod-name> -o yaml `

8. **Check pod and pvc**
   - `kubectl describe pod my-csi-app-set-0`
   - `kubectl get pvc`
   - `kubectl describe pvc <optional-pvc-name>`
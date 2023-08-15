# Cluster Architecture, Installation & Configuration (25%)

kubernetes.io > Documentation > Reference > kubectl CLI > [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

kubernetes.io > Documentation > Tasks > Monitoring, Logging, and Debugging > [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Accessing Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) using API

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Use Port Forwarding to Access Applications in a Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)

### Setup autocomplete for k8s commands

<details><summary>show</summary>
<p>

```bash
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

</p>
</details>

### Show kubeconfig settings

<details><summary>show</summary>
<p>

```bash
kubectl config view
```

</p>
</details>

### Use multiple kubeconfig files at the same time

<details><summary>show</summary>
<p>

```bash
KUBECONFIG=~/.kube/config:~/.kube/kubconfig2
```

</p>
</details>

### Create a role the will allow users to get, watch, and list pods and container logs

<details><summary>show</summary>
<p>

```bash
# create a file named role.yml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "watch", "list"]

# create the role
kubectl apply -f role.yml
```

</p>
</details>

### Create a role binding that binds to a role named pod-reader, applies to a user named `dev`

<details><summary>show</summary>
<p>

```bash
# create a file named role-binding.yml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader
  namespace: default
subjects:
- kind: User
  name: dev
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

</p>
</details>

### 

### Permanently save the namespace for all subsequent kubectl commands in that context.

<details><summary>show</summary>
<p>

```bash
kubectl config set-context --current --namespace=ggckad-s2
```

</p>
</details>

### Set a context utilizing a specific username and namespace

<details><summary>show</summary>
<p>

```bash
kubectl config set-context gce --user=cluster-admin --namespace=foo \
  && kubectl config use-context gce
```

</p>
</details>

### List all services in the kube-system namespace

<details><summary>show</summary>
<p>

```bash
kubectl get svc -n kube-system
```

</p>
</details>

### Get pods on all namespaces

<details><summary>show</summary>
<p>

```bash
kubectl get po --all-namespaces
```

</p>
</details>

### List all pods in the namespace, with more details

<details><summary>show</summary>
<p>

```bash
kubectl get pods -o wide
```

</p>
</details>

### List a particular deployment

<details><summary>show</summary>
<p>

```bash
kubectl get deployment my-deployment
```

</p>
</details>

### List all pods in the default namespace

<details><summary>show</summary>
<p>

```bash
kubectl get pods
```

</p>
</details>

### Get pod's YAML

<details><summary>show</summary>
<p>

```bash
kubectl get po nginx -o yaml
```

</p>
</details>

### Get information about the pod, including details about potential issues (e.g. pod hasn't started)

<details><summary>show</summary>
<p>

```bash
kubectl describe po nginx
```

</p>
</details>

### Get pod logs

<details><summary>show</summary>
<p>

```bash
kubectl logs nginx
```

</p>
</details>

### Output a pod's YAML without cluster specific information

<details><summary>show</summary>
<p>

```bash
kubectl get pod my-pod -o yaml
```

</p>
</details>

### List all nodes in the cluster

<details><summary>show</summary>
<p>

```bash
kubectl get nodes
# or, get more information about the nodes
kubectl get nodes -o wide
```

</p>
</details>

### Describe nodes with verbose output

<details><summary>show</summary>
<p>

```bash
kubectl describe nodes
```

</p>
</details>

### List services sorted by name

<details><summary>show</summary>
<p>

```bash
kubectl get services --sort.by=.metadata.name
```

</p>
</details>

### Get the external IP of all nodes

<details><summary>show</summary>
<p>

```bash
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'
```

</p>
</details>

### Create a new namespace

<details><summary>show</summary>
<p>

```bash
kubectl create namespace web
```

</p>
</details>

### List all the namespaces that exist in the cluster

<details><summary>show</summary>
<p>

```bash
kubectl get namespaces
```

</p>
</details>

### Create a pod which runs an nginx container

<details><summary>show</summary>
<p>

```bash
kubectl run nginx --image=nginx
# or
kubectl run nginx2 --image=nginx --restart=Never --dry-run -o yaml | kubectl create -f -
```

</p>
</details>

### Delete a pod

<details><summary>show</summary>
<p>

```bash
kubectl delete po nginx
```

</p>
</details>

### Get the status of the control plane components (cluster health)

<details><summary>show</summary>
<p>

```bash
# check the livez endpoint 
curl -k https://localhost:6443/livez?verbose

# or

kubectl get --raw='/livez?verbose'

# check the readyz endpoint
curl -k https://localhost:6443/readyz?verbose

# or

kubectl get --raw='/readyz?verbose'

# check the healthz endpoint
curl -k https://localhost:6443/healthz?verbose

# or

kubectl get --raw='/healthz?verbose'
```
[Kubernetes API Health Endpoints](https://kubernetes.io/docs/reference/using-api/health-checks/)

</p>
</details>

### Create a deployment with two replica pods from YAML

<details><summary>show</summary>
<p>

```YAML
# create a deployment object using this YAML template with the following command
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      run: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
EOF
```

```bash
# create the file deploy.yaml with the content above
vim deploy.yaml
# create the deployment
kubectl apply -f deploy.yaml
# get verbose output of deployment YAML
kubectl get deploy nginx-deployment -o yaml
# add an annotation to the deployment
kubectl annotate deploy nginx mycompany.com/someannotation="chad"
# delete the deployment
kubectl delete deploy nginx
```

</p>
</details>

### Add an annotation to a deployment

<details><summary>show</summary>
<p>

```bash
kubectl annotate deploy nginx mycompany.com/someannotation="chad"
```

</p>
</details>

### Add a label to a pod

<details><summary>show</summary>
<p>

```bash
kubectl label pods nginx env=prod
```

</p>
</details>

### Show labels for all pods in the cluster

<details><summary>show</summary>
<p>

```bash
kubectl get pods --show-labels
# or get pods with the env label
kubectl get po -L env
```

</p>
</details>

### List all pods that are in the running state using field selectors

<details><summary>show</summary>
<p>

```bash
kubectl get po --field-selector status.phase=Running
```

</p>
</details>

### List all services in the default namespace using field selectors

<details><summary>show</summary>
<p>

```bash
kubectl get svc --field-selector metadata.namespace=default
```

</p>
</details>

### List all API resources in your Kubernetes cluster

<details><summary>show</summary>
<p>

```bash
kubectl api-resources
```

</p>
</details>

### List the services on your Linux operating system that are associated with Kubernetes

<details><summary>show</summary>
<p>

```bash
systemctl list-unit-files --type service --all | grep kube
```

</p>
</details>

### List the status of the kubelet service running on the Kubernetes node

<details><summary>show</summary>
<p>

```bash
systemctl status kubelet
```

</p>
</details>

### Use the imperative command to create a pod named nginx-pod with the image nginx, but save it to a YAML file named pod.yaml instead of creating it

<details><summary>show</summary>
<p>

```bash
kubectl run nginx --image nginx-pod --dry-run=client -o yaml > pod.yaml
```

</p>
</details>

### List all the services created in your Kubernetes cluster, across all namespaces

<details><summary>show</summary>
<p>

```bash
kubectl get svc -A
```

</p>
</details>

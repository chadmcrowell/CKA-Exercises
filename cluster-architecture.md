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

### Setup alias for kubectl

<details><summary>show</summary>
<p>

```bash
alias k=kubectl
# have this persist beyond the current shell
echo 'alias k=kubectl' >> ~/.bashrc
source ~/.bashrc
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

### Perform the command to list all API resources in your Kubernetes cluster

<details><summary>show</summary>
<p>

```bash
kubectl api-resources
```

</p>
</details>

### View the client certificate that the kubelet uses to authenticate to the Kubernetes API

<details><summary>show</summary>
<p>

```bash
# The kubelet’s client certificate is named `kubelet-client-current.pem` and is stored locally on the control plane node
# on the cka exam, make sure to ssh to the control plane node first
ls /var/lib/kubelet/pki/

# view the certificate file `kubelet-client-current.pem` with openssl CLI
openssl x509 -in /var/lib/kubelet/pki/kubelet-client-current.pem -text -noout
```
</p>
</details>

### Create a new user named “Sandra”, first creating the private key, then the certificate signing request, then using the CSR resource in Kubernetes to generate the client certificate.

<details><summary>show</summary>
<p>

```bash
# create a private key using openssl with 2048-bit encryption
openssl genrsa -out sandra.key 2048

# create a certificate signing request to give to the Kubernetes API
openssl req -new -key sandra.key -subj "/CN=sandra" -out sandra.csr

# store the file `sandra.csr` in an environment variable named "REQUEST"
export REQUEST=$(cat sandra.csr | base64 -w 0)

# create the CSR as a Kubernetes resource
cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: sandra
spec:
  groups:
  - developers
  request: $REQUEST
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
EOF

# list the requests in the Kubernetes cluster
k get csr

# approve the csr resource
k certificate approve sandra

# extract the client certificate from the approved csr
k get csr sandra -o jsonpath='{.status.certificate}' | base64 -d > sandra.crt

```

[Try this in Killercoda's Kubernetes Lab Environment](https://killercoda.com/chadmcrowell/course/cka/kubernetes-create-user)

</p>
</details>

### Add that new user `sandra` to your local kubeconfig using the kubectl config command

<details><summary>show</summary>
<p>

```bash
# set the credentials in your existing kubeconfig (~.kube/config)
k config set-credentials carlton --client-key=sandra.key --client-certificate=sandra.crt --embed-certs

# view the kubeconfig to see sandra added
k config view

# get your current context
k config get-contexts

# set the context for sandra
k config set-context sandra --user=sandra --cluster=kubernetes

# switch to using the `sandra` context
kubectl config use-context sandra
```
</p>
</details>

### Upgrade the control plane components using kubeadm. When completed, check that everything, including kubelet and kubectl is upgrade to version 1.31.6

<details><summary>show</summary>
<p>

```bash
# list the control plane components at their current version and target version
kubeadm upgrade plan

# apply the upgrade to 1.31.6
kubeadm upgrade apply v1.31.6

# optionally upgrade kubeadm 
# this is if you get the message "Specified version to upgrade to "v1.31.6" is higher than the kubeadm version "v1.31.0". Upgrade kubeadm first using the tool you used to install kubeadm"

# Download the public signing key for the Kubernetes package repositories
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Add the appropriate Kubernetes apt repository
# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

# update kubeadm to version 1.31.6-1.1
sudo apt install -y kubeadm=1.31.6-1.1

# try again to upgrade the control plane components using kubeadm
kubeadm upgrade apply v1.31.6 -y

# run kubeadm upgrade plan again to verify that everything is upgraded to 1.31.6
kubeadm upgrade plan
```
</p>
</details>


### Create a role the will allow users to get, watch, and list pods and container logs

<details><summary>show</summary>
<p>

Create a role just using the kubectl command line
```bash
# create a role using `kubectl create role -h` for help
kubectl create role pod-reader --verb=get,watch,list --resource=pods,pods/log
```

Create a role from a YAML file named `role.yaml`
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

# EXTRA CREDIT: After you've created the role binding, use `kubectl auth can-i..` to test the role

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

# create the role binding from YAML
kubectl create -f role-binding.yaml
```

Test the role binding using `kubectl auth can-i`
```bash
# see if the user "dev" can list pods
kubectl auth can-i get pods --namespace=default --as=dev

# see if the user "dev" can view pod logs
kubectl auth can-i get pods/log --namespace=default --as=dev

# check that the user "dev" cannot create pods
kubectl auth can-i create pods --namespace=default --as=dev

```

</p>
</details>

### Create a new role named `sa-creator` that will allow creating service accounts.

<details><summary>show</summary>
<p>

```bash
# use kubectl to create the role, with the help of `kubectl create role -h`
kubectl create role sa-creator --verb=create --resource=sa
```

</p>
</details> 

### Create a role binding that is associated with the previous `sa-creator` role, named `sa-creator-binding` that will bind to the user `sandra`

<details><summary>show</summary>
<p>

```bash
# use kubectl to create the role binding, with the help of `kubectl create rolebinding -h`
kubectl create rolebinding sa-creator-binding --role=sa-creator --user=sandra

# use `kubectl auth can-i` to verify sandra can create service accounts
kubectl auth can-i create sa --as sandra
```

</p>
</details>

### Create a role named `deployment-reader` in the `cka-20834` namespace, and allow it to get and list deployments.

<details><summary>show</summary>
<p>

```bash
# create the namespace `cka-20834`
k create ns cka-20834

# create the role in the `cka-20834` namespace with the help of `kubectl create role -h`
k -n cka-20834 create role deployment-reader --verb=get,list --resource=deploy --api-group=apps
```

</p>
</details>

### Create a role binding named `deployment-reader-binding` in the `cka-20834` namespace that will bind the `deployment-reader` role to the service account `demo-sa`

<details><summary>show</summary>
<p>

```bash
# create a service account named `demo-sa` in the `cka-20834` namespace
k -n cka-20834 create sa demo-sa

# create the role binding with the help of `kubectl create rolebinding -h`
k -n cka-20834 create rolebinding deployment-reader-binding --role=deployment-reader --serviceaccount=cka-20834:demo-sa

# verify the permission with `kubectl auth can-i`
kubectl auth can-i get deployments --as=system:serviceaccount:cka-20834:demo-sa -n cka-20834
kubectl auth can-i list deployments --as=system:serviceaccount:cka-20834:demo-sa -n cka-20834

```

</p>
</details>

### Permanently save the namespace `ggcka-s2` for all subsequent kubectl commands in that context.

<details><summary>show</summary>
<p>

```bash
kubectl config set-context --current --namespace=ggcka-s2
```

</p>
</details>

### Set a context utilizing a specific `cluster-admin` user in `default` namespace

<details><summary>show</summary>
<p>

```bash
# set context gce to user "admin" in the namespace "default"
kubectl config set-context gce --user=cluster-admin --namespace=default \

# use the context
kubectl config use-context gce
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

### List the pods in all namespaces

<details><summary>show</summary>
<p>

```bash
kubectl get po --all-namespaces
# or
k get po -A
```

</p>
</details>

### List all pods in the `default` namespace, with more details

<details><summary>show</summary>
<p>

```bash
kubectl -n default get pods -o wide
```

</p>
</details>

### List a deployment named `nginx-deployment`

<details><summary>show</summary>
<p>

```bash
kubectl get deployment nginx-deployment
# or
kubectl -n default get deploy nginx-deployment
```

</p>
</details>

### List all pods in the default namespace

<details><summary>show</summary>
<p>

```bash
kubectl -n default get pods
# or
k -n default get po
```

</p>
</details>

### Get the pod YAML from a pod named `nginx`

<details><summary>show</summary>
<p>

```bash
kubectl get po nginx -o yaml
# or
k -n default get po -o yaml
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

### Get pod logs from a pod named `nginx`

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

### List services in the default namespace, sorted by name

<details><summary>show</summary>
<p>

```bash
kubectl -n default get services --sort.by=.metadata.name
# or
k -n default get svc --sort.by=.metadata.name
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

### Create a service account and pod that does NOT mount the service account token

<details><summary>show</summary>
<p>

Create the service account
```bash
# create the YAML for a service account named 'secure-sa' with the '--dry-run=client' option, saving it to a file named 'sa.yaml'
kubectl -n default create sa secure-sa --dry-run=client -o yaml > sa.yaml 

# add the automountServiceAccountToken: false to the end of the file 'sa.yaml'
echo "automountServiceAccountToken: false" >> sa.yaml

# create the service account from the file 'sa.yaml'
kubectl create -f sa.yaml

# list the newly created service account
kubectl -n default get sa
```

Creat a pod that uses the service account
```bash
# create the YAML for a pod named 'secure-pod' by using kubectl with the '--dry-run=client' option, output to YAML and saved to a file 'pod.yaml'
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
spec:
  serviceAccountName: secure-sa
  containers:
  - image: nginx
    name: secure-pod
EOF

# watch the 'secure-pod' pod waiting until the pod is running before proceeding
kubectl get po -w
```

Ensure the service account token was not mounted
```bash
# get a shell to the pod and try to list the contents of the token
kubectl exec secure-pod -- cat /var/run/secrets/kubernetes.io/serviceaccount/token
```

You should get the following, indicating that the token was not mounted
```bash
cat: /var/run/secrets/kubernetes.io/serviceaccount/token: No such file or directory
```

[Try this in Killercoda's Kubernetes Lab Environment](https://killercoda.com/chadmcrowell/course/cka/create-sa-for-pod)

</p>
</details>



---

## FIND MORE KILLERCODA CKA EXAM EXERCISES
[Killercoda.com](https://killercoda.com), the same company that brought you the [exam simulator](https://killer.sh) for the CKA Exam, brings you a free [Kubernetes](https://kubernetes.io/) lab environment.
[MORE CKA EXAM EXERCISES HERE](https://killercoda.com/cka)
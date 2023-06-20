# Workloads & Scheduling (15%)

kubernetes.io > Documentation > Reference > kubectl CLI > [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

kubernetes.io > Documentation > Tasks > Monitoring, Logging, and Debugging > [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Accessing Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) using API

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Use Port Forwarding to Access Applications in a Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)

### Create a deployment from a YAML file named deploy.yml

<details><summary>show</summary>
<p>

```bash
kubectl apply -f deploy.yml
```

</p>
</details>

### Describe a pod named nginx

<details><summary>show</summary>
<p>

```bash
kubectl describe po nginx
```

</p>
</details>

### Delete a pod named nginx

<details><summary>show</summary>
<p>

```bash
kubectl delete po nginx
```

</p>
</details>

### Create a deployment named nginx and use the image nginx

<details><summary>show</summary>
<p>

```bash
kubectl create deploy nginx --image=nginx
```

</p>
</details>

### Create the YAML specification for a deployment named nginx, outputting to a file named deploy.yml

<details><summary>show</summary>
<p>

```bash
kubectl create deployment nginx --image=nginx --dry-run -o yaml > deploy.yml
```

</p>
</details>

### Create a configmap named my-configmap with two values, one single line and one multi-line

<details><summary>show</summary>
<p>

```bash
# create a file named my-configmap.yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  key1: Hello, world!
  key2: |
    Test
    multiple lines
    more lines

# create the confimap from the file my-configmap.yml
kubectl apply -f my-configmap.yml

# view the configmap data in the cluster
kubectl describe configmap my-configmap
```

</p>
</details>

### Create a secret via yaml that contains two base64 encoded values

<details><summary>show</summary>
<p>

```bash
# create two base64 encoded strings
echo -n 'secret' | base64

echo -n 'anothersecret' | base64

# create a file named secret.yml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  secretkey1: <base64 String 1>
  secretkey2: <base64 String 2>

# create a secret
kubectl create -f secretl.yml
```

</p>
</details>

### Apply the label “disk=ssd” to a node. Create a pod named “fast” using the nginx image and make sure that it selects a node based on the label “disk=ssd”

<details><summary>show</summary>
<p>

```bash
# label the node named 'node01'
kubectl label no node01 "disk=ssd"

# create the pod YAML for pod named 'fast'
kubectl run fast --image nginx --dry-run=client -o yaml > fast.yaml
```

```yaml
# fast.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: fast
  name: fast
spec:
  nodeSelector: ### ADD THIS LINE
    disk: ssd   ### ADD THIS LINE
  containers:
  - image: nginx
    name: fast
```

</p>
</details>


### Edit the “fast” pod (created above), changing the node selector to “disk=slow.” Notice that the pod cannot be changed, and the YAML was saved to a temporary location. Take the YAML in /tmp/ and apply it by force to delete and recreate the pod using a single imperative command

<details><summary>show</summary>
<p>

```bash
# edit the pod
kubectl edit po fast
```

```yaml
# edit fast pod
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: fast
  name: fast
spec:
  nodeSelector:
    disk: slow  ### CHANGE THIS LINE
  containers:
  - image: nginx
    name: fast
```

```bash
# output will look similar to the following:
# :error: pods "fast" is invalid
# A copy of your changes has been stored to "/tmp/kubectl-edit-136974717.yaml"
# error: Edit cancelled, no valid changes were saved.

# replace and recreate the pod
k replace -f /tmp/kubectl-edit-136974717.yaml --force
```

</p>
</details>

### Create a new pod named “ssd-pod” using the nginx image. Use node affinity to select nodes based on a weight of 1 to nodes labeled “disk=ssd”. If the selection criteria don’t match, it can also choose nodes that have the label “kubernetes.io/os=linux”

<details><summary>show command</summary>
<p>

```bash
# create the YAML for a pod named 'ssd-pod'
kubectl run ssd-pod --image nginx --dry-run=client -o yaml > pod.yaml
```

</p>
</details>

<details><summary>show pod YAML</summary>
<p>

```yaml
# pod.yaml file
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ssd-pod
  name: ssd-pod
spec:
############## START HERE ############################
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/os
            operator: In
            values:
            - linux
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: disk
            operator: In
            values:
            - ssd
############## END HERE ############################
  containers:
  - image: nginx
    name: ssd-pod
```

</p>
</details>

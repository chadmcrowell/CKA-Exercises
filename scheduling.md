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
# create the yaml file
kubectl create deploy my-deployment --image nginx --dry-run=client -o yaml > deploy.yml

# create the resource from the yaml spec
kubectl apply -f deploy.yml
```

</p>
</details>

### Describe a pod named nginx

<details><summary>show</summary>
<p>

```bash
# create a pod named nginx
k run nginx --image nginx

# describe the pod
k describe po nginx
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

### Update the `nginx` deployment to use at new image tag `1.27.4-alpine-slim`

<details><summary>show</summary>
<p>

```bash
# list the deployments
k get deploy

# patch the deployment
kubectl set image deploy nginx nginx=nginx:1.27.4-alpine-slim

# verify that the new image is set
k get deploy nginx -o yaml | grep image:
```

</p>
</details>


### Create a configmap named `my-configmap` with two values, one single line and one multi-line

<details><summary>show</summary>
<p>

```bash
# create a configmap with a siingle line and a multi-line
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  single: "This is a single line value"
  multi: |
    This is a multi-line value.
    It spans multiple lines.
    You can include as many lines as needed.
EOF

# view the configmap data in the cluster
kubectl describe cm my-configmap


```

</p>
</details>

### Use the configMap `my-configmap` in a deployment named `my-nginx-deployment` that uses the image `nginx:latest` mounting the configMap as a volume

<details><summary>show</summary>
<p>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        volumeMounts:
        - name: config-volume
          mountPath: /etc/config
          readOnly: true
      volumes:
      - name: config-volume
        configMap:
          name: my-configmap
```

</p>
</details>

### Use the configMap `my-configmap` as an environment variable in a deployment named `mynginx-deploy` that uses the image `nginx-latest`, passing in the single line value as an environment variable named `SINGLE_VALUE` and the multi-line value as `MULTI_VALUE`

<details><summary>show</summary>
<p>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mynginx-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        env:
        - name: SINGLE_VALUE
          valueFrom:
            configMapKeyRef:
              name: my-configmap
              key: single
        - name: MULTI_VALUE
          valueFrom:
            configMapKeyRef:
              name: my-configmap
              key: multi

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

### Using kubectl, create a secret named `admin-pass` from the string `SuperSecureP@ssw0rd`

<details><summary>show</summary>
<p>

```bash
# create a secret from the string `SuperSecureP@ssw0rd`
kubectl create secret generic admin-pass --from-literal=password=SuperSecureP@ssw0rd
```

</p>
</details>

### Inject the secret `admin-pass` into a deployment named `admin-deploy` as an environment variable named `PASSWORD`

<details><summary>show</summary>
<p>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: admin-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: secret-app
  template:
    metadata:
      labels:
        app: secret-app
    spec:
      containers:
        - name: my-container
          image: nginx
          env:
            - name: PASSWORD
              valueFrom:
                secretKeyRef:
                  name: my-secret
                  key: password
```

</p>
</details>

### Use the secret `admin-pass` inside a deployment named `secret-deploy` mounting the secret inside the pod at `/etc/secret/password`

<details><summary>show</summary>
<p>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secret-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: secret-app
  template:
    metadata:
      labels:
        app: secret-app
    spec:
      containers:
        - name: my-container
          image: nginx
          volumeMounts:
            - name: secret-volume
              mountPath: "/etc/secret"
              readOnly: true
      volumes:
        - name: secret-volume
          secret:
            secretName: my-secret
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

### Create a pod named “limited” with the image “httpd” and set the resource requests to 1 CPU and “100Mi” for memory

<details><summary>show</summary>
<p>

```bash
# create the yaml for a pod
k run limited --image httpd --dry-run=client -o yaml > pod.yaml
```

Add the YAML for resources requests to the YAML file. Here is the complete file.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: limited
spec:
  containers:
  - name: httpd
    image: httpd
    resources:
      requests:
        cpu: "500m"
        memory: "100Mi"
```

Create the pod from the YAML file
```bash
# create the pod from `pod.yaml` file
k create -f pod.yaml

# list the pods to see the pod is now running
k get po

```

</p>
</details>

---

## FIND MORE KILLERCODA CKA EXAM EXERCISES
[Killercoda.com](https://killercoda.com), the same company that brought you the [exam simulator](https://killer.sh) for the CKA Exam, brings you a free [Kubernetes](https://kubernetes.io/) lab environment.
[MORE CKA EXAM EXERCISES HERE](https://killercoda.com/cka)
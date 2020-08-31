# Cluster Maintenance (11%)

kubernetes.io > Documentation > Reference > kubectl CLI > [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

kubernetes.io > Documentation > Tasks > Monitoring, Logging, and Debugging > [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Accessing Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) using API

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Use Port Forwarding to Access Applications in a Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)

### Install the metrics server add-on and view resource usage by a pods and nodes in the cluster

<details><summary>show</summary>
<p>

```bash
# install the metrics server
kubectl apply -f https://raw.githubusercontent.com/linuxacademy/content-cka-resources/master/metrics-server-components.yaml

# verify that the metrics server is responsive
kubectl get --raw /apis/metrics.k8s.io/

# create a file named my-pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: metrics-test
spec:
  containers:
  - name: busybox
    image: radial/busyboxplus:curl
    command: ['sh', '-c', 'while true; do sleep 3600; done']

# create a pod from the my-pod.yml file
kubectl apply -f my-pod.yml

# view resources usage by the pods in the cluster
kubectl top pod

# view resource usage by the nodes in the cluster
kubectl top node
```

</p>
</details>
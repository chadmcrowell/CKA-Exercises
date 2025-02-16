# Troubleshooting (30%)

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

In cluster “ik8s”, in a namespace named “db08328”, create a deployment with the kubectl command-line (imperatively) named “mysql” with the image “mysql:8”. List the pods in the “db08328” namespace to see if the pod is running. If the pod is not running, view the logs to determine why the pod is not in a healthy state. Once you’ve collected the necessary log information, make the necessary changes to the pod in order to fix the pod and get the pod back up in a running healthy state.

Run the command k run testbox --image busybox --command 'sleep 3600' to create a new pod named “testbox”. See if the container is running or not. Go through the decision tree to find out why and fix the pod so that it’s running.

Create a new container named “busybox2” that uses the image “busybox:1.35.0”. Check if the container is in a running state. Find out why the container is failing and make the corrections to the pod yaml to get it to a running state.

Create a new container named “curlpod2” that uses the image “nicolaka/netshoot” while opening a shell to it upon creation. While a shell is open to the container, run nslookup on the kubernetes service. Exit out of the shell and see why the container is not running. Fix the container, so that it continues to run.

In cluster “ik8s”, in a namespace named “ee8881”, create a deployment with the kubectl command-line (imperatively) named “prod-app” with the image “nginx”. List the pods in the “ee8881” namespace to see if the pod is running. Run the command `curl https://raw.githubusercontent.com/chadmcrowell/acing-the-cka-exam/main/ch_08/kube-scheduler.yaml --silent --output /etc/kubernetes/manifests/kube-scheduler.yaml` to make a change to the kube scheduler simulating a cluster component failure. Now, scale the deployment from 1 replica to 3. List the pods again and see if the additional 2 pods in the deployment are running. Find out why the two additional pods are not running and fix the scheduler so that the containers are in a running state again.

Move the file kube-scheduler.yaml to the /tmp directory with the command mv /etc/Kubernetes/manifests/kube-scheduler.yaml /tmp/kube-scheduler.yaml.

Create a pod with the command k run nginx –image nginx. List the pods and see if the pod is in a running status

Determine why the pod is not starting by looking at the events and the logs. Determine what the fix will be and get the pod back in a running state.

Run the command `curl https://raw.githubusercontent.com/chadmcrowell/acing-the-cka-exam/main/ch_08/10-kubeadm.conf --silent --output /etc/systemd/system/kubelet.service.d/10-kubeadm.conf; systemctl daemon-reload; systemctl restart kubelet`

Check the status of kubelet, and go through the troubleshooting steps to resolve the problem with the kubelet service.

In cluster “ik8s”, run the command `k replace -f https://raw.githubusercontent.com/chadmcrowell/acing-the-cka-exam/main/ch_08/kube-proxy-configmap.yaml` `–``force` to purposely insert a bug in the cluster. Immediately after that, delete the “kube-proxy” pod in the kube-system namespace (it will automatically recreate). List the pods in the namespace, and see that the kube-proxy pod is in an error state. View the logs to determine why the kube-proxy pod is not running. Once you’ve collected the necessary log information, make the necessary changes to the pod in order to fix the pod and get the pod back up in a running healthy state.

In cluster “ik8s”, in a namespace named “kb6656”, run the command `k apply -f` `https://raw.githubusercontent.com/chadmcrowell/acing-the-cka-exam/main/ch_08/deploy-and-svc.yaml` to create a deployment and service in the cluster. This is an nginx application running on port 80, so try to reach the application by using curl to reach the IP address and port of the service. Once you realize that you cannot communicate with the application via curl, try to see why. Make the necessary changes to the reach the application using curl and return the nginx welcome page.



---

## FIND MORE KILLERCODA CKA EXAM EXERCISES
[Killercoda.com](https://killercoda.com), the same company that brought you the [exam simulator](https://killer.sh) for the CKA Exam, brings you a free [Kubernetes](https://kubernetes.io/) lab environment.
[MORE CKA EXAM EXERCISES HERE](https://killercoda.com/cka)
# Cluster Maintenance (11%)

kubernetes.io > Documentation > Reference > kubectl CLI > [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

kubernetes.io > Documentation > Tasks > Monitoring, Logging, and Debugging > [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Accessing Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) using API

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Use Port Forwarding to Access Applications in a Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)

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
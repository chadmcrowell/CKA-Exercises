# Core Concepts (13%)

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
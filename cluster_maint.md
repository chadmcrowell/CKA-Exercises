# Cluster Maintenance (11%)

kubernetes.io > Documentation > Reference > kubectl CLI > [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

kubernetes.io > Documentation > Tasks > Monitoring, Logging, and Debugging > [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Accessing Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) using API

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Use Port Forwarding to Access Applications in a Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)

### Drain a node for maintenance named `node1.mylabserver.com`

<details><summary>show</summary>
<p>

```bash
kubectl drain node1.mylabserver.com --ignore-daemonsets --force
```

</p>
</details>

### Put the node `node1.mylabserver.com` back into service, so pods can be scheduled to it

<details><summary>show</summary>
<p>

```bash
kubectl uncordon node1.mylabserver.com
```

</p>
</details>

### Updgrade kubeadm to version 1.18.6

<details><summary>show</summary>
<p>

```bash
sudo apt install -y kubeadm --allow-change-held-packages kubeadm=1.18.6-00
```

</p>
</details>

### Plan and upgrade the control plane components with kubeadm to version 1.18.6

<details><summary>show</summary>
<p>

```bash
sudo kubeadm upgrade plan

sudo kubeadm upgrade apply v1.18.6
```

</p>
</details>

### Update kubelet to version 1.18.6

<details><summary>show</summary>
<p>

```bash
sudo apt install kubelet=1.18.6-00
```

</p>
</details>

### Update kubectl to version 1.18.6

<details><summary>show</summary>
<p>

```bash
sudo apt install kubectl=1.18.6-00
```

</p>
</details>

### Restart kubelet on the node

<details><summary>show</summary>
<p>

```bash
sudo systemctl daemon-reload

sudo systemctl restart kubelet
```

</p>
</details>

### Upgrade the kubelet configuration on a worker node

<details><summary>show</summary>
<p>

```bash
sudo kubeadm upgrade node
```

</p>
</details>
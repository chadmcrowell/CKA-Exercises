[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

# CKA Exercises

> IMPORTANT EXAM CHANGES COMING FEBRUARY 10th, 2025
[Read more about the CKA exam changes coming February 18th, 2025](cka-changes-2024/README.md)

[![cka-badge](https://training.linuxfoundation.org/wp-content/uploads/2019/03/logo_cka_whitetext-300x293.png)](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/)

A set of exercises to help you prepare for the [Certified Kubernetes Administrator (CKA) Exam](https://www.cncf.io/certification/cka/)

The format of the exam is entirely in a hands-on, command-line environment. You can take the exam at home or in a testing center, and you must complete the exam in 180 minutes. Register for the exam [here](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/). The cost is US $395.00.

As of December 2021, over 32,000 people have taken the CKA exam since it's introduction in 2017!

To complete the exercises in this repo and get even more practice with exam-like scenarios in a **FREE** Kubernetes lab environment, go to [killercoda.com](https://killercoda.com/chadmcrowell)

During the exam, you will have access to six different clusters (below) in the following configurations:

| Cluster | Members                | CNI      | Description                        |
| :------ | :--------------------- | :------- | :--------------------------------- |
| k8s     | 1 master\, 2 worker    | flannel  | k8s cluster                        |
| hk8s    | 1 master\, 2 worker    | calico   | k8s cluster                        |
| bk8s    | 1 master\, 1 worker    | flannel  | k8s cluster                        |
| wk8s    | 1 master\, 2 worker    | flannel  | k8s cluster                        |
| ek8s    | 1 master\, 2 worker    | flannel  | k8s cluster                        |
| ik8s    | 1 master\, 1 base node | loopback | k8s cluster \- missing worker node |

[Important Instructions - Official Exam Handbook](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad#cka-and-ckad-environment)

[FAQ: CKA - Official Exam Handbook](https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks)

Also during the exam, you may have one and ONLY one of the following tabs open at all times:  
[kubernetes.io/docs](https://kubernetes.io/docs/home/)  
[kubernetes.io/blog](https://kubernetes.io/blog/)

Not sure if you have the right equipment to take the exam at home? [Run a system check](https://www.examslocal.com/ScheduleExam/Home/CompatibilityCheck)

## Contents

- [Cluster Architecture, Installation & Configuration - 25%](cluster-architecture.md)
- [Workloads & Scheduling - 15%](scheduling.md)
- [Services & Networking - 20%](networking.md)
- [Storage - 10%](storage.md)
- [Troubleshooting - 30%](troubleshooting.md)

[View the most current exam curriculum](https://github.com/cncf/curriculum)

## Additional Grading Information

The CKA exam is graded for outcome only (i.e. end state of the system). The path that a exam taker may have taken to get to the outcome is not evaluated, meaning an exam taker can take any path they want as long as it achieves the correct outcome. Incomplete work, i.e. work that is input but did not lead to the correct outcome, will not be evaluated.

Some exam items may have multiple parts and therefore have multiple 'checks' (one for each verifiable component of the answer). Candidates will be given credit for each successful check, so partial credit is possible on such items.

Exam items are also setup to be independent of one another.  As long as the candidate does exactly what the questions ask, there will be no dependencies or conflicts, and as long as the candidate correctly achieves the outcome being asked for in the specific exam item, they would earn points for that particular exam item.

Scoring is done using an automatic grading script. The grading scripts have been time tested and continuously refined, so the likelihood of having incorrectly graded a question or two is very low since we grade on outcomes (end state of the system), not the path the user took to get there.

## Updates - as of October 2022

The CKA exam is currently on v1.31 of k8s. The removal of dockershim happend in v1.24, so expect the containerd container runtime if you are taking the exam today and into the future. You can view the container runtime in use with the command `k get no -o wide`. The output will look similar to this (see the column named "CONTAINER-RUNTIME"):
```bash
NAME           STATUS   ROLES           AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
controlplane   Ready    control-plane   34d   v1.28.1   172.30.1.2    <none>        Ubuntu 20.04.5 LTS   5.4.0-131-generic   containerd://1.6.12
node01         Ready    <none>          34d   v1.28.1   172.30.2.2    <none>        Ubuntu 20.04.5 LTS   5.4.0-131-generic   containerd://1.6.12
```


## Exam Release Cycle
The exams are upgraded to the latest version of k8s within 4-6 weeks of the version being released. [Dockershim FAQ](https://kubernetes.io/blog/2020/12/02/dockershim-faq/)

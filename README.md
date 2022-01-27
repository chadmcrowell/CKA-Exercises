[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

# CKA Exercises

[![cka-badge](https://training.linuxfoundation.org/wp-content/uploads/2019/03/logo_cka_whitetext-300x293.png](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/)

A set of exercises to help you prepare for the [Certified Kubernetes Administrator (CKA) Exam](https://www.cncf.io/certification/cka/)

The format of the exam is entirely in a hands-on, command-line environment. You can take the exam at home or in a testing center, and you must complete the exam in 180 minutes. Register for the exam [here](https://www.cncf.io/certification/cka/). The cost is US $300.00.

During the exam, you will have access to six different clusters (below) in the following configurations:

| Cluster | Members                | CNI      | Description                        |
| :------ | :--------------------- | :------- | :--------------------------------- |
| k8s     | 1 master\, 2 worker    | flannel  | k8s cluster                        |
| hk8s    | 1 master\, 2 worker    | calico   | k8s cluster                        |
| bk8s    | 1 master\, 1 worker    | flannel  | k8s cluster                        |
| wk8s    | 1 master\, 2 worker    | flannel  | k8s cluster                        |
| ek8s    | 1 master\, 2 worker    | flannel  | k8s cluster                        |
| ik8s    | 1 master\, 1 base node | loopback | k8s cluster \- missing worker node |

[source](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad#cka-and-ckad-environment)

Also during the exam, you may have one and ONLY one of the following tabs open at all times:  
[kubernetes.io](https://kubernetes.io/docs/home/)  
[github.com/kubernetes](https://github.com/kubernetes/)  
[kubernetes.io/blog](https://kubernetes.io/blog/)

Not sure if you have the right equipment to take the exam at home? [Run a system check](https://www.examslocal.com/ScheduleExam/Home/CompatibilityCheck)

## Contents

- [Cluster Architecture, Installation & Configuration - 25%](core_concepts.md)
- [Workloads & Scheduling - 15%](scheduling.md)
- [Services & Networking - 20%](networking.md)
- [Storage - 10%](storage.md)
- [Troubleshooting - 30%](troubleshooting.md)

[View the most current exam curriculum](https://github.com/cncf/curriculum)

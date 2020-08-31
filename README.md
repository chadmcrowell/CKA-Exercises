[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

# CKA Exercises
[![a-cloud-guru](https://imgrepopublic.blob.core.windows.net/img/Twitter-3.jpg)](https://acloud.guru)

A set of exercises to help you prepare for the [Certified Kubernetes Administrator (CKA) Exam](https://www.cncf.io/certification/cka/) 

The format of the  exam is entirely in a hands-on, command-line environment. You can take the exam at home or in a testing center, and you must complete the exam in 180 minutes. Register for the exam [here](https://identity.linuxfoundation.org/pid/699). The cost is US $300.00. 

These exercises are supplemental to and follow the [CKA course](https://linuxacademy.com/course/cloud-native-certified-kubernetes-administrator-cka/) on Linux Academy. If you don't have a cluster to practice on, the Hands-on Labs at Linux Academy are the quickest way to get started. Also, I created this [k8s cheat sheet](https://acloudguru.com/blog/engineering/kubernetes-cheat-sheet) to help you memorize commands.

During the exam, you will have access to six different clusters (below) in the following configurations:

| Cluster | Members | CNI | Description |   
| :--- | :--- | :--- | :--- |
| k8s | 1 master\, 2 worker | flannel | k8s cluster |
| hk8s | 1 master\, 2 worker | calico | k8s cluster |
| bk8s | 1 master\, 1 worker | flannel | k8s cluster |
| wk8s | 1 master\, 2 worker | flannel | k8s cluster |
| ek8s | 1 master\, 2 worker | flannel | k8s cluster |
| ik8s | 1 master\, 1 base node | loopback | k8s cluster \- missing worker node |

[source](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad#cka-and-ckad-environment)

Also during the exam, you may have one and ONLY one of the following tabs open at all times:  
[kubernetes.io](https://kubernetes.io/docs/home/)  
[github.com/kubernetes](https://github.com/kubernetes/)  
[kubernetes.io/blog](https://kubernetes.io/blog/)

Not sure if you have the right equipment to take the exam at home? [Run a system check](https://www.examslocal.com/ScheduleExam/Home/CompatibilityCheck)

## Contents

- [Core Concepts - 19%](core_concepts.md)
- [Installation, Configuration & Validation - 12%](install_configure_validate.md)
- [Cluster Maintenance - 11%](cluster_maint.md)
- [Networking - 11%](networking.md)
- [Scheduling - 5%](scheduling.md)
- [Application Lifecycle Management - 8%](app_lifecycle.md)
- [Storage - 7%](storage.md)
- [Security - 12%](security.md)
- [Logging/Monitoring - 5%](logging_monitoring.md)
- [Troubleshooting - 10%](troubleshooting.md)

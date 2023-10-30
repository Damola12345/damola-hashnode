---
title: "Resolving "The Node Had Condition [Node Disk-Pressure]" & "The Node Was Low On Resource: Ephemeral-storage" In Kubernetes"
datePublished: Mon Oct 30 2023 00:45:45 GMT+0000 (Coordinated Universal Time)
cuid: cloc6i94k000309l9dbw2fhca
slug: resolving-the-node-had-condition-node-disk-pressure-the-node-was-low-on-resource-ephemeral-storage-in-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1698625730328/77a0450a-370e-4244-89a5-e5c9c912d194.png
tags: docker, aws, kubernetes, devops, containers

---

Kubernetes is a powerful platform for managing containerized workloads, but it can be challenging to ensure that your applications are running efficiently and without issue. One common issue that you may encounter is `Node Disk-Pressure & Node Was Low On Resource`, which occurs when a node in your Kubernetes cluster runs out of disk space. In this blog post, we'll explore how to identify and resolve Node Disk-Pressure issues in your  Kubernetes environment.

### **Identifying The Issue**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697726221541/c1050d70-84aa-42f9-99fb-4e8eb9800773.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1698626360665/4e3ab82b-db6b-442d-8cda-f299c78e1cc2.jpeg align="center")

A pod was evicted because of DiskPressure issues, and upon inspecting the specific pod, two prevalent issues were observed:

1. [Node conditions](https://kubernetes.io/docs/concepts/scheduling-eviction/node-pressure-eviction/#node-conditions): `[DiskPressure]`
    
2. The node was low on resources: ephemeral-storage.
    

To identify the source of the Node Disk-Pressure issue, there are a couple of different methods you can use. One way is to use the Kubernetes `Kubectl top` command to see which processes are consuming the most disk space on your node. Another way is to SSH into the node and run the `df -h` command to check the disk usage on the node and identify which directories are consuming the most disk space.

Once you've identified the processes or directories that are causing the disk usage, you can determine whether they are essential to the system or whether they can be deleted or moved to another location.  By removing unnecessary files and directories or moving them to a different disk or node, you can free up disk space and resolve the Node Disk-Pressure issue.

## **Resolving the Issue**

To resolve the Node Disk-Pressure issue, you can take several steps, such as increasing the EBS volume size, deleting unused resources, implementing resource limits, implementing pod anti-affinity, or adding more nodes to your cluster.

***In my case, I resolved the issue by deleting unused resources and increasing the EBS volume size.***

1\. One way to resolve the issue is to increase the EBS volume size. To do this, you can follow the steps outlined in the [AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html). Essentially, you will need to modify the volume size in the EC2 console or using the AWS CLI or Terraform, and then extend the file system on the node to utilize the additional space.

2\. Another way to free up disk space and resolve the Node `Disk-Pressure` issue is to delete any unused resources on your node. This can include pods, images, or volumes that are no longer required. By removing these resources, you can free up additional disk space on the node and prevent future `Disk Pressure` issues.

3\. Finally, if none of the above steps works, you may need to consider adding more nodes to your cluster to distribute the workload across a larger number of machines. This will reduce the likelihood of any one node experiencing `Disk-Pressure` issues.

## **Conclusion**

Node Disk-Pressure can be a challenging issue to resolve, but by following the steps outlined in this blog post, you can ensure that your Kubernetes cluster is running efficiently and that your applications can run successfully without being evicted due to Disk-Pressure issues. It's important to monitor your  Kubernetes environment and take proactive steps to prevent Disk-Pressure issues from occurring in the first place.
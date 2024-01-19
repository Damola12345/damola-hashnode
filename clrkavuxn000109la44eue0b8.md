---
title: "Kubernetes Affinity Basics"
datePublished: Fri Jan 19 2024 07:09:35 GMT+0000 (Coordinated Universal Time)
cuid: clrkavuxn000109la44eue0b8
slug: kubernetes-affinity-basics
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705629869292/9883990a-1e39-4d62-8994-2810612e33e5.png
tags: kubernetes, scheduling, cluster, pods

---

In Kubernetes, affinity is like a set of rules that decide where your pods go. Think of pods as small units of your application. There are two types:

* ***Node Affinity***: Decides where to put pods.
    
* ***Pod Affinity***: Decides where to group pods together.
    

These rules help you control how pods are placed based on specific conditions.

## ***Node Affinity VS Pod Affinity***

**Node Affinity**

***RequiredDuringSchedulingIgnoredDuringExecution***: Pods must follow these rules to be scheduled on a node. If the rules are not met, the pod won't be placed on that node. But once scheduled, the pod won't be moved even if the rules are broken.

***PreferredDuringSchedulingIgnoredDuringExecution***: These rules are preferred, but not mandatory. The scheduler will try to follow them, but if it can't, the pod might still be placed on a node that breaks the rules.

Here's an example of a node affinity rule that requires a pod to be scheduled on a node with a specific label. Think of node affinity like choosing where your pod should live.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705485265172/134ea3f1-a45b-479f-b585-e25b96a6db68.png align="center")

**Pod Affinity**

***RequiredDuringSchedulingIgnoredDuringExecution***: Similar to node affinity, but it's about pods wanting to be on the same node as other pods. Rules must be satisfied for the pods to be scheduled together on a node.

***PreferredDuringSchedulingIgnoredDuringExecution***: Similar to preferred node affinity, but at the pod level.

Here's an illustration of a pod affinity guideline below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705485223885/7459b9b1-ead6-430f-95ff-ab67c349aa6d.png align="center")

In conclusion, Affinity rules in Kubernetes use labels (key-value pairs) to guide where pods should be placed in the cluster. By setting these rules, you enhance efficiency and performance, essentially telling Kubernetes where you prefer your pods to be based on the labels you've defined. This optimization helps in better resource utilization, improves performance, and ensures that related pods share the same nodes for efficient communication. For more details, check out the [official documentation](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity). It's a powerful tool for managing workload placement in Kubernetes.
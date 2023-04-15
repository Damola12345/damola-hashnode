---
title: "CONTAINERS ON AWS (EKS vs ECS vs Fargate vs EC2)"
datePublished: Sun Jul 31 2022 13:47:06 GMT+0000 (Coordinated Universal Time)
cuid: cl69dmmoe0abty6nv95lr1jdp
slug: containers-on-aws-eks-vs-ecs-vs-fargate-vs-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1659275063204/NQpI-CoK8.jpeg
tags: aws, devops, containers, ecs, eks

---

**Why are containers so popular?** 

The core reason is that containers are easy to scale and can be added or subtracted quickly from an  environment, it is easy to break complex monolithic applications into smaller, modular microservices.  Systems that are used to require expensive, dedicated hardware resources can now share hardware with  other systems. In addition, containers are self-contained and portable. If a container works on one host,  it will work just as well on any other, so long as that host provides a compatible runtime. 

AWS puts forward a lot of alternatives when it comes to deploying container and container orchestration  services inclusive of EKS, ECS, Fargate, EC2 and Kubernetes, explaining the role each service plays in  container hosting (runs them) and orchestration (manages containers) on AWS.
 
- Hosting Layer (EC2, Fargate) 
- Orchestration Layer (ECS, EKS, Kubernetes) 

In this post, we’ll analyse three eminent services in AWS (ECS, EKS, Fargate), container security, and how  you can choose which service is best for you. 

**ECS vs EKS**
 
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5n5r6jjmzkvariwdz9id.jpeg)

It’s essential to understand cloud terminology by distinguishing the services. Container management tools  can be grouped into the following: 
- Registry services (manage and store container images) 

- Orchestration services (manage when and where you can run containers) 

- Compute services (powers containers) 

**_The Orchestration Layer_**  

Container orchestration automates the provisioning, deployment, networking, scaling, availability, and  lifecycle management of containers. If you work with containers, you’ll virtually want to use an  orchestration tool. The question is **“which of the tools”**? 

**What is Amazon ECS (Elastic Container Service)?**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6wjuaic5o7nertga2yel.png)
This is AWS own container service. ECS, you can fully manage container orchestration i.e. focus on  application management, building and it completely automates cluster management (ECS Clusters). This  means that you don’t have to install, monitor, scale or operate your cluster because everything is taken  care of by the service.

_There are seven core components to ECS_: 
 - Cluster
 - Containers and images 
 - Container agent 
 - Task definition 
 - Task
 - Service

**What is Amazon EKS (Amazon Elastic Kubernetes Service)?**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ds6ep3oagkxy1ymd7myb.png)

EKS manages the Kubernetes applications within the AWS ecosystem, furthermore you can use EKS to  fully utilize Kubernetes without the need to manage it yourself. AWS also automates Kubernetes cluster  provisioning and administration, allowing customers to focus on their business and applications rather  than wasting valuable time on Kubernetes cluster provisioning and maintenance. 

**What is Kubernetes?**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/si2dtgx4tlh0zl6f2juj.png)

Kubernetes is a popular DevOps tool and an open-source container orchestration service. Using  Kubernetes on AWS brings a lot of benefits which include easy managing and full compatibility with the  Kubernetes environment (clusters that run on-premises or on other cloud vendor platforms can be moved  to EKS with no modification). 

**Features ECS vs EKS**

_ECS_

- Makes it easy for you to create an event-driven system that integrates tightly with other Amazon components and services. In a nutshell, choose ECS if you want to use the AWS  ecosystem, including DynamoDB, SQS, ASG, SNS, CloudTrail, CloudWatch, ECR and more. 

- ECS gets more configuration options compared to EKS and it automatically recovers unhealthy  containers to ensure you have the required number of containers needed to run your application  smoothly. 

_EKS_
  
- EKS also excels at providing an efficient and cost-effective solution. It is well architectured,  providing you the necessary means to scale your application, gives you the necessary portability  that you may need over your environment. 

- EKS also provides more granular control over how containers are managed, but more control  leads to greater complexity. 

- EKS uses Kubernetes, it is more flexible, meaning you could migrate your workload to another  platform easily, making it more suitable for complex multi-cloud workloads.

**_The Hosting Layer_**  

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qupbu7xpvw45zfbmadbv.png)

The hosting layer of your container deployment represents the virtual machines on which containers are  hosted on. 

**What is EC2(Elastic Compute Cloud)?** 

EC2 is a highly scalable, secure, resizable compute capacity, high performance container management  service for Docker containers running on EC2 instances. It can also be used to provide a Kubernetes  cluster. 

**What Is Fargate?**

Fargate is a serverless infrastructure platform for containers. It means the user doesn’t have to concern  themselves with managing and paying for servers. Instead, they pay for just the compute resources  containers consume. 

**Features of EC2 vs Fargate**

_EC2_

- EC2 allows operators to take control of the underlying infrastructure for container workloads.  These are referred to as container instances and are fully configurable. Users can define the  instance types, storage, memory capacities as well as setting scaling parameters using Auto  Scaling Groups. This ownership approach means that teams are responsible for configuring them to run containers securely, and then manage both the containers and the EC2 instances. 

_Fargate_

- Fargate is a serverless engine that offloads the burden of managing infrastructure from users,  allowing software teams to focus on optimising their container applications. 

- The scalability and availability of your container applications are managed by AWS. This works  especially well for workloads that suit serverless tasks which will probably become the norm in  years to come. 
 
**Similarities and Differences** 

ECS and EKS are both AWS-managed services with a specific focus on containers and microservice  applications. There are few essential differences and similarities between them which are listed below 

- **_Security_**: AWS provides a standard level of security for all their services. ECS and EKS both have  access to AWS IAM, the access control system through which you can limit access to ECS tasks or  EKS pods. 

- **_Pricing_**: Both ECS and EKS services - AWS charges for resources used by your applications. This  means that if you allocate EC2 or Fargate, you will be charged according to those services costs  because you run your ECS tasks or pods on them. However, if you use EKS, you will be charged  $0.10 per hour approximately $72 per month for each EKS cluster you have. If you are just getting  started or exploring microservices, then ECS might make more sense for you to use cost-wise. 

- **_Compatibility and Portability_**: EKS is a managed Kubernetes service that you can run on any  infrastructure, from on-premises to all cloud vendors. it's designed to work anywhere, but it’s not  the same for ECS; ECS is designed and served solely for workloads running on the AWS. 
When it comes to portability i.e, moving between cloud vendors with minimal disruption, EKS is  based on an open-source Kubernetes that makes portability workable, in other words, ECS is an  exclusive AWS service that is not available for other infrastructures. 

- **_Community Support_**: Support is one of the most important aspects to consider when comparing  two services. Due to its open-source platform, EKS provides an extensive community and support  system such as _Github posts_, _Slack channels_, _Stack Overflow_, _blogs_, _tutorials_. ECS, on the other  hand, doesn’t have that type of support because of its exclusive nature. If you run into issues and  need assistance, you will have to reach out to AWS corporate support. 

- **_Management_**: Choices between these services may vary depending on your expertise and  knowledge. ECS is a simple service for developers to manage, but EKS is a complete control plane. 

**AWS Container Security Best Practices** 

- Using IAM Roles 
- Using Security Groups 
- Secrets Management (SSM Parameter Store) 
- Make use of images from a secure container repository
- Log & Monitoring

**Conclusion**

In summary, both ECS and EKS are good options depending on your situation and organisation’s needs. If  you are developing and operating large-scale projects where many teams will collaborate on several  deployments and products simultaneously or your team already has Kubernetes-native expertise, then  EKS is a better option. On the other hand, ECS is a great option if you are looking for a free control plane  and new to containerisation and microservices.

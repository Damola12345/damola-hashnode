---
title: "Ansible Made Easy"
datePublished: Sun Oct 02 2022 17:18:20 GMT+0000 (Coordinated Universal Time)
cuid: cl8rlwxzi000l09jpc1b7ggd4
slug: ansible-made-easy
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1664717068426/Ph4WUyzIq.png
tags: aws, ansible, devops, beginners, 2articles1week

---

This piece will walk you around the definition of Ansible, how it works and the problems it solves, and a simple  tutorial using Ansible to install these applications on 3 Ubuntu servers - curl, http, nginx, apache. 

## Ansible 

[Ansible](https://www.ansible.com/#) is an open-source automation configuration management and application deployment tool written in  Python. It helps to reduce managerial overhead by automating the deployment of the application and managing IT  infrastructure. 

![Screenshot 2022-09-25 at 12.11.50.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664712313944/KTzH4zPoQ.png align="left")

It runs on UNIX-like systems and can provision and configure both UNIX-like and Windows systems. Ansible does  not require an agent to run on a target system, unlike other automation software. It leverages on the SSH connection  and python interpreter to perform the given tasks on the target system. 

Ansible can be installed on a cloud server to manage other cloud servers from a central location, or it can also be  configured to use on a personal system to manage cloud or on-premises systems. 

## How it works 
Ansible is all about automation, it needs a directive to achieve all tasks and it is easy and simple to do version  control. 

Ansible consists of `control nodes` and `host nodes`. Ansible is installed on the `control nodes`, while the `host nodes` is  managed by the `control nodes`. It uses no agents and it's so easy to deploy, above all it uses a simple language  (**YAML** in the form of Ansible Playbooks).

## Problems It Solves

![Screenshot 2022-09-25 at 12.11.21.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664712370752/_97pbEdfu.png align="left")

According to the [official documentation](https://www.ansible.com/overview/it-automation), Ansible delivers simple IT automation that ends repetitive tasks and frees up  DevOps teams for more strategic work. 

Next, is a simple tutorial on Ansible and the aim of this tutorial is to install these applications on 3 Ubuntu servers  `curl, nginx, apache`. Check out this [repo](https://github.com/Damola12345/nautilus-devops/tree/master/week5).


![ansible.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1664712665927/eBf8HULHR.jpg align="left")

- The first step is to launch an EC2 instance(ubuntu) and name it `Ansible-master`. 

- Update the repository  cache by running the command.
```
sudo apt update && sudo apt upgrade
```

- Run the following command to install the latest version of Ansible on `Ansible-master`.
```
sudo apt install  ansible -y ansible –version
```

## Generate SSH Key Pair 

We can connect to remote hosts using a password through Ansible. It is recommended to set up key-based  authentication for easy and secure logins. 

SSH into the server and run this command to create key pairs on the ansible-master .
```
ssh username@host.com 
cd ~/.ssh 
ssh-keygen -t rsa -b 2048
```

Run the `ls` command to see two keys generated in the .ssh folder. The public key contains a .pub extension  while the private key doesn’t. 
```
ls
```
Copy & paste the public key into a newly Import key pair on the console.
```
cat xx.pub 
ansible key

```

![Screenshot 2022-10-02 at 13.22.21.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664713405634/muThlnpiA.png align="left")

Launch three new servers and use the newly created key pair `ansible key`.

![Screenshot 2022-08-02 at 18.38.47.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664714102749/nH1G2m0PC.png align="left")

Test connection (login to the server by running the command without asking for a password). 
```
ssh  ubuntu@54.xxx.8.xxx
```

Create `inventory file` (contains IP address assigned to servers). 

![Screenshot 2022-10-02 at 13.28.26.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664713805279/p6aDWeucW.png align="left")

Create playbook.

![Screenshot 2022-10-02 at 13.30.54.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664713888192/09y3-_Wwg.png align="left")

Create  `ansible.cfg` file.

![Screenshot 2022-10-02 at 13.28.42.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664713817441/U8Bip5RBt.png align="left")

We have done the minimal configuration required to connect to the remote machine using Ansible. Run the  following command on (ansible-master) to ping the host using Ansible ping module. 
```
ansible all --key-file  ~/.ssh/id_rsa -i inventory -m ping
```
![Screenshot 2022-08-02 at 18.35.38.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664714044442/_Vt5uSqKn.png align="left")

Command to execute the playbook.
```
ansible-playbook -i inventory playbook.yml
```
![Screenshot 2022-08-02 at 18.41.01.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664714070556/LIA6LX6kQ.png align="left")

## Summary 
Ansible automates everything across your IT infrastructure and makes it look simple. 
We learned how to install Ansible on Ubuntu and also saw how to connect to remote servers using SSH key-based  authentication, Ran some simple Ansible commands to connect to the servers. You can learn more about Ansible from the documentation hosted at [Ansible Documentation](https://docs.ansible.com/) and also checkout this blog on [Ansible](https://spacelift.io/blog/ansible-tutorial).

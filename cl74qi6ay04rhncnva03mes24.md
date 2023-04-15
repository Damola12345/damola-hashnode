---
title: "How To Manage And Store Terraform State File On Terraform Cloud"
datePublished: Mon Aug 22 2022 12:28:25 GMT+0000 (Coordinated Universal Time)
cuid: cl74qi6ay04rhncnva03mes24
slug: how-to-manage-and-store-terraform-state-file-on-terraform-cloud
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1661171008679/02iI88_Ax.jpeg
tags: devops, terraform, terraform-state, terraform-cloud, 4articles4weeks

---

In this piece, I will be discussing how we can keep our terraform state file using terraform cloud and access it effortlessly.

![R-state-file](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g9cmnf2d7xjmkwupg31w.jpg)

## What is a State File?

The state file is an artifacts that you’re left with once an Infrastructure as Code framework finishes a deployment. It contains what was deployed, where it was deployed, and all the configuration needed to deploy it. (i.e. it contains a ton of sensitive data). This isn’t the kind of file you just want to put on an open file system somewhere. If this file is exposed, then there's a big problem. The state file is important for deployment, furthermore, it is referenced during any update, performance or redeployment to look for
differences between what the state file says is deployed, and what is actually deployed.

## Need For Terraform Remote States and Its Benefits?

According to the documentation, by default, Terraform stores state locally in a file named **_terraform.tfstate_**. This file contains a custom JSON format that records a mapping from the Terraform resources in your templates to the representation of those resources in the real world.

When working with Terraform in a team, use of a local file makes Terraform usage complicated because each user must make sure they always have the latest state data before running Terraform and make sure that nobody else runs Terraform at the same time.

Best practice with remote state, Terraform writes the state data to a remote data store, which can then be shared between all members of a team.
Remote state is implemented by a backend or by Terraform Cloud, both of which you can configure in your configuration's root module.

## Benefits

- **_Security_**: local state file save the content in plain text. It is very common to have secrets in the state, so local state files are insecure. Remote state resolves this issue.
- **_Safe storage_**: Storing state on the remote server helps prevent sensitive information from leaking.
- **_Collaboration_**: State keeps track of the version of an applied configuration, and it's stored in a remote.
- **_Auditing_**: Invalid access can be identified by enabling logging.
- **_Idempotence_**: Whenever a Terraform configuration is applied, Terraform checks if there is an actual change made. Only the resources that are changed will be updated.
- **_Deducing dependences_**: Terraform maintains a list of dependencies in the state file so that it can properly deal with dependencies that no longer exist in the current configuration.

## Terraform Remote State Storage

Terraform advocates storing state file in various cloud providers listed below, but for this article i will be discussing about Terraform cloud
- Terraform Cloud
- Amazon S3
- Azure Blob Storage 
- Google Cloud Storage 
- HashiCorp Consul, etc.

## Terraform Cloud

In Terraform Cloud, your resources are organized by workspaces, which contain your resource definitions, environment and input variables, and state files. A Terraform operation occurs within a workspace, and Terraform uses the configuration and state for that workspace to modify your infrastructure. In addition, changes in your infrastructure are tracked and can also be viewed in the web-based user interface.

Terraform Cloud supports three workflows for your Terraform runs:
- CLI-driven workflow
- API-driven workflow
- UI/Version Control System(VCS)-driven workflow

In this tutorial, I will be making use of a **_CLI-driven workflow_**. Let's delve into it.

## Prerequisites

- A Terraform Cloud account.
- Terraform installed in local ( terraform -cli) 

## Steps
- Create an Organization and workspace in Terraform Cloud
- Authentication with Terraform Cloud
- Create Configuration files

## Create an Organization and workspace in Terraform Cloud

Creating a new organization and workspace will help you organize your Terraform Cloud resources and users under a single organization. Below is a screenshot of me creating an organization called **_arterycloud_** and a workspace called **_Nautilus_**.

![workspace](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pek42svb8t6rcoza3l1e.png)

![org](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j6koboojqia9uzvfzpti.png)

## Set Variable

Click on the Variables tab in the workspace’s **_Nautilus_** settings and click Add variable to add variables and tick the Sensitive box, and click on the Save variable to add the variable to your workspace. For real-world configurations, we can add any cloud platform credentials and any other configuration variables to the workspace. But in this tutorial I'm making use of _AWS credentials_.

![variable](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xjmgs7llcj28ciin13xa.png)

## Authentication with Terraform Cloud
From creating an account on Terraform Cloud, it’s now time to set up our backend.But before we do that , we've to create a _Terraform Cloud API_ token and use the **_API token_** to authenticate with **_Terraform Cloud_**.

![Api](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mr6rwxanbgzpilgav4bj.png)

This token ensures that nobody else can access or change the infrastructure without authorization. In order to authenticate with Terraform Cloud, run the _terraform login_ command and type _yes_ and press _Enter_ when prompted.

![Token](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0mho7j447balv6y76eau.png)
Enter your API token when prompted to Enter a value

Once login is successful you will get the following output

![output](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pwd5vo6r0sfyqgdubv6h.png)

## Create Configuration files

Copy and paste the below content into your _Backend.tf_

![Backend](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ucrnvnqd7por2cs3vyxc.png)

Run the `terraform fmt` command to automatically format the source code to make it human-readable.

Run the `terraform validate` command to validates the configuration files in a directory.

Next, run the `terraform init` command to initialize the working directory.

Run the `terraform plan` command to generate an execution plan, showing you what actions will be taken without actually performing the planned actions.

Run the `terraform apply` command to Creates or updates infrastructure depending on the configuration files.

Navigate back to your Terraform Cloud main page, then click on the workspace’s to access your workspace overview.

![state](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2jo6rb08r2nzt2p3hijh.png)

![Resources](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0ri4ku1xrh1g5r64e1bd.png)

To get started with terraform cloud visit [ Terraform Documentation](https://learn.hashicorp.com/tutorials/terraform/cloud-workspace-configure?in=terraform/cloud-get-started)

## Conclusion 
In this tutorial, you’ve learned how to keep **_Terraform state_** into Terraform Cloud by setting up a Terraform Cloud environment. In addition, when working with Terraform as a team, it’s best to decide where you’re going to keep those state files for safekeeping. Many 3rd party platforms that offer remote state backend options. It’s most suitable to set up a remote state as multiple people want to update the same state file and it's not easy to work with multiple copies of differing state files.
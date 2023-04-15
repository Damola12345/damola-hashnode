---
title: "Deploy A Simple Application To AWS EC2"
datePublished: Sun Oct 02 2022 17:34:40 GMT+0000 (Coordinated Universal Time)
cuid: cl8rmhydg000n09jpfb8nekl1
slug: deploy-a-simple-application-to-aws-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1664667372254/M5ckcNyPe.png
tags: aws, devops, beginners, 2articles1week, github-actions-1

---


This piece walks you through how to automate a simple Nodejs App to AWS EC2 via GitHub Actions  leveraging third-party actions on GitHub Marketplace. 

![ec2-gh.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1664667305230/DaJpwExlP.jpg align="left")

To learn more about [GitHub Actions](https://damolaaji.hashnode.dev/automate-android-build-using-github-actions) and [AWS EC2](https://aws.amazon.com/ec2/) check out this article. 

## Create Nodejs App 

The basic way to set up a nodejs app is to check out [NodeJS](https://nodejs.org/en/) and [NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) websites respectively, make a download  and install. Furthermore, check out this [repo](https://github.com/Damola12345/nautilus-devops/tree/master/week7) to guide you through creating a nodejs app.

## Spin Up EC2 With Terraform 

Create a `ec2.tf`

```
terraform {
  cloud {
    organization = "arterycloud"

    workspaces {
      name = "Nautty-dev"
    }
  }
}

provider "aws" {
  region  = var.region
  profile = "terraform-user"
}

resource "aws_instance" "web" {
  ami           = "ami-xxxxxxxxxxxx"
  instance_type = "t2.micro"
  count         = 1

  tags = {
    Name = "Nautty-dev"
  }
}

variable "region" {
  default     = "us-east-1"
  description = "AWS Region"
  type        = string
}
```
## Generate SSH key 

In this case, we'll SSH into the `ec2 instance` and generate a key. Checkout this [step](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to generate a new SSH Key.

- Add public Key to `authorized key` and the main reason for doing this is to add the  (xx.pub) to `authorized key` so Github Actions using the  private key can access the server. 

Run this command to append the public key to the `authorized key`.
```
cat xxx.pub  >> authorized keys
```
- Add private key to repository’s secrets .

![Secret-key](https://cdn.hashnode.com/res/hashnode/image/upload/v1664667849223/6sPK-diZC.png align="left")

## Set up Workflow to Deploy to EC2 Instance 

Look at this [article](https://damolaaji.hashnode.dev/automate-android-build-using-github-actions) to learn about creating a workflow.

```
name: EC2 CI/CD

on:
  push:
    branches: [ "ft/week7_cicd" ]

defaults:
  run:
    working-directory: ./week7/

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x]
        
    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
        cache-dependency-path: './week7/package-lock.json'
    - run: npm i

    - name: Deploy to EC2 instance
      uses: easingthemes/ssh-deploy@main
      env:
        SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}
        REMOTE_HOST: ${{ secrets.REMOTE_HOST }}
        REMOTE_USER: ${{ secrets.REMOTE_USER }}
        TARGET: ${{ secrets.TARGET }}
        SOURCE: ""

    - name: Executing remote ssh commands using ssh key
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.REMOTE_HOST }}
        username: ${{ secrets.REMOTE_USER }}
        key: ${{ secrets.SSH_PRIVATE_KEY }}
        script: |
            # sudo npm install pm2 -g && pm2 update
            cd /home/ubuntu/nautilus-devops/week7
            # sudo pm2 start index.js --name=week7 
            sudo pm2 restart week7
```

The code above  does a few things.

- Creates a workflow named EC2 CI/CD. 

- Checks out  the code.

- Set up dependencies and requirements for our build, configuring cache for npm build system.

- Deploy to EC2 instance using the [easingthemes/ssh-deploy action](https://github.com/easingthemes/ssh-deploy), which is a third-party action  available on the [GitHub marketplace](https://github.com/marketplace/actions/ssh-deploy), helps in deploying the code to the server using `ssh` and perform `rsync` form the runner.

- Executing remote ssh commands using [appleboy/ssh-action@master](https://github.com/appleboy/ssh-action).

## Summary 
This article helps you understand how you can automatically deploy your code to AWS EC2 from GitHub.

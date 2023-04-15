---
title: "Local AWS Cloud With LocalStack"
datePublished: Thu Sep 08 2022 13:03:37 GMT+0000 (Coordinated Universal Time)
cuid: cl7t28xkz073mcxnv9yui27je
slug: local-aws-cloud-with-localstack
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1662642502796/5X8dIUzWV.jpeg
tags: docker, aws, beginners, amazon-s3, 4articles4weeks

---

In this article, I will be discussing localstack, which I got to know and learn while working on an [open source project](https://github.com/ngneat/nx-serverless) and the task was meant to add an example usage of S3 using localstack. 

## What is LocalStack? 

![stack](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vwztvoekwz1abcc684e2.jpeg)

A fully functional local AWS cloud stack which provides an easy-to-use test/mocking framework for  developing Cloud applications. It's an open-source mock of the real AWS services, it produces a testing  environment on our local machine that emulates the AWS cloud environment with the same APIs as the  real AWS services. 

LocalStack has three different tier: 
- Community Edition 
- Pro 
- Enterprise 

Each tier has its services and features. Pro and Enterprise require payment, while Community Edition is  free for personal usage. 

## Why Use LocalStack? 

The procedure of briefly using a test/mock system in place of actual ones is a common way of running tests  for applications with external dependencies. In other words LocalStack executes a test system of our AWS  services and its primary objective is to assist in speeding up various procedures, saving money on  development testing. 

## Problems LocalStack Solves 
- Cost effective testing 
- Available as Docker image 
- Local/offline testing 
- Continuous integration 
- Easy to use 
- Increases dev speed 
- Running applications without connecting to AWS. 
- Avoid the complexity of AWS configuration and focus on development.
 
## Requirements 

LocalStack is a Python designed application and its main aim is overriding the AWS endpoint URL with  the URL of LocalStack. Furthermore, LocalStack runs inside a Docker container, but can also run as a  Python application. 

- python (Python 3.7 up to 3.10 supported) 
- pip (Python package manager) 
- Docker 

For installing run this command
```
pip install localstack 
```

We can also run LocalStack directly as a Docker image either with Docker run or with Docker-compose.  Before you start running Localstack, ensure that Docker service is up and running. I would highly recommend you Checkout [LocalStack repository](https://github.com/localstack/localstack) 

Start Localstack in Docker with this command 
```
localstack start -d 
```
Check Service status 
```
localstack status services 
```

## Setting up Localstack 

To start running localstack in the project, you should have Docker and Docker-compose installed or make use of Docker Desktop to create containers locally.

Create a Docker-compose YAML file at the root of the project

```
version: '3.8'

services:
  localstack:
    container_name: '${LOCALSTACK_DOCKER_NAME-localstack_main}'
    image: localstack/localstack
    network_mode: bridge
    ports:
      - '127.0.0.1:4510-4559:4510-4559' # external service port range
      - '127.0.0.1:4566:4566' # LocalStack Edge Proxy
    environment:
      - AWS_DEFAULT_REGION=eu-west-1
      - DEBUG=1
      - DATA_DIR=/tmp/localstack/data
      - SERVICES=s3
    volumes:
      - '/private/tmp/localstack:/tmp/localstack'
      - '/var/run/docker.sock:/var/run/docker.sock'
```
In this `docker-compose.yml`, we set the environment variable SERVICES to the name of the services we want to use (S3).

Run the below command to get `docker-compose.yml` file up and running.

```
docker-compose up -d
```

## Add an example usage of S3

AWS S3 is a program that’s built to store, protect, and retrieve data from “buckets” at any time from anywhere on any device.

Let’s start by creating a directory at the root of the project and installing the NPM package.

```
mkdir localstack-s3
cd localstack-s3
npm init
npm install --save aws-sdk

```
The code below provides a summary of the `upload.js` file. You can checkout the code [nx-serverless](https://github.com/Damola12345/nx-serverless/blob/ft/nx_s3_localstack/localstack-s3/upload.js)

```
const AWS = require("aws-sdk");
const fs = require('fs')

const KEY_ID = "nxserverless"
const SECRET_KEY = "nxserverless"

const BUCKET_NAME = "nxboy";

const s3 = new AWS.S3({
  region: "us-east-1",
  accessKeyId: KEY_ID,
  secretAccessKey: SECRET_KEY,
  endpoint: 'http://localhost:4566', // This is the localstack EDGE_PORT
  s3ForcePathStyle: true,
});

const params = {
  Bucket: BUCKET_NAME
}

// To create an s3 bucket 
s3.createBucket(params,(err, data)=>{
  if(err){
    console.log(err)
  }
  else{
    console.log("Bucket created successfully", data.Location);
  }
})

// To upload a file to s3 bucket 
const uploadFile = (filename) => {
  const filecontent = fs.readFileSync(filename);

  const params = {
    Bucket: BUCKET_NAME,
    Key: "",
    Body: filecontent,
    ContentType: ""
  }

  s3.upload(params,(err, data)=>{
    if(err){
      console.log(err)
    }
    else{
      console.log("File uploaded successfully", data.Location)
    }
  })
}

uploadFile("")
```
Run the below command to create an s3 bucket
```
node upload.js
```

![upload.js](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fennjbmui3hx0q23snjx.png)
Run the below command to list s3 bucket created

```
aws --endpoint-url=http://localhost:4566 s3 ls
```

![localstack](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/atelbmrxiqu30r77k61c.png)

## Conclusion
LocalStack provides an environment to test and develop your application and infrastructure with AWS services locally.

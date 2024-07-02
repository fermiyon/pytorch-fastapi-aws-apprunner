![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)

By Selman Karaosmanoglu 

## Date created
2 July 2024

# FastAPI PyTorch Deployment on AWS

## Overview

This project demonstrates how to deploy a PyTorch model using FastAPI on AWS App Runner. It provides a scalable and efficient way to serve machine learning models. The project includes detailed instructions for building a Docker image on AWS Cloud9 and deploying it to AWS App Runner via AWS Elastic Container Registry (ECR). 

The PyTorch project was converted from the official PyTorch tutorial using Flask by Noah Gift.

Tasks:

1.  Run the fastapi app:  `python app.py`
2.  Upload an image to classify, say a cat using the swagger docs url:  /docs
3.  Deploy to AWS and the AWS App Runner service

## Notes on Running docker with PyTorch and FastAPI

```bash
docker build .
```

Note use: `docker images` to find (replace id with the image id):

```bash
docker run -p 127.0.0.1:8080:8080 54a55841624f
```

## Build, Push, Deploy Docker Image on AWS

## Getting Started

### Create AWS Cloud9 Environment

![sc](resources/1-create-cloud9-env.png)å

![sc](resources/2-create-cloud9-env.png)

### Create Amazon Elastic Container Registry

![sc](resources/3-ecr.png)

![sc](resources/4-ecr.png)

![sc](resources/5-ecr.png)

### Build and Push Docker Image on AWS

#### Clone Git Repository to AWS Cloud9

```bash
git clone ..
```

#### Create virtual environemnt

```bash
python3 -m venv ~/.venv
```

#### Activate environment

```bash
source ~/.venv/bin/activate`
```

![sc](resources/6-cloud9.png)

#### Increase AWS Cloud9 Volume Size

Go to the settings of the EC2 instance of Cloud9 and increase the volume size

![sc](resources/7-ec2-volume-size.png)

#### Reboot Cloud9

```bash
sudo reboot
```

![sc](resources/8-reboot-cloud9.png)

- Check volume size and build docker image

```bash
df --human-readable
```

```bash
docker build -t pytorch .
```

![sc](resources/9-cloud9-docker-build.png)
 
#### Run an instance of the Docker Image and preview it

```bash
docker images
```

```bash
docker run -p 127.0.0.1:8080:8080 <docker image id>
```

Click Preview button of AWS Cloud9 and go to /docs page of the FastAPI app in preview pane as shown in the screenshot below

![sc](resources/10-cloud9-run-preview.png)

#### View push commands in AWS Elastic Container Registry

![sc](resources/11-ecr-push-commands.png)

![sc](resources/12-ecr-push-commands.png)

#### Apply Docker push commands via AWS Cloud9 instance to AWS ECR

Login

```bash
aws ecr get-login-password --region eu-west-2 | docker login --username AWS --password-stdin 176839071578.dkr.ecr.eu-west-2.amazonaws.com
```

Docker Tag

```bash
docker tag pytorch:latest 176839071578.dkr.ecr.eu-west-2.amazonaws.com/pytorch:latest
```

Docker Push

```bash
docker push 176839071578.dkr.ecr.eu-west-2.amazonaws.com/pytorch:latest
```

![sc](resources/13-cloud9-push-commands.png)

#### Check Amazon Elastic Container Registry

![sc](resources/14-aws-ecr-images.png)

### Create AWS App Runner Service

![sc](resources/15-aws-app-runner.png)

![sc](resources/16-aws-app-runner.png)

![sc](resources/17-aws-app-runner.png)

### Verify Swagger Working

Go to /docs url

![sc](resources/18-success.png)


### References

* [Resize Cloud9 Instance Root Volume](https://ec2spotworkshops.com/ecs-spot-capacity-providers/workshopsetup/resize_ebs.html)

* Duke University - Virtualization, Docker, Kubernetes Data Engineering Program
---
title: "Navigating Streamlined Docker container Deployment on AWS"
datePublished: Mon Jul 17 2023 05:52:04 GMT+0000 (Coordinated Universal Time)
cuid: cllbvzwce000709lbguwv2qno
slug: navigating-streamlined-docker-container-deployment-on-aws
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692078646174/f2dbaa51-7829-4d02-96fb-d04f95a48958.jpeg
tags: docker, aws, devops, ecs, docker-compose

---

As the retirement of [Docker Compose integration for ECS](https://docs.docker.com/cloud/ecs-integration/) (Amazon Elastic Container Service) and ACI (Azure Container Instances) approaches in November 2023, there arises a concern about finding suitable alternatives to enable Docker in ECS

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tijlqsotbv4bsw0u3iti.png align="left")

## So any alternative?

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eh72a88pjik1fbraugdp.gif align="left")

Fortunately, a viable alternative called [Compose-ECS](https://github.com/docker/compose-ecs) has emerged, providing an open-source solution for deploying containerised applications in ECS

## Compose ECS ( `compose-ecs`)

`compose-ecs` is CLI tool to deploy your application using docker container in AWS ECS service with just few commands.

Example, you can deploy any application or tool like Jenkins, WordPress, Angular, React and many more with just help of `docker-compose.yaml` file. You just have o select your image from https://hub.docker.com/. Create a file like below

```bash
services:
  jenkins:
    image: jenkins/jenkins:lts
    ports:
      - 8080:8080
```

If you're curious to know how it works and what the difference is between the earlier option and this, then watch this live session.

%[https://www.youtube.com/watch?v=z8WSQblxTvo] 

## Connect with us:-

Slack: https://launchpass.com/collabnix
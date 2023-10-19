---
title: "Seekable OCI - Lazy Loading Container Images on ECS and Fargate"
datePublished: Thu Oct 19 2023 05:17:14 GMT+0000 (Coordinated Universal Time)
cuid: clnwqd03m00000ami2ojj06i6
slug: seekable-oci-lazy-loading-container-images-on-ecs-and-fargate
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1697651081234/ee4baf1f-5698-44cd-bda8-4e3b54bd10cd.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1697651111846/f3f67b98-4adc-4ee5-a5a8-0c4f0b9e6a75.png
tags: docker, aws, containerization, aws-fargate, container-orchestration

---

Hello Devs,

Have you come across any situation where an ECS container is taking time to initialise a new ECS task with an ECR image. ? This mostly happens when your Docker image size is more example more than 200 MiB.

In this blog, I will explain new technology which is going to help to eliminate that worry. I got to know this concept in AWS Community Day Pune 2023 when [**Mayur Bhagia**](https://www.linkedin.com/in/mayurbhagia/) **(** Principal Solutions Architect at Amazon Web Services ) was giving a talk on "Efficient scaling: Seekable OCI for faster container startup for Amazon ECS and Fargate". This topic attracts me to do more study on this. So, here I come up with a details blog about "**SOCI**" - Seekable OCI.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697646300723/e66a9d9f-4d71-47ed-ba2d-df88364a8b43.jpeg align="center")

### Lazy loading?

You all might have heard about lazy loading in Javascript, Angular or React code. Lazy loading is loaded when it is needed in the background so that performance may not be impacted and it can help to speed up the application experience. Exactly on a similar note Seekable OCI also provides lazy loading while pulling images from Amazon ECR on ECS and Fargate. Let's talk about Seekable OCI.

### What is Seekable OCI?

Seekable OCI is an open-source technology developed by AWS that can launch containers faster by lazily loading the container image. It is also called as "**SOCI**" - Seekable OCI" " and is pronounced, "so-CHEE". SOCI works by creating an index of the files within the container image which is called as **SOCI index.**

Most of the time while launching containers download the entire container image from ECR or registry before starting the container. It is unnecessary to wait to load all the data in case only a small portion of data is needed for the startup process. A research paper published on Usenix says that pulling image take 76% of container start time, but only 6.4% of that data is needed to start the process. So it is very obvious that 76% of the time is an area to look for improvement in which process of pulling Docker images from the ECR registry. SOCI works in that highlighted area to improve the start-up time of the container.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697647716049/711aa37a-0198-40e1-b4b1-0963eb916d3a.png align="center")

### SOCI is the right option?

There are various ways to solve this problem including reducing the size of container images by using a multi-stage docker build and pre-fetching container images into local storage. But as I mentioned above SOCI supports lazy loading. Lazy loading is the approach when data is downloaded in the background while the start-up is going in parallel. Container images are stored as an ordered list of layers, and layers are most often stored as gzipped tar files.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697648421922/045f924e-0421-4489-aba0-85c903282645.jpeg align="center")

SOCI's approach for addressing this is to eliminate the need to download the entire image before launching the container and to instead lazily load data on demand, and prefetch data in the background. That is why SOCI is the right option for this problem.

### How does SOCI work?

`containerd` is a container runtime that manages the lifecycle of a container on a physical or virtual machine (a host). In `containerd` has one component which manages the container filesystem is called a `snapshotter`. The main job of a `snapshotter` is to create a folder that can be used by `containerd` to unpack a layer. `snapshotter` pulls and decompresses the entire container image before the container gets started. With the help of lazy loading snapshotter container starts without downloading the entire image content and lazily loads files from the Amazon ECR ( or any other OCI-compatible registry). When the container is started without waiting for container content to be fully downloaded it results in a shorter start time.

Before the SOCI `snapshotter` lazily loads a container image it needs to know image metadata means which files are in each layer of the image. Then only the SOCI snapshotter will be able to lazy load the container image. The SOCI index gives all required metadata to the lazy load.

### Important note

* AWS Fargate support for SOCI is available at no additional cost and you will only be charged for storing the SOCI indexes in Amazon ECR
    
* Only tasks that run on Linux platform version 1.4.0 can use SOCI indexes. Tasks that run Windows containers on Fargate aren't supported.
    
* Lazy loading with container images greater than 250 MiB compressed in size. You will likely see a reduction in smaller images.
    

### SOCI provide benefits like :

* Faster container startup times
    
* Reduced bandwidth usage
    
* Improved performance of containerised workloads
    

%[https://youtube.com/shorts/ZfZPlDE_CgU] 

I hope this information excites you to give it a try on SOCI to reduce your container start time on Amazon ECS and Fargate. Feel free to reach out to me on my Twitter handle @[@AvinashDalvi_](@AvinashDalvi_) or comment on the blog.

After reading multiple blogs I have consolidated my learning from Mayur's talk and research. Find below all references which can give more details about the whole execution.

### References :

* [https://aws.amazon.com/blogs/aws/aws-fargate-enables-faster-container-startup-using-seekable-oci/](https://aws.amazon.com/blogs/aws/aws-fargate-enables-faster-container-startup-using-seekable-oci/)
    
* [https://docs.aws.amazon.com/AmazonECS/latest/userguide/container-considerations.html#fargate-tasks-soci-images](https://docs.aws.amazon.com/AmazonECS/latest/userguide/container-considerations.html#fargate-tasks-soci-images)
    
* [https://github.com/awslabs/soci-snapshotter](https://github.com/awslabs/soci-snapshotter)
    
* [https://aws.amazon.com/blogs/containers/under-the-hood-lazy-loading-container-images-with-seekable-oci-and-aws-fargate/](https://aws.amazon.com/blogs/containers/under-the-hood-lazy-loading-container-images-with-seekable-oci-and-aws-fargate/)
    
* [https://dev.to/napicella/what-is-a-containerd-snapshotters-3eo2](https://dev.to/napicella/what-is-a-containerd-snapshotters-3eo2)
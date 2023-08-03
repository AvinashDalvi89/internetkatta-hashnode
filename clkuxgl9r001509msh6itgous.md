---
title: "Debugging into AWS ECS Task Containers: What You Need to Know"
datePublished: Thu Aug 03 2023 09:01:19 GMT+0000 (Coordinated Universal Time)
cuid: clkuxgl9r001509msh6itgous
slug: debugging-into-aws-ecs-task-containers-what-you-need-to-know
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690823531613/7d787a8a-a5ac-4ac0-b86f-7c9c6c30c004.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1690823540427/9ee111a8-e221-4ba9-a7da-6e8a8c2a58e7.png
tags: aws, debugging, ecs, awscommunity, aws-fargate

---

Hello Devs,

In [previous blog](https://www.internetkatta.com/host-wordpress-on-aws-ecs-using-fargate) and [YouTube video](https://youtu.be/fGz5znsEHpE) explained how to deploy a WordPress website using Fargate in AWS ECS.

In this blog going to learn about how to debug into AWS ECS task containers whether it is using Fargate either Serverless or with an EC2 machine.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690786660753/bb85426c-71bb-497f-a9c1-6740134b0686.png align="center")

### Why is this needed?

Debugging to ECS task is one of the most important parts of your production support activity whenever there is any incident happening in the production system and you need to know why this is happening, any code failure or why the task is getting fails. For this to achieve need to log into the ECS task container like we generally logged into the EC2 machine using the .pem file. Two types of ECS task containers can configure as per the above image.

* AWS Fargate (serverless)
    
* Amazon EC2 instances
    

Let's understand one by one both options.

### How to debug into the ECS task container using EC2?

After logging into the AWS console following steps:-

1. Go to ECS console -&gt; Select your desired ECS cluster -&gt; Select ECS service
    
2. Select the task which you want to debug -&gt; Task Configuration
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690787891771/61d1d31e-db15-48b6-9c7e-1678a10cd431.png align="center")

1. Then click on Connect under EC2 console -&gt; Logged into EC2 machine using.pem file
    
2. Once you logged into the EC2 machine run the command `docker ps -a` to see a list of containers ( There might be a case where one EC2 machine will have different others containers which you don't want to check )
    
3. Then you will get a list of containers choose your container and copy "Container ID" or you can run `docker ps -a | grep keyword` so that you can find desired task container
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691028138679/0c71289b-2e5e-474a-8b18-d9fc9eb78d0c.png align="center")
    
4. Run `docker exec -it {ContainerID} /bin/bash` replace {ContainerID} exact Container ID column value which you copied in step 5. should be like `docker exec -it ba7844220cc2 /bin/bash`
    
5. Then will be inside the task container. Here you can debug your code or check the execution log also.
    
6. If you wish to check the container log can run \`docker logs `` {ContainerID}` `` If the container is getting failed you will get to know the reason for the code error.
    

This was a much easier option to ssh into the ECS task container because EC2 was there, so that was able to log in to the server. But in the case of the Fargate Serverless option, there is no EC2 upfront which we can directly log into it. Let's explore it

### How to debug into the ECS task container for Fargate Serverless :

Important things to consider is whether the ECS task can run Amazon ECS `Exec` command or not under task definition. So you should able to run the `docker exec` command automatically and landed on the directly targeted container. To achieve this need to following things :

* Enabled ECS Exec for task definition. Setting will be `enableExecuteCommand` flag.
    
* Install SSM ( AWS Systems Manager Session Manager ) this will help us to create a tunnel into a container and your local so could able to log into the container.
    
* IAM role should have access to run `ecs:ExecuteCommand` command
    

Let's follow the below steps to achieve the same :

1. There is one utility [https://github.com/aws-containers/amazon-ecs-exec-checker](https://github.com/aws-containers/amazon-ecs-exec-checker) available to check whether `exec` command is enabled or not for Task definition. Clone this repository in your local machine
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690831546811/926e36f8-5192-41c7-b856-a6e6ec1f8779.png align="center")
    
2. Run the below command or follow the instructions given on the GitHub repository. If able to pass this step and can directly jump to the last step. Check the output of the command it might show AWS credential is missing, the SSM manager is missing etc. Follow the instructions as per the [GitHub repository](https://github.com/aws-containers/amazon-ecs-exec-checker).
    
    ```bash
    ./check-ecs-exec.sh <YOUR_ECS_CLUSTER_NAME> <YOUR_ECS_TASK_ID>
    ```
    
3. Install Session Manager plugin - The session manager plugin allows us to connect EC2 instances or AWS fargate. Follow the instructions to install the plugin on the respective OS given here [https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)
    
4. Add a policy to the IAM role which is used as a role for Task definition to access SSM
    
    ```yaml
    {
       "Version": "2012-10-17",
       "Statement": [
           {
           "Effect": "Allow",
           "Action": [
                "ssmmessages:CreateControlChannel",
                "ssmmessages:CreateDataChannel",
                "ssmmessages:OpenControlChannel",
                "ssmmessages:OpenDataChannel"
           ],
          "Resource": "*"
          }
       ]
    }
    ```
    
5. Add this policy to the same Task definition role. Otherwise, you will not be able to run `aws ecs execute-command` command.
    
    **Note**: Resource and other policies can be customised based on your security team's suggestion.
    
    ```yaml
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "User access to ECS ExecuteCommand",
                "Effect": "Allow",
                "Action": "ecs:ExecuteCommand",
                "Resource": "*"
            }
        ]
    }
    ```
    
6. Run the below command to enable `--enable-execute-command`. to enable Task definition to run Exec command.
    
    For existing services use `update-service` API for new ECS `create-service`
    
    ```bash
    aws ecs update-service \
        --cluster <clustername> \
        --task-definition <task-definition-name> \
        --service <service-name> \
        --enable-execute-command \
    ```
    
    for new ECS use `create-service` API
    
    ```bash
    aws ecs create-service \
        --cluster <cluster-name> \
        --task-definition <task-definition-name> \
        --service <service-name> \
        --desired-count 1 \
        --enable-execute-command
    ```
    
7. To confirm whether `execute` command is enabled for Task definition or not run this command.
    
    ```bash
    aws ecs describe-tasks --cluster sample-cluster --tasks 
    1a1466028dd24a97b021111c1e3da4d3
    ```
    
    If everything works fine then you will receive a similar response
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690831400964/a81cbd5a-0b03-40d6-948d-e94c784476f8.png align="center")
    
8. the final step is to log into the ECS task container. Run this command
    
    `aws ecs execute-command --cluster --task --container --interactive --command "/bin/sh"`
    
    ```bash
    aws ecs execute-command --cluster sample-cluster \
        --task 1a1466028dd24a97b021111c1e3da4d3 \
        --container sample-container \
        --interactive \
        --command "/bin/sh"
    ```
    
9. Now you are in the ECS task container console. You try to check your code reference folder or files.
    

**Important**: If you are tasks are running already before your task definition changes ( step 6) will not take place in the existing running task. You need to register a new task or deregister an existing one and try creating a new one.

%[https://www.youtube.com/watch?v=kWoL4ozFtZE] 

I hope this blog helps you to learn. Feel free to reach out to me on my Twitter handle @[@AvinashDalvi_](@AvinashDalvi_) or comment on the blog. This blog is inspired by my day-to-day production support activity and one of my followers reach out to me for help.

### References :

* [https://aws.amazon.com/blogs/containers/new-using-amazon-ecs-exec-access-your-containers-fargate-ec2/](https://aws.amazon.com/blogs/containers/new-using-amazon-ecs-exec-access-your-containers-fargate-ec2/)
    
* [https://towardsthecloud.com/amazon-ecs-execute-command-access-container](https://towardsthecloud.com/amazon-ecs-execute-command-access-container)
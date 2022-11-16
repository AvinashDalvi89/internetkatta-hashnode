# Host WordPress on AWS ECS using Fargate

Hello Devs,

In this blog I'm going to cover how to host WordPress websites on AWS ECS service using AWS Fargate. Let me tell you why this is needed and why not to use EC2 for hosting Wordpress. 

Few days back I was working with my friend to enhance the performance of the WordPress website. He was facing timeout errors frequently due to memory exhaustion. Reason behind this is he has configured cron job ( PHP scripts ) which was running every 5 minutes and eating more memory due to this EC2 was getting dead state and getting timeout. After this problem I have done some research and talked to a few AWS community builders then came to conclusion to host WordPress site on ECS cluster where can run more than one task can accommodate separate infra for running main website and another task can run separate infra for running cron jobs. In this way load can be divided and easily manageable. Both activities will not disturb each other. 

This is the reason for selecting AWS ECS service over EC2 to enhance performance and be more resilient. 

### Lets understand first perquisites: 

**Docker** - Docker helps you to separate out your application from infra so that you can deliver the final product faster. This is exactly similar to making 2 minutes of instant noodles.
While cooking instant noodles we have to open the package and put it into hot water and then after 4-5 minutes it will be ready to eat. An Exactly similar Docker container is the instant noodles package which we have to just configure to infra and run apps. More details can find here https://docs.docker.com/get-started/overview/

**ECR** - AWS ECR ( Elastic container registry ) is an AWS service which enables us to keep a registry of docker images. We can store any docker image and pull inside AWS services whenever it requires. 

**ECS** - AWS ECS ( Elastic container service ) is a service where you can deploy any docker image or host any code inside a container and enable us via 80 or 443 port to view in the browser using ALB ( Application load balancer ). This is like boiled water where you can put multiple instant noodle packets similarly in AWS ECS cluster can add more number tasks which run on each container. More details can find here https://aws.amazon.com/ecs/

**AWS Fargate** -  help you to focus on building applications without managing servers. AWS Fargate removes the operational overhead of scaling, patching, securing, and managing servers. AWS Fargate is an instant noodle masala packet which removes our headache of making noodles tastier. More details can find here https://aws.amazon.com/fargate/

![banetti-banetti-market.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1668618932653/yAcYiLJVa.gif align="left")

**AWS ECS components** :
 - Clusters
 - Service
 - Tasks 
 - Task definition
 - Container Agent


![ECS_Component.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668622151786/9Fj-tIQjx.png align="left")

### Let's understand what to do. 

Step 1 : Create ECR entry in AWS console. Go to AWS console -> ECR -> Create repository -> Click on Create

![Screenshot 2022-11-16 at 11.41.16 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668622319895/EhvliSUrH.png align="left")
![Screenshot 2022-11-16 at 11.41.28 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668622307203/rE3a1WAG8.png align="left")

Once it gets created click on "View push commands" as shown below image. 

![Screenshot 2022-11-16 at 11.44.08 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668622480120/_tu1SDQgs.png align="left")

Step 2: Run `aws configure` command on your terminal if AWS credential is not configured in your macOS or Linux machine. Make sure AWS IAM users which are going to use it should have access to ECR. Then following command 

`aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin {ACCOUNTID}.dkr.ecr.us-east-1.amazonaws.com` {ACCOUNTID} - this will be your AWS account ID. 

Output will be like : 

![Screenshot 2022-11-16 at 11.49.34 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668622802813/376pQmhUJ.png align="left")

Step 3: Get Docker image from official docker page https://hub.docker.com/_/wordpress. Run this command `docker pull wordpress` make sure your Docker desktop is installed and running on your machine. 

Step 4: Push image to ECR. 
Run this command `docker tag wordpress {ACCOUNTID}.dkr.ecr.us-east-1.amazonaws.com/sample-wordpress:latest`
then 
`docker push {ACCOUNTID}.dkr.ecr.us-east-1.amazonaws.com/sample-wordpress:latest` After this command docker image will be available under AWS ECR.  Copy this URI from the ECR console. 

![Screenshot 2022-11-17 at 12.00.04 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668623429753/eYxYOS3yn.png align="left")

Step 5: Go to AWS Console -> ECS -> Create custom cluster

![Screenshot 2022-11-16 at 11.58.42 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668623361772/mVrtxSPVs.png align="left")

Click on configure and paste the value of URI to the second field. Give the name to cluster as "sample-wordpress" and enter 80 port values. Other configurations are optional based on use case basis. 

![Screenshot 2022-11-17 at 12.01.42 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668623531717/qmY20Tnfq.png align="left")


![Screenshot 2022-11-17 at 12.08.46 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668623973520/OGylbVLCn.png align="left")
Click on edit if any change needs to be done like name of task definition, memory, cpu etc. 
![Screenshot 2022-11-17 at 12.09.22 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668623980848/AGIxFnNek.png align="left")

Click on next and select ALB mode. 

![Screenshot 2022-11-17 at 12.10.46 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668624062664/aLwVvQgrw.png align="left")

Hit the create button and wait for final completion of creation. 

![Screenshot 2022-11-17 at 12.11.23 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668624129244/viiW5Bfye.png align="left")

Once it finished all creation process it will show status like this 

![Screenshot 2022-11-17 at 12.14.00 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668624263181/RW9COpUjY.png align="left")

Now, click on view services. Review created service and their task. 

![Screenshot 2022-11-17 at 12.15.40 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668624414531/mHWsQqS0t.png align="left")


![Screenshot 2022-11-17 at 12.16.05 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668624433543/T7ygmxQ3m.png align="left")

You can click on the target group and review its target group configuration. 

![Screenshot 2022-11-17 at 12.18.33 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668624522175/oJoG6y8hr.png align="left")

Step 6 : Find out URL to check WordPress website. Go to EC2 console -> Elastic Load balancer -> Copy URL

![Screenshot 2022-11-17 at 12.20.24 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668624703132/7ZsN54KWD.png align="left")

>>Note : You need to configure a health check URL to keep the task healthy. If you don't do this task will keep creating after going to an unhealthy state due to lack of health check URL. Temporary until WordPress installation finishes you can set `/wp-admin/setup-config.php` as a health check URL.This is just a WordPress setup. For running a full WordPress website you need to set up an RDS instance for database connection. 

![Screenshot 2022-11-17 at 12.33.58 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668625453415/6JQyEvojL.png align="left")

Finished!


![Screenshot 2022-11-17 at 12.30.41 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668625578563/fhCqOA6vA.png align="left")

I hope this blog helps you to learn. Feel free to reach out to me on my twitter handle @AvinashDalvi_ or comment in the blog. If you like my blog please don't forget to like the article. It will encourage me to write more helpful articles. Stay tune for next blog will come up different use case with ECS. 


### References : 
- https://aws.amazon.com/blogs/containers/running-wordpress-amazon-ecs-fargate-ecs/


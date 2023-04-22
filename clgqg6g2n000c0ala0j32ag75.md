---
title: "How to use DataDog to find utilisation of AWS EBS volume"
seoTitle: "How to use DataDog to find utilisation of AWS EBS volume"
seoDescription: "This blog will explain how to use DataDog to find disk utilisation and integration steps."
datePublished: Fri Apr 21 2023 11:07:38 GMT+0000 (Coordinated Universal Time)
cuid: clgqg6g2n000c0ala0j32ag75
slug: how-to-use-datadog-to-find-utilisation-of-aws-ebs-volume
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682074929117/d9b72309-8dd2-4e9e-82d9-34b130a56cae.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1682074974372/41ed3940-16d8-4910-82d9-53716c540d2a.png
tags: aws, monitoring, awscommunity, datadog, aws-ebs

---

Hello Devs,

This blog will explain how to use DataDog to find disk utilisation. But before that tell me why it should be required.

While using EC2 machines we generally have attached EBS volume or extra EBS volume. You might have seen many times “No space left on device” or sometimes 100% utilisation state similar kinds of errors. On this error, you might have taken action by adding extra volume or resizing the current EBS volume. But sometimes that action we may take late because we get to know very late and this might cause customer user experience badly. Sometimes you oversize your volume and end up paying an extra cost. How to avoid this?

I will explain here how to avoid this and monitor EBS volume disk utilisation using Datadog. How it can save you from any incident and save your cost also.

Let's understand first why Disk utilisation is more important.

### Why does Disk utilisation matter?

Your disk is nothing but your hard drive or in-cloud instance volume. This is not only used for saving data or your code but it plays an important role in writing and reading operations. This has the same weightage as the CPU or RAM. Every hard disk is capable of many read-and-write operations that decide what the IOPS rate is. So it will slow down your instance or PC. Customers or end users may face issues while accessing applications. Disk utilisation can reach a limit like 90% or 100% just because code or application demands more read and write operations which you can’t reduce.

So, Disk utilisation metrics are important for consumers to know how much storage is under or over-utilised to optimise the cost of disk. And also get over any incident. Observing Disk utilisation metrics can find out what pattern of usage in the general application is expected accordingly and can modify disk size.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682072630932/bcde720e-3c82-402a-a8f1-86520734dfb8.gif align="center")

But the question is how can I get early notification whenever disk utilisation reaches some threshold? how I can know what is disk average usage? Here Datadog can help.

### DataDog ?

Datadog is one the best monitoring and log management platforms. Where you can integrate your on-premise or cloud instance or services to get all logs and metrics in one place. Datadog provides real-time monitoring and event triggering based on a metric threshold or logs message to act upon it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682133256845/619fc0de-1337-4b42-a905-eb18850b649b.png align="center")

### How can Datadog help in Disk utilisation?

Datadog provides installation agents which need to be installed inside a server or instance. That agent will keep on dumping logging data and metric data to the Datadog dashboard. Datadog has a build mechanism based on a threshold of metrics like when “CPU usage more than 80%” or “disk usage cross 70%” can trigger notification to the respective team or member of the team via email or Slack or ops genie etc. Also based on the pattern of usage you can set the scaling of the disk.

Let me take an example of AWS EBS volume. AWS EBS volume provides CloudWatch metrics that can easily integrate with Datadog. Also as said above can install an agent and fetch logs within 15 seconds of the timeframe.

### How to do this?

Let's understand how DataDog can be integrated with AWS EBS volume and fetch required metrics. Prerequisite for this you have already AWS EC2 running with the default volume attached.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682072904442/94cb90e3-9b6b-44d8-addd-2f99e166fd17.png align="center")

1. First, if you haven't an account already with Datadog then create one on Datadog.com 14 days trial period is there.
    
2. After creating an account, you have to choose which platform you are going to integrate like AWS, Google, Docker, Azure etc.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682073065823/99767f9c-4b7b-416e-a005-6305364eeb50.png align="center")
    
3. Choose an instance family-like OS.
    
4. Start following the steps mentioned on the page. Once all is complete click on Finish.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682073074055/f26bdfb7-712d-4fc3-b7c3-124ddcc85ad1.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682073268807/4dfc1b5b-1e07-4865-98c7-480c10dfd757.png align="center")
    
5. You will be redirected to https://app.datadoghq.com/ first welcome screen will look like this if you are doing it for the first time.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682073280457/49dc62d8-89fb-47b9-9567-0b281976e295.png align="center")
    
6. On the Left side, navigation click on **Metrics** -&gt; **Explorer** -&gt; **Add query**
    
7. Select disk-related query. For disk utilisation can select `system.disk.in_use` , `system.disk.free`and `system.disk.used`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682073316797/ff2e7d2c-68fb-4ed5-be3e-70b549b9c88c.png align="center")
    
8. After running the query can see metrics showing on the page. Click on “**Save to Dashboard**”
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682073610825/fa4b7201-a7d7-497e-b9b0-25a751e619a0.png align="center")
    
    .
    
9. In the search query, you can play around more like adding formulas.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682073677439/dd47bf51-e62a-45d0-a9d8-bf9f790875fe.png align="center")
    
10. Like a few other metrics you can try out as per point #7.
    
11. Under “Monitor” you can create an alert whenever disk space is going above a threshold or under-utilised. ( optional step )
    

This is how the integration of Datadog with the instance is completed and able to view EBS volume metrics and follow patterns to decide on optimising cost :)

I hope this blog helps you to learn. Feel free to reach out to me on my Twitter handle @[@AvinashDalvi_](@AvinashDalvi_) or comment on the blog.

### References :

* [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using\_cloudwatch\_ebs.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cloudwatch_ebs.html)
    
* [https://www.datadoghq.com/blog/amazon-ebs-monitoring/](https://www.datadoghq.com/blog/amazon-ebs-monitoring/)
    
* [https://docs.datadoghq.com/getting\_started/monitors/](https://docs.datadoghq.com/getting_started/monitors/)
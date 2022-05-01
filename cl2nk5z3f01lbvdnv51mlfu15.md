## How to avoid AWS unintentional charges ?


Hello Devs,

Hope everyone is doing well!. 

This blog will be similar to my other blogs where I came across one problem and I struggled to solve then finally able to solve. As the title suggests in this blog I am going to talk about how to avoid AWS charges which we do sometimes unintentionally like without checking in details settings and we just do click-click and set up a particular service. 

Actually a few days back I was helping one of my friends to set up his AWS cloud infrastructure for a website. I was setting up EC2+ ELB + Route53 + RDS in AWS. As EC2 was basic one selected and ELB was selected as classic load balancer ( *$0.025 per Classic Load Balancer-hour (or partial hour) | $0.008 per GB of data processed by a Classic Load Balancer*). But RDS was not selected as a basic requirement to handle big data import processing. 

### What did I did while creating RDS ?

1. Selected AWS console -> RDS ( Asia-Mumbai region ) 
2. Standard create -> MySQL 
3. Template I choose as Production (*Use defaults for high availability and fast, consistent performance.*)  
![Screenshot 2022-05-01 at 10.04.32 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651422914658/62UhCbSW-.png align="left")
4. Standard instance name, username and password setting 
5. Selected "db.t2.medium" ( 2 vCPUs 4GB RAM No EBS optimised ) 
6. Selected "Provisioned IOPS SSD ( io1) " ![Screenshot 2022-05-01 at 10.12.32 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651423471844/W6DRaujhp.png align="left")
7. Kept 1000 as value to provisioned IOPS. 
8. Also kept checked "Enable storage auto scaling" option 
9. Created Multi AZ instance 
10. Others rest of setting normal as it like Security group, VPC, Authentication.

> Note : Provisioned IOPS SSD ( io1 and io2 ) volumes are designed to meet the needs of I/O-intensive workloads, particularly database workloads, that are sensitive to storage performance and consistency

### What mistake made in above steps :
Step number 6, 7, 8 and 9 was a mistake which I ignored and forgot to understand the pricing model. Step 6 is the bigger culprit for causing unintentional charges. Actually the website was not running that big scaling level to use those params. 

See my billing for reference 👇🏻 
![Screenshot 2022-05-01 at 10.07.59 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651423090385/q8gvtyNtt.png align="left")

### What help to reduce pricing : 

- Selecting "General Purpose SSD (gp2) 
- Unchecked "Multi AZ '' environment. This step is recommended when going to actual production and big scale level. Not the initial stage. 
- Disabled "Enabled storage auto scaling" option. 
This helps me to reduce pricing from $300 to $78-90 range. 

### What action I will recommend to avoid unintentional charges ? 
- Above RDS example is one of the services which charge a higher one. But make sure while creating other services give more importance to all default option before clicking on final create button. 
- Choose option or scaling or instance type which you required based on environment and scaling expectation. Why to pay extra if scaling is not accepted on production environment. 
- Last action is important. One is to use the "[AWS Pricing Calculator](https://calculator.aws/)" tool to evaluate a better pricing model.  Use this tool before creating any AWS service. I made a big mistake ignoring this. 

### What is the AWS Pricing Calculator ? 
AWS Pricing Calculator is a tool which provides you the option to select service and configure options based on requirement like instance type, SSD, IOPS, bandwidth, request count. Mark my word it gives exact similar pricing which you will be billed on usage. Experiencing same thing now. Choose exact params and calculate pricing. 

It is free of cost to use. It gives you the option to export as PDF or Excel. 


### How does it works ? 

![Screenshot 2022-05-01 at 9.43.25 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651421615632/5-CGVEsOx.png align="left")

### Benefits : 
- Transparent pricing
- Share your estimates
- Hierarchical estimates
- Estimate exports

### Best use cases : 
- Reduce spend on EC2 
- Reduce spend on RDS usage 
- Estimate your AWS usage for approval 

![Screenshot 2022-05-01 at 10.41.28 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651425211354/Q6NXFAlwG.png align="left")
![Screenshot 2022-05-01 at 10.43.19 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651425218511/lW2RE94e3.png align="left")


This is about my experience on how I landed to pay higher charges and avoid unintentional charges for later use. 

Thanks to the [AWS Community Builder](https://aws.amazon.com/developer/community/community-builders/) group who help and guide me here to solve this issue. Proud to be part of this group. 

Hope it will solve your issue if you come across a similar problem. Feel free to comment if you have any issues. If you like my blog please don't forget to like the article. It will encourage me to write more helpful articles. You can reach out to me over my twitter handle @aviboy2006

### References : 
- https://docs.aws.amazon.com/pricing-calculator/latest/userguide/what-is-pricing-calculator.html

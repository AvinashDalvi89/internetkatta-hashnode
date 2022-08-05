## What is the importance of performance insights in AWS RDS?

Hello Devs, 

In this blog we are going to learn about how to do analysis of AWS RDS performance and debugging. So, there was one incident that happened when an AWS RDS instance was utilising 100% CPU usage even after having autoscaling enabled and read-write replica in place. Somehow not able to find out what was causing it exactly. Then we connect to AWS support got to know one of best feature of AWS RDS monitoring which is **Performance Insights**. I am not an expert of this but whatever I know trying to put here so others can get benefits of this feature.

Let's understand what AWS RDS is and how Performance Insights can help to debug database issues. 

### What is AWS RDS ? 
- RDS stands for Relational Database System offers by AWS
- It is database managed service for SQL query as a query language
- RDS provides MySQL, Postgres, MariaDB, Oracle, Microsoft SQL server, Aurora (AWS properties service).
More details about AWS RDS can be found at the YouTube session at bottom of this blog. 

### Lets understand now what is Performance Insights
![aws-performance-insights-view.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659703014108/TntA7h8Ms.png align="left")
As name suggest **Performance Insights** it is directly related to CPU utilisation and database load. Even though CPU utilisation is related to each other, they are independent of each other. Databases can be on high load and CPU utilisation was low at that time. Similarly, Database can be on low load but CPU utilisation was high. This is actually designed for developers or users who are not having much expertise as DBA to analyse DB queries. AWS CloudWatch is majorly used for metrics to watch out for. This was not enough to analyse in details.  ![Screenshot 2022-08-05 at 6.22.09 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659703939930/9VMWTF2H3.png align="left")
So, here AWS introduced **Performance Insights Dashboard**

**Performance Insights Dashboard** collects metric data from the database engine to monitor the actual load on a database. Majorly analyse on AAS ( Average active session ) and CPU usage. It is a graph filtered by the following attributes.  : ![Screenshot 2022-08-05 at 6.24.50 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659704111565/g3YSBvrOa.png align="left")

![Screenshot 2022-08-05 at 6.27.05 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659704244649/wN0aorPoZ.png align="left")
With help of this attribute can see more insights to drill down analysis. Below image can see there are different colour codes indicating different areas to look for analysis. 
![Screenshot 2022-08-05 at 6.26.47 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659706711832/j37sKJ7a9.png align="left")
How this can be differentiated. I learned one good example from another source. Like there were multiple people in one room. There different type of people : 
- People who don't talk just come enjoy party and go away - Best performing query 
- People who just say hi to everyone and enjoy party and go away - Average performing query
- People who just talk more and discuss make more noise - like heavy and long running queries impact CPU usage or database load. 
In this third type of people making more noise means utilising more space in a room similar to a long running query.  This is one insight to look for the root cause. 

### Usage 
- It helps you to monitor multiple database performance metrics without having to analyse numerous complex graphs. All the metrics are incorporated into insightful dashboard
- It doesn't require configuration or maintenance. You simply enable it on your RDS instance, and access it.
- You can monitor the production database to enhance scaling of AWS RDS. 
- It helps to optimise query also if any query takes longer time. 

### Pricing 
By default Performance Insights offers free tier 7 days of performance data history and one million API requests per month. In RDS console retention periods can upto 24 months. 
![Screenshot 2022-08-05 at 7.31.29 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659708099302/cNJwQ3xhP.png align="left")

Note : To turn this feature on or off, you don't need to reboot the DB instance.

This is an idea about the Performance Insights Dashboard. More information is available below the YouTube session and references since I am also learning.



<iframe width="560" height="315" src="https://www.youtube.com/embed/yOeWcPBT458" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/8IdqshlAl9Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I hope this blog helps you to learn. Feel free to reach out to me on my twitter handle @aviboy2006 or comment in the blog. If you like my blog please don't forget to like the article. It will encourage me to write more helpful articles.



### References : 
- https://aws.amazon.com/rds/performance-insights/pricing/
- https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PerfInsights.html
- https://aws.amazon.com/devops-guru/features/devops-guru-for-rds/

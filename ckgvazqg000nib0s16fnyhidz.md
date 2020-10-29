## Want to know magic of AWS CloudWatch ?

**Are you guys using AWS services** ? 


- If not then try it once AWS services, it has amazing things which bind each other by ARN. 
What is AWS CloudWatch ? 
- If yes then have you seen magic behind CloudWatch ? Have you heard about CloudWatch ?

**What is CloudWatch** ? 

As the name says It's to keep watch on each and every resource of AWS and keep listening and dumping in the form of log data. And it's available to read or analyze in terms of metrics. This data can be used as an alert mechanism for creating failure or latency mechanisms.

**Sample look of CloudWatch log trails** : 

![CloudWatch log trails](https://cdn.hashnode.com/res/hashnode/image/upload/v1603830364835/i3XC7BXvd.png)

**What magic CloudWatch has**(Features): ? 

- Query Your Log Data  
- Monitor Logs from Amazon EC2 Instances  
- Monitor AWS CloudTrail Logged Events  
- Log Retention  
- Archive Log Data  
- Log Route 53 DNS Queries  
- Event Rule ( Scheduler or Cron Job )

**How to enabled** ? 

If you are unable to get CloudWatch log in, the trial check service has the right permission to execute the role to dump logs into CloudWatch or not. Some services will have by default log enable. 
Example : For dumping Lambda log, execution role required to have permission to CloudWatch.  

`Go to Lambda -> Permission -> Select Role -> Edit Role -> Edit policy -> Add CloudWatch access policy`
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "logs:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}

```

**How to filter **? 

1.  Go to CloudWatch -> CloudWatch Logs -> Log groups
2. Select or type service which would like to see log 
3. Example want to see API gateway log then search string will be "/aws/apigateway"

**How to search string or specific criteria** ?

CloudWatch can write custom queries. Go to Insight -> Select Log group -> write query. Select the field which you would like to show on insight and arrange in order by field.  [Query syntax is here.](https://docs.amazonaws.cn/en_us/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html) 
```
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```
Available fields - 

![Screenshot 2020-10-30 at 1.57.39 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604003286624/iUSnpQPhD.png)

Sample Query can find inside insight right panel -

![Screenshot 2020-10-30 at 1.57.32 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604003307558/dd1PWYc1x.png)
 
Example of execution log of Lambda - 

![Query in log](https://cdn.hashnode.com/res/hashnode/image/upload/v1604003439232/VB3wfAcco.png)

**Use as cronjob or scheduler** :

For setting schedule follow below path. 

Go to Event -> Create Rule -> Either select Event pattern or Schedule -> Select Service 

When set schedule then can add cron job expression or use predefined timing. The target can set input for lambda or other resources. 

![Event Rule](https://cdn.hashnode.com/res/hashnode/image/upload/v1604003678299/tIECeZ3c5.png)

**Note** : This article is just a high level overview to know about CloudWatch. 

Thanks for reading. If any feedback feel free to reach out to me. 

**References** : 
-  [What is CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) 
- [Enable log for API gateway ](https://aws.amazon.com/premiumsupport/knowledge-center/api-gateway-cloudwatch-logs/) 
-  [Sample Query](https://docs.amazonaws.cn/en_us/AmazonCloudWatch/latest/logs/CWL_AnalyzeLogData_RunSampleQuery.html) 
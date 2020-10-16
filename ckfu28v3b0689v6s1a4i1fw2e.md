## Host Angular 2 or 4 or 5 version on AWS S3 using CloudFront

Here we are going to see how to host Angular 2 or greater version on AWS S3 using CloudFront.

Let's first understand what is AWS S3, CloudFront, ACM and Route53. 

-  [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html)  is cloud storage which used for storing content or media or **static web hosting** 
-   [CloudFront](https://aws.amazon.com/cloudfront)  is like another CDN provided by AWS which is used as endpoint to publish server or S3 content. Its faster content delivery network. It has many endpoint like elastic load balancer or S3 bucket etc. 
-  [ACM](https://aws.amazon.com/certificate-manager)  is AWS SSL certificate provider (Public SSL/TLS certificates provisioned through AWS Certificate Manager are free. You pay only for the AWS resources you create to run your application.)
-  [Route53](https://aws.amazon.com/route53/)  is network routing provider where can create NS record or A record or CNAME  for domain routing. Which gives list of endpoint to select either direct S3 Bucket or CloudFront.

Once got glimpse of terminology let's dive into actual story. 

**How overall hosting flow works below** 👇🏻


![Hosting Flow Diagram](https://cdn.hashnode.com/res/hashnode/image/upload/v1601696174266/4cwTAFqDs.png)
                                                      

**Lets start step by step to create as it shown above architecture** : 

1. Create S3 Bucket and set up required configuration. Keep bucket name same as domain name. Like you have domain `www.example.com` then keep `www.example.com` . Here is details steps to set up S3 bucket as website hosting : https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html ![S3 Bucket Creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1601696610600/uTGoJdFCX.png)                                

2. Create a Angular build using command `ng build`
3. Copy paste `dist` folder source and paste into S3 bucket
4. Set up S3 website hosting index and error page to index.html. In Angular all page route request should goes to `index.html` for that reason we have set both to index. ![Setting up root file](https://cdn.hashnode.com/res/hashnode/image/upload/v1601696889823/JsUJIPPPg.png)
5. Set up bucket policy under Permission-> Block Public Access and Permission -> Bucket Policy. 
Bucket Policy use this same as below:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::example.com/*"
            ]
        }
    ]
}
``` 
![Block Public Access Configuration](https://cdn.hashnode.com/res/hashnode/image/upload/v1602126494963/y84ykju-j.png)

6. Create CloudFront endpoint and follow this steps : https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-https-requests-s3/
7. Set up Route53 entry for domain. ![Route53 Entry Creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1601750718290/IZLZh8-Zj.png)

Now everything is completed ? 

Wait... 

One important things is left. Angular has routing and URL rewriting concept inside. Means one page does all routing. `index.html` is root file which is responsible for all routing. This above setup will works when your continuously used application without refreshing. If hit refresh then it will give "Access Denied" from S3 bucket. 

Answer is S3 doesn't understand route open when you reload and open in new tab. Have  to inform S3 is for this route used index.html. Whenever new route open its gives 403 [access denied ] error. for this you need to do setting CloudFront to set 403 error page redirect to index.html.

Open CloudFront -> Select distribution -> Error page -> Create Custom Error Response

![Error Pages](https://i.stack.imgur.com/EhteQ.png)

Now we are finally done 😁👍🏻

You can reach out to me over [twitter](https://twitter.com/aviboy2006) for any clarification or stuck anywhere.  You can refer automated deployment script written in Python for Angular build deployment. 

**Reference** : 


- [Receive AccessDenied when trying to access a reload or refresh or one in new tab in angular 5](https://stackoverflow.com/questions/50299204/receive-accessdenied-when-trying-to-access-a-reload-or-refresh-or-one-in-new-tab) 
- [Automation of Angular Build to AWS S3 + CloudFront](https://github.com/aviboy2006/angular-build-upload-s3-cloudfront)

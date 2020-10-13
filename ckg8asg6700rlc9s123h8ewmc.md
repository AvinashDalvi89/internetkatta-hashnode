## Rewrite non www to www in AWS S3 bucket and CloudFront

Hello Everyone, 

This article cover how to redirect non-www domain to www while using S3 bucket static hosting and CloudFront. As everyone might know about how to redirect/rewrite non-www to www using `.htaccess` in apache server. 

```
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteRule ^(.*)$ http://www.%{HTTP_HOST}/$1 [R=301,L]
```
Like above apache rule but differently have to setup rules in S3 bucket and CloudFront.

**What is expectation** ? 

Redirect all request which comes to `http://example.com/` requests to `http://www.example.com`

**Answer** : 

1. Create `www.example.com` S3 bucket and set all code in this bucket![Bucket creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1602598965324/t6VOPWTz1.png)
2. Create `example.com` S3 bucket and set redirect to www.example.com said by  ![Second bucket creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1602599118139/btwLvSnjw.png)![Redirection setting](https://cdn.hashnode.com/res/hashnode/image/upload/v1602599106146/upzhIYrQA.png)
3. Create CloudFront and configure with s3 bucket link of www.example.com and add CNAME entry only for www.example.com. In route 53 for www.example.com point alias as CloudFront link related to s3 bucket ![S3 origin configuration](https://cdn.hashnode.com/res/hashnode/image/upload/v1602599344922/WvGbN5o--.png)![CNAME setting in CloudFront](https://cdn.hashnode.com/res/hashnode/image/upload/v1602599354653/qPxYgWH9K.png)
4. Do same setting as per point 3 for example.com point alias s3 bucket of example.com
5. Create Route53 entry for both domain `www.example.com` and `example.com` A record and endpoint should be respective CloudFront endpoint. ![Route53 Configuration](https://cdn.hashnode.com/res/hashnode/image/upload/v1602613594781/rg1D_71pf.png)

Then we are done 🥳 🥳 🚀🚀🚀

**Reference** : 
- [How to redirect URL in S3](https://aws.amazon.com/blogs/aws/root-domain-website-hosting-for-amazon-s3/) 
- https://stackoverflow.com/questions/37000416/how-to-redirect-non-www-to-www-in-aws-s3-bucket-and-cloudfront/37026855#37026855
- https://www.digitalocean.com/community/tutorials/how-to-redirect-www-to-non-www-with-apache-on-ubuntu-14-0
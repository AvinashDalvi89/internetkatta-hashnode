## Host WordPress site on AWS S3 bucket

Are you using WordPress blog or site ? Would like to host on S3 and serve content faster ? 

Follow these steps to host WordPress site on AWS S3 :

1.  Create one subdomain or test domain for only publishing or creating post/pages from wp-admin
2.  Host all WordPress setup on that domain
3.  Create S3 bucket where we are going put content
4.  Create CloudFront which is point to S3 bucket origin
5.  Point main domain to CloudFront
6.  Install this plugin [wpcloudhosting](https://wordpress.org/plugins/wpcloudhosting/) for taking snapshot of content to S3 or you can  [Simple Static plugin](https://www.simplystatic.co/)  which does help in converting WordPress to Statics HTML files.
7.  Set up all configuration in wp-admin and publish all content using this plugin.


![Hosting Flow .png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601576707165/H3hceXP95.png)
Any query feel free to comment.
## Host WordPress site on AWS S3 bucket

<span class="s"></span>

# Host wordpress site on aws s3 bucket


<noscript><img alt="Image for post" class="t u v fu aj" src="https://miro.medium.com/max/2400/0*_rpOtP7xsv-XHBky.png" width="1200" height="600" srcSet="https://miro.medium.com/max/552/0*_rpOtP7xsv-XHBky.png 276w, https://miro.medium.com/max/1104/0*_rpOtP7xsv-XHBky.png 552w, https://miro.medium.com/max/1280/0*_rpOtP7xsv-XHBky.png 640w, https://miro.medium.com/max/1400/0*_rpOtP7xsv-XHBky.png 700w" sizes="700px"/></noscript>

Follow these step to host wordpress site on aws S3 :

1.  create one subdomain or test domain for only publishing or creating post/pages from wp-admin
2.  host all wordpress setup on that domain
3.  create s3 bucket where we are going put content
4.  create cloudfront which is point to s3 bucket origin
5.  point main domain to cloudfront
6.  install this plugin [https://wordpress.org/plugins/wpcloudhosting/](https://wordpress.org/plugins/wpcloudhosting/) for taking snapshot of content to s3
7.  set up all configuration in wpadmin and publish all content using this plugin.


![Hosting Flow .png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601576707165/H3hceXP95.png)
Any query feel free to comment.
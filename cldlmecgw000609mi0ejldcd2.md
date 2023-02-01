# Move files from SSH server to S3 bucket

Hello, devs

This blog going to explain the steps to move files or folders from the SSH server ( **where AWS credentials are not configured** ) to the AWS S3 bucket easily without missing any files or folders.

I was working on moving away from a legacy server to S3 bucket hosting for static file hosting. Being legacy there was many files and folder was there and the legacy server was not having AWS access enabled. Due to the limitation of storage cannot run the zip command to compress and download files. Later those files and folders have to move to the S3 bucket.

While googling found good solutions to tackle this problem.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675250168362/cba008e3-e9e4-438b-96c7-cfdc24e8bbc1.gif align="center")

### Let's follow below steps:

Considering you have access to the SSH server and S3 bucket AWS credentials.

1. Need to use `resync` command. There is `SCP` the command is also available but if the internet interrupts you have to start again. And FileZilla took more time than the terminal command. `sshdirectorypath` will be SSH server directory from where you have to get files and folders and it will do all recursively. Same `/localmachine/directory/` your local machine directory.
    

```bash
> rsync -avz -e ssh username@hostname:/sshdirectorypath/ /localmachine/directory/
receiving file list ... done
```

1. If the SSH server is already enabled AWS credentials like secrete key and access key under `~/.aws/credential` then can skip the first step.
    
2. Running command to copy the local backup to the S3 bucket.
    

```bash
s3 cp ./ s3://bucketname/ --recursive
or if you have to select specific folder inside bucket then 
s3 cp ./ s3://bucketname/folder/folder --recursive
```

### Benefits of using `resync` command:

* is no data leakage will happen
    
* even if the internet interruption will resume where it last stop
    
* faster and it lists the first contents and copies.
    

I hope this blog helps you to learn. Feel free to reach out to me on my Twitter handle @[@AvinashDalvi_](@AvinashDalvi_) or comment on the blog.

Keep learning and keep sharing!

### References :

* [https://www.geeksforgeeks.org/rsync-command-in-linux-with-examples/](https://www.geeksforgeeks.org/rsync-command-in-linux-with-examples/)
    
* [https://serverfault.com/a/350557](https://serverfault.com/a/350557)
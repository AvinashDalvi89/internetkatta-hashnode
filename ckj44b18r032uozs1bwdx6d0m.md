## How to create tar file compression in AWS S3 ?

I am going to explain about how to create tar file compression in AWS S3 bucket files using Python(Boto3). This solution I came across while solving one  [StackOverflow question](https://stackoverflow.com/questions/64341192/how-to-create-a-tar-file-containing-all-the-files-in-a-directory/64341789#64341789) .  Let me explain first what a tar file. 

**What is Tar file ? **

In computing, tar is a computer software utility for collecting many files into one archive file, often referred to as a tarball, for distribution or backup purposes. It's almost similar like ZIP compression but the level of compression is different for tar files. The major difference between the two formats is that in ZIP, compression is built-in and works independently for every file in the archive, whereas tar compression is an extra step that compresses the entire archive.

Now we have an idea about tar files, so let's see how we can compress AWS S3 bucket files and make tar files. This I am going to explain in Python  [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html)  library. 

**Perquisite** : 

- If you are running locally this code make sure you install `pip` to install required Python dependancies
- `pip install boto3` or `pip3 install boto3` 
- `pip install tarfile` 
- Make sure you have the right AWS credential set in locally. See this for  [setting up AWS credential](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html) 

Once these dependencies are cleared we are all set for writing code. Follow below steps:

1. Import required library. 
```
import boto3
import tarfile
import os.path
s3Client = boto3.client('s3')
s3object = boto3.resource('s3')
```
2.  Write a function to find matching S3 keys input for this function will be bucket name, prefix and suffix of file.
```
def get_matching_s3_keys(bucket, prefix='', suffix=''):
    """
    Generate the keys in an S3 bucket.
    :param bucket: Name of the S3 bucket.
    :param prefix: Only fetch keys that start with this prefix (optional).
    :param suffix: Only fetch keys that end with this suffix (optional).
    """
    kwargs = {'Bucket': bucket, 'Prefix': prefix}
    while True:
        resp = s3Client.list_objects_v2(**kwargs)
        for obj in resp['Contents']:
            key = obj['Key']
            if key.endswith(suffix):
                yield key

        try:
            kwargs['ContinuationToken'] = resp['NextContinuationToken']
        except KeyError:
            break
```
3. Then you call the above `get_matching_s3_keys()` function in parent or init method. In this line of code we are going to use the `tarfile` package `open()` method to initialise tar files. This is almost similar functionality like file operation which includes `open()`, `write()` or `add()` and `close()`.  Can see below 👇🏻 In this code I am compressing only `js` file you can specify which you would like to do. 

```
    agtBucket = "angularbuildbucket"
    key=""
    tar = tarfile.open('/tmp/example.tar', 'w')
    source_dir="/tmp/"
    for fname in get_matching_s3_keys(bucket=agtBucket, prefix=key, suffix='.js'):
        print(fname)
        file_obj = s3object.Object(agtBucket, fname)
        s3object.Bucket(agtBucket).download_file(fname, '/tmp/'+fname)
        tar.add(source_dir, arcname=os.path.basename(source_dir))
    tar.close()
    s3object.meta.client.upload_file(source_dir+"example.tar", agtBucket, 'example.tar')
```

Then we are done with writing code. If you would like to use AWS Lambda with same code then here is gist link : [tar-creation-s3-bucket.py](https://gist.github.com/aviboy2006/31fc1fc450edb2c4af19d47f36ab1259)


Hope you like my blog. Thanks for reading my blog. If you have any questions you can reach out to me at my twitter handle - [@aviboy2006](https://twitter.com/aviboy2006)

**References** : 

- https://www.brendanlong.com/zip-vs-tar-for-compressed-archives.html
- https://en.wikipedia.org/wiki/Tar_(computing)
- https://stackoverflow.com/questions/64341192/how-to-create-a-tar-file-containing-all-the-files-in-a-directory/64341789#64341789
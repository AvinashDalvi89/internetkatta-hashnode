## Search file or folder in nested subdirectory of S3 bucket

**Why**? 

Few days back i came to one StackOverflow question where user wanted to search file name in nested directory of S3 bucket in python. I thought of writing details about this problem and solution. This was originally published on dev.to i am sharing it again on blog.

**What is exactly **? 
I am explaining about searching file in nested subdirectory is exist in S3 bucket or not. 

**How to solve ** ? 

Created AWS lambda code in Python using `boto3` to find existence of sub directory. This code can used in basic Python also not necessary to user Lambda code but this is quickest way to run code and test it.  

**Which are perquisites ** ? 

- Python3 - You can use python2.x also you need to modify print function calls
- `pip` - installation of other dependant pythonlibrary
- `boto3` - `pip3 install boto3` or `pip install boto3`
- `aws-cli` - to configure aws creditional basesd on environment specific
 

**How it does** ? 

1. List bucket objects 
```
client.list_objects(Bucket=_BUCKET_NAME, Prefix=_PREFIX)
```
Above function gives list of all content exist in bucket along with path. It will be easy to trace it out.

2. Then iterate through list of folder and files to find exact object or file. 

**Here is example of code **:
```
import boto3

client = boto3.client('s3')
bucket_name = "bucket_name"
prefix = ""

s3 = boto3.client("s3")

result = client.list_objects(Bucket=bucket_name, Delimiter='/')
   for obj in result.get('CommonPrefixes'):  
       prefix = obj.get('Prefix')
       file_list = list_files(client, bucket_name, prefix)
       for file in file_list:
          if "processed/files" in file:
              print("Found",file)

def list_files(client, bucket_name, prefix):
    _BUCKET_NAME = bucket_name
    _PREFIX = prefix
    """List files in specific S3 URL"""
    response = client.list_objects(Bucket=_BUCKET_NAME, Prefix=_PREFIX)

    for content in response.get('Contents', []):
        #print(content)
        yield content.get('Key')
```


Reference : 
- [https://gist.github.com/aviboy2006/bf3feb8828d2fb311ffe22b750b2b297](https://gist.github.com/aviboy2006/bf3feb8828d2fb311ffe22b750b2b297)
- [https://stackoverflow.com/questions/62158664/search-in-each-of-the-s3-bucket-and-see-if-the-given-folder-exists/62160218#62160218](https://stackoverflow.com/questions/62158664/search-in-each-of-the-s3-bucket-and-see-if-the-given-folder-exists/62160218#62160218)
 
Thanks for reading article. Share your valuable feedback and suggestions!

_Originally published at_ [_https://dev.to_](https://dev.to/aviboy2006/search-nested-subdirectory-in-s3-bucket-52ke) _on June 19, 2020._
## Extract text or data using AWS Textract

Hello Devs,

In this article, we will learn how to extract text from images or PDF files using the AWS Textract tool. Let me tell you first what Textract is and how it works ?

**What is AWS Textract** ?:
 
![Arch_AWS-Textract_64.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609243612474/VvvFSmOBL.png)
Textract word is combination of two words one is Text and another is extract. This tool is used to extract raw form of text from an image and PDF file. AWS has built their machine learning mechanism behind this to get text or values.  This extract printing text, handwritten text that goes beyond OCR ( Optical Character Recognition). This doesn't give only raw text, this categories data if its table or form format and shows it separately. 

**What is benefits **: 

- Extract structured & unstructured data quickly and accurately
- Go beyond simple Optical Character Recognition (OCR)
- Easily implement human reviews
- Security & Compliance

**What is use cases** : 

- Bank statement parsing 
- Insurance form scanning to avoid manual data entry work
- Cheque book entry scanning ( Axis Bank has their own cheque entry automated process) 
- In all educational or bank institute data entry related work
- tax filing 
- Many more cases are there. 

**Let's see how its works** : 

- You should have an AWS console account to access AWS CodeGuru.
- Go to Search Console -> Open Machine Learning -> Textract
![Screenshot 2020-12-29 at 5.49.42 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609244417692/DXDm5dIvg.png)
- Click Upload document ( if you have PDF file you have to upload to S3 bucket and name will be **textract-console-us-east-1**). Image can upload directly. 
- Once its process it will show data in three tab Raw text, Form and Tables. 

Processed data is available in three different formats when you download a zip file. It is in CSV, text and JSON ( key value pair ).

**Note**:  There is might be some data can't extract correctly like the way sometime human wrote. For this AWS has given human review its last option before its process or finalised. 

**Here is demo video** : 

<iframe width="560" height="315" src="https://www.youtube.com/embed/0J0BNWw6Xjo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This can be done using Python boto3 library. Here is reference link : https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/textract.html I will explain in my next blog how to use textract using boto3. 

Hope you like my blog. Thanks for reading my blog. If you have any questions you can reach out to me at my twitter handle - @aviboy2006

**References** :
 
- https://aws.amazon.com/textract/

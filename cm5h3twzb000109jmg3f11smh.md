---
title: "Simplified Data Masking in AWS Lambda with Powertool"
seoTitle: "AWS Lambda Data Masking Made Easy"
seoDescription: "Learn how to implement data masking with Powertools in AWS Lambda to ensure compliance in healthcare and finance industries"
datePublished: Fri Jan 03 2025 18:44:41 GMT+0000 (Coordinated Universal Time)
cuid: cm5h3twzb000109jmg3f11smh
slug: simplified-data-masking-in-aws-lambda-with-powertool
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1735929105657/fc524c73-d47a-4ef7-9c99-7013bc31a457.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1735929299229/db3317ee-b6ed-4705-af96-2a27a5e5ca44.png
tags: lambda, aws, python, serverless, lambda-function, powertools

---

Hello Devs,

> “Data is the new oil,” they say, but in healthcare and finance, it’s more like nitroglycerin—immensely valuable, yet dangerously explosive if mishandled.”

I recently shared on social media that I've joined the healthcare industry. This marks a shift from my background in both e-commerce and finance. During my time in finance, I dealt extensively with highly sensitive data like bank statements, KYC information, personal identification details, and other financial records. I vividly recall understanding that even a minor data handling error could have severe repercussions: breaches, fines, and, most importantly, a loss of public trust. This same level of data sensitivity exists in healthcare, where every piece of patient information is crucial and protected by regulations like GDPR and HIPAA. To comply with these regulations and similar ones worldwide, data masking has become essential. In this blog, I’ll break down what Powertools is, how it can be used for data masking in AWS Lambda, and why it’s critical for domains like finance and healthcare. While Powertools for AWS Lambda existed for .NET and TypeScript, the May 2024 release of Python support has been a major boost, making it accessible to a much wider audience. Let's dive into Powertools for AWS Lambda.

## **What is Powertools for AWS Lambda?**

Think of **Powertools for AWS Lambda** as a Swiss Army knife for serverless applications. It’s an open-source library that helps you write better, more secure, and more maintainable code. Instead of reinventing the wheel every time you need to mask data, log securely, or handle retries, Powertools provides ready-to-use utilities.

#### **Key Features of Powertools for AWS Lambda**

Powertools offers a robust set of features to simplify serverless development:

* **Tracer**
    
* **Logger**
    
* **Metrics**
    
* **Event Handler**
    
* **Parameters**
    
* **Batch Processing**
    
* **Typing**
    
* **Validation**
    
* **Event Source Data Classes**
    
* **Parser (Pydantic)**
    
* **Idempotency**
    
* **Data Masking** *(Focus of this blog)*
    
* **Feature Flags**
    
* **Streaming**
    
* **Middleware Factory**
    
* **JMESPath Functions**
    
* **CloudFormation Custom Resources**
    

In this blog, we are going to explore **Data Masking** usage in detail. For data masking, Powertools ensures that only necessary data appears in your logs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735881923883/882e6bc3-54d6-45e9-a11d-2c92eeab62e0.png align="center")

*Image taken from official website.*

## **Why is Data Masking Important in AWS Lambda?**

When dealing with serverless functions, especially in industries like healthcare and finance:

* Logs are often sent to systems like CloudWatch.
    
* Sensitive information (like SSNs, credit card numbers, or medical IDs) can easily end up in plaintext logs.
    
* Compliance standards (HIPAA for healthcare, PCI-DSS for finance) strictly prohibit exposing such data.
    

This is where **Powertools for AWS Lambda** comes into play.

## **How to Use Powertools for AWS Lambda for Data Masking**

Let’s break this down step by step. I am taking example of Python here because it widely used and I used this my day to day activities.

**1\. Install Powertools**

```plaintext
pip install aws-lambda-powertools // use pip3 for Python3 
pip install jsonpath_ng // if you get issue for jsonpath_ng. This mostly require for below example
pip install ply // if you get issue for ply. This mostly require for below example
```

**2\. Use the Logger Utility for Masking ( erase, encrypt, decrypt).**

In this example we are using `erase` method to mask data. Powertools for AWS Lambda offers three primary functions for data masking:

* **erase**: Removes sensitive data fields completely.
    
* **encrypt**: Encrypts sensitive data fields.
    
* **decrypt**: Decrypts previously encrypted fields.
    

```python
from __future__ import annotations

from aws_lambda_powertools import Logger
from aws_lambda_powertools.utilities.data_masking import DataMasking
from aws_lambda_powertools.utilities.typing import LambdaContext

logger = Logger()
data_masker = DataMasking()


@logger.inject_lambda_context
def lambda_handler(event: dict, context: LambdaContext) -> dict:
    data: dict = event.get("body", {})

    logger.info("Erasing fields email, address.street, and company_address")

    erased = data_masker.erase(data, fields=["email", "address.street", "company_address", "aadhar", "diagnosis","blod_group"])  

    return erased
```

In this example, sensitive data like Aadhar, blood group, diagnosis is masked while still allowing logs to be useful for debugging.

```json
Response:
{
  "id": 1,
  "name": "Avinash Dalvi",
  "age": 30,
  "email": "*****",
  "address": {
    "street": "*****",
    "city": "Bengaluru",
    "state": "KA",
    "zip": "211311"
  },
  "diagnosis": "*****",
  "blod_group": "*****",
  "aadhar": "*****",
  "company_address": "*****"
}
```

To use `encrypt` method refer this official documentation [https://docs.powertools.aws.dev/lambda/python/latest/utilities/data\_masking/#encrypting-data](https://docs.powertools.aws.dev/lambda/python/latest/utilities/data_masking/#encrypting-data) and for `decrypt` method use this [https://docs.powertools.aws.dev/lambda/python/latest/utilities/data\_masking/#encrypting-data](https://docs.powertools.aws.dev/lambda/python/latest/utilities/data_masking/#encrypting-data)

In this way we can validate compliance with Powertools and it is not only masks data but also helps with metrics and tracing to ensure compliance with industry standards like GDPR or HIPPA

## **Real-World Use Case: Healthcare Application Logs**

Imagine a healthcare Lambda function that processes insurance claims. Without masking, logs might show:

```python
INFO: Processing claim for Patient: Avinash Dalvi, Aadhar ID : 1233-2432-2233, Blood Group : O+
```

With Powertools:

```python
INFO: Processing claim for Patient: Avinash Dalvi, Aadhar ID : *****, Blood Group : *****
```

A tiny change, but one that can save millions in potential fines and, more importantly, protect lives.

## **How It Impacts Healthcare and Finance**

* **Healthcare:** Ensures compliance with HIPAA by preventing PHI (Protected Health Information) leaks.
    
* **Finance:** Aligns with PCI-DSS guidelines to prevent exposure of payment details.
    
* **Audit Readiness:** Masked logs are audit-friendly while maintaining transparency for debugging.
    

In fast growing world of Serverless architectures, Powertools for AWS Lambda isn’t just tool - it’s a shield which protect us and make it compliant. Whether you are handling medical records, financial transactions or personal user data integrating Powertools ensures you are not just compliant but also responsible.

As I continue building solutions in the healthcare space, Powertools has become my go-to companion, making critical use cases easier, safer, and scalable.

*Have you faced similar challenges in your Serverless journey? Share your thoughts below, and let’s keep building secure and responsible applications together.*

I hope this blog helps you to learn. Feel free to reach out to me on my Twitter handle @AvinashDalvi\_ or leave a comment on the blog. Stay tuned for more learning for data masking using Powertools.

## References:

* [https://docs.powertools.aws.dev/lambda/python/latest/utilities/data\_masking/](https://docs.powertools.aws.dev/lambda/python/latest/utilities/data_masking/)
    
* [https://github.com/aws-powertools/powertools-lambda-python](https://github.com/aws-powertools/powertools-lambda-python)
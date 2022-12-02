# Overview of AWS Application Composer

Hello Devs,

I am going to give an overview of AWS Application Composer which was got announced in AWS re: Invent 2022. They are a couple of services launched at this event. You might think I am writing about this service only. Let me tell you a story.

%[https://twitter.com/Werner/status/1598494852173692928?s=20&t=j1IuLmEz_bEszTdiM7iAdw] 

I am one of them who is waiting for this kind of service for a long time. I have been interviewing with the AWS product team for giving this feedback when I shared what challenges faced while creating interlink of service using CloudFormation or AWS Console UI. How is this difficult for people who haven't much aware of this CloudFormation process etc? Why can't this work like drag and drop service the way plug and play model works? There should be something that can work which can enable us to visualise architecture and make it work.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669963618079/Mn4YUz7da.gif align="left")

And the wait is over...

Here is AWS's announced service called

### AWS Application composer

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669961129475/SYK2FApFD.gif align="left")

The picture is used from AWS Official site.

### What is AWS Application Composer?

AWS Application Composer helps us to create and accelerate the architecture, configuration and building of serverless applications using just a simple drag-and-drop workflow. It also empowers us to deploy the necessary configuration to our application.

### What is the way to do this?

*   Can use the demo project to start
    
*   Can use a blank template to use
    
*   if you have any CloudFormation ready can import and see a visualisation
    

### Let's check out how this works

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669963143396/RfyOgtXzb.png align="left")

### Lets start :

1.  Open AWS Console -&gt; AWS Application Composer -&gt; Click on "New blank project"
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669963796569/Uetu6NgyN.png align="left")

2.  Will use a simple use case to create an API gateway which is to get items.
    
3.  Click the left sidebar and select the relevant service which choose to create. In our case will select the API gateway
    
4.  Then add properties value to the right sidebar like API gateway Logical ID, path, Allow origin value etc.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669964250534/5sRXRhLbX.png align="left")
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669964433326/KNmMm6sE7.png align="left")

5.  This way can link two services to each other and click on the Template tab to see the CloudFormation YAML file.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669964983491/w3ClA9WY1.png align="left")
```yaml
Transform: AWS::Serverless-2016-10-31
Resources:
  Api:
    Type: AWS::Serverless::Api
    Properties:
      Name: !Sub
        - ${ResourceName} From Stack ${AWS::StackName}
        - ResourceName: Api
      StageName: Prod
      DefinitionBody:
        openapi: '3.0'
        info: {}
        paths:
          /:
            get:
              x-amazon-apigateway-integration:
                httpMethod: POST
                type: aws_proxy
                uri: !Sub arn:${AWS::Partition}:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${Function.Arn}/invocations
              responses: {}
      EndpointConfiguration: REGIONAL
      TracingEnabled: true
  Function:
    Type: AWS::Serverless::Function
    Properties:
      Description: !Sub
        - Stack ${AWS::StackName} Function ${ResourceName}
        - ResourceName: Function
      CodeUri: src/Function
      Handler: index.handler
      Runtime: nodejs18.x
      MemorySize: 3008
      Timeout: 30
      Tracing: Active
      Events:
        ApiGET:
          Type: Api
          Properties:
            Path: /
            Method: GET
            RestApiId: !Ref Api
  FunctionLogGroup:
    Type: AWS::Logs::LogGroup
    DeletionPolicy: Retain
    Properties:
      LogGroupName: !Sub /aws/lambda/${Function}
```

There are some more features available like :

*   Arrange services
    
*   Group services
    
*   Zoom in and Zoom out
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669965416070/mEcGbihfW.png align="left")

This is a very basic use case to create the Architecture of any application. I hope this blog helps you to learn. Feel free to reach out to me on my Twitter handle @[@AvinashDalvi_](@AvinashDalvi_) or comment on the blog. I will write more on this service as it this very close to my heart. Stay tuned!

### Reference :

*   [https://aws.amazon.com/blogs/compute/visualize-and-create-your-serverless-workloads-with-aws-application-composer/](https://aws.amazon.com/blogs/compute/visualize-and-create-your-serverless-workloads-with-aws-application-composer/)
    
*   [https://docs.aws.amazon.com/application-composer/latest/dg/reference-navigation.html](https://docs.aws.amazon.com/application-composer/latest/dg/reference-navigation.html)
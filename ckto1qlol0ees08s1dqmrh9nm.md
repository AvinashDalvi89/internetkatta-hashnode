## How to create SNS notification for API gateway monitoring using CloudFormation

Hello Devs,

In my previous post last year I explained about  [how to create SNS notification for Lambda monitoring](https://www.internetkatta.com/aws-lambda-monitoring-mechanism-using-sns) . Now I will extend that functionality more by adding API gateway monitoring into it along with creation of full stack using CloudFormation. 

### Before we jump into actual explanation let me explain basic concepts : 

- **What is SNS**  - SNS is an AWS notification or event management service which has two endpoints like subscribe and publish. Based on the status of those events SNS can notify another SNS or Lambda or SMS or Email ( Any medium). SNS topic and receive published messages using a supported endpoint type, such as Amazon Kinesis Data Firehose, Amazon SQS, AWS Lambda, HTTP, email, mobile push notifications, and mobile text messages (SMS). Good explanation and diagram given on AWS documentation here https://docs.aws.amazon.com/sns/latest/dg/welcome.html
- **What is Lambda** - Lambda is an AWS serverless service ( compute )  which provides us a container to run our code like Python, NodeJs, .Net, GoLang, Java etc. It's like a small piece of code runner. Lambda can trigger by many ways like API gateway, SNS, Steps Functions etc.  Lambda runs your code on a high-availability compute infrastructure
- **What is API gateway** - AWS API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs like any other API endpoint. 
- **What is CloudFormation **- AWS CloudFormation is magic which enables developers and businesses an easy way to create a stack of related AWS and external resources, and manage them in the right order. ​This works as Infrastructure as code. 
![CloudFormation_Diagram.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631763082462/3XQkO7mRe.png)

### Let's start with how to create using CloudFormation ? 

1. **First will create basic CloudFormation** - 
Just for reference AWS has provided many predefined templates based on use cases here https://aws.amazon.com/cloudformation/resources/templates/ can refer to that too. 
We will start with scratch. We have to create a template either in yaml or json format. Templates tell us about how resources are created, what is configuration of them and how they wiring each other. Whichever is mentioned in {} those are custom or user-defined names of values. 
 
 ```
AWSTemplateFormatVersion: '2010-09-09'
Parameters:
  {ParamsName}:
    Type: String
    Description: Description of params 
    Default: default value
Resources:
  {ResourceCustomName}:
    Type: AWS::ApiGateway::RestApi
    Properties:
      ApiKeySourceType: HEADER
      Description: An API Gateway with a Lambda Integration
      EndpointConfiguration:
        Types:
          - EDGE
      Name: Name of resources which are going to be used publicly. 
 ``` 

2. **Defining CloudFormation parameters** ( Optional) :  This step is optional. If you would like to pass parameters while running CloudFormation stack then you select this else ignore. I will recommend this step for better and dynamic deployment.

 ```
Parameters:
  ArtifactsBucket:
    Type: String
    Description: The S3 bucket where the artifacts are stored
    Default: 'awsugblr-cnf-demo'
  LambdaArtifactsKey:
    Type: String
    Description: The key from the S3 Bucket with the artifacts to deploy 
    Default: 'lambda-sample.zip'
  FunctionName:  
    Type: String
    Description: Name of the function
    Default: 'lambda-apigateway-sns-test'
``` 

3. **Create Lambda resources** : While creating Lambda resources need to create a role or if you have an existing role available under IAM can be used. Make sure the role should have a similar policy to access like below example. You can use  [AWS IAM policy generator tool](https://awspolicygen.s3.amazonaws.com/policygen.html) 

 ```
Policies:
      - PolicyName: root
        PolicyDocument:
          Version: '2012-10-17'
          Statement:
          - Effect: Allow
            Action:
            - sns:*
            Resource: arn:aws:sns:*:*:*
 ```
Once you are ready with an IAM role you can use Lambda resources. While creating lambda you can use zip as code base or pass runtime code like mentioned here. https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-function.html. I am here using zip as a code base. Creating code zip and uploading to the respective S3 bucket. Before you start with CloudFormation template creation make sure to create an S3 bucket to upload code base. 

  ```
  LambdaApiGatewaySNSTest:
    Type: AWS::Lambda::Function
    Properties:
      Description: To test apigateway and SNS on OnFailure
      Handler: index.handler
      Runtime: nodejs12.x
      FunctionName: !Ref FunctionName
      Code:
        S3Bucket: !Ref ArtifactsBucket
        S3Key: !Ref LambdaArtifactsKey
      Role: !GetAtt LambdaIamRole.Arn
      MemorySize: 128
      Timeout: 60
  LambdaIamRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: 'Allow'
            Principal:
              Service:
                - 'lambda.amazonaws.com'
                - 'sns.amazonaws.com'
            Action:
              - 'sts:AssumeRole'
      Policies:
      - PolicyName: root
        PolicyDocument:
          Version: '2012-10-17'
          Statement:
          - Effect: Allow
            Action:
            - sns:*
            Resource: arn:aws:sns:*:*:*
      Path: '/'
  
  ``` 

4. **Create SNS topics and subscription**:  In SNS topics you have a Lambda endpoint under `FunctionName`. ` !Ref LambdaApiGatewaySNSTest` points to the name of the Lambda function element. And can choose any medium to get notified either SMS or email. You can link Lambda to either `OnFailure` or `OnSuccess` to get notified by SNS. 

 ```
  LambdaEmailAlertSNSTopic: 
    Type: AWS::SNS::Topic 
    Properties: 
      TopicName: lambda-apigateway-sns-test-execution
      DisplayName: lambda-apigateway-sns-test-execution 
      Subscription: 
        - Endpoint: "example@gmail.com" 
          Protocol: "EMAIL-JSON"
  LambdaEventInvokeConfig:
    Type: AWS::Lambda::EventInvokeConfig
    Properties:
        FunctionName: !Ref LambdaApiGatewaySNSTest
        Qualifier: "$LATEST"
        MaximumEventAgeInSeconds: 600
        MaximumRetryAttempts: 0
        DestinationConfig: 
            OnFailure:
                Destination: !Ref LambdaEmailAlertSNSTopic
 ``` 

5. **Create API gateway** : Sample template code will be there at AWS documentation. Don't be afraid of longer code for API gateway. While creating API gateway there components like API, Resource, Method, Model( Return and response data type), API stage, Deployment and IAM role ( if not existing using. ). 

 ```
  LambdaApiGatewayRestApi:
    Type: AWS::ApiGateway::RestApi
    Properties:
      ApiKeySourceType: HEADER
      Description: An API Gateway with a Lambda Integration
      EndpointConfiguration:
        Types:
          - EDGE
      Name: lambda-api
  LambdaApiGatewayResource:
    Type: AWS::ApiGateway::Resource
    Properties:
      ParentId: !GetAtt LambdaApiGatewayRestApi.RootResourceId
      PathPart: 'lambda'
      RestApiId: !Ref LambdaApiGatewayRestApi
  LambdaApiGatewayMethod:
    Type: AWS::ApiGateway::Method
    Properties:
      ApiKeyRequired: false
      AuthorizationType: NONE
      HttpMethod: POST
      Integration:
        ConnectionType: INTERNET
        Credentials: !GetAtt LambdaApiGatewayIamRole.Arn
        IntegrationHttpMethod: POST
        PassthroughBehavior: WHEN_NO_MATCH
        TimeoutInMillis: 29000
        Type: AWS_PROXY
        Uri: !Sub 'arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${LambdaApiGatewaySNSTest.Arn}/invocations'
      OperationName: 'lambda'
      ResourceId: !Ref LambdaApiGatewayResource
      RestApiId: !Ref LambdaApiGatewayRestApi
  LambdaApiGatewayModel:
    Type: AWS::ApiGateway::Model
    Properties:
      ContentType: 'application/json'
      RestApiId: !Ref LambdaApiGatewayRestApi
      Schema: {}
  LambdaApiGatewayStage:
    Type: AWS::ApiGateway::Stage
    Properties:
      DeploymentId: !Ref LambdaApiGatewayDeployment
      Description: Lambda API Stage v0
      RestApiId: !Ref LambdaApiGatewayRestApi
      StageName: 'v0'
  LambdaApiGatewayDeployment:
    Type: AWS::ApiGateway::Deployment
    DependsOn: LambdaApiGatewayMethod
    Properties:
      Description: Lambda API Deployment
      RestApiId: !Ref LambdaApiGatewayRestApi
  LambdaApiGatewayIamRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Sid: ''
            Effect: 'Allow'
            Principal:
              Service:
                - 'apigateway.amazonaws.com'
            Action:
              - 'sts:AssumeRole'
      Path: '/'
      Policies:
        - PolicyName: LambdaAccess
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: 'Allow'
                Action: 'lambda:*'
                Resource: !GetAtt LambdaApiGatewaySNSTest.Arn
```

Once we finish all above steps you are ready with the CloudFormation template to deploy on stack.  If any error occurs while validation CloudFormation AWS console will throw an error. Most errors come because the reference value you mention is not able to be created. Suppose an error throws at API gateway it will say unresolved reference lambda resource. You have to look into the CloudFormation event console. Below are some error examples from the CloudFormation stack console. 

![Screenshot 2021-09-17 at 12.41.58 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631862739191/pwt68xxQX.png)

![Screenshot 2021-09-17 at 12.42.48 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631862785673/WZBSIrTSr.png)

Hope you enjoyed and learned from this blog about creation of API gateway error mechanism using CloudFormation. 

Thanks for reading.  If you have any Queries or Suggestions, feel free to reach out to me in the Comments Section below or my twitter handle @aviboy2006. 


**References** : 

-  [Github Example](https://github.com/aviboy2006/lambda-apigateway-sns-cnf-awsugblrdemo) 
-  [Session Demo](https://www.youtube.com/watch?v=lG-8JV-aXqI&t=2255s)  
-  https://aws.amazon.com/cloudformation/faqs/


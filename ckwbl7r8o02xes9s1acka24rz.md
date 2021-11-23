## How to use AWS Amplify environment variable in React ?

Hello Devs, 

Recently delivered session at Jaws Pankration 2021, Japan on **how to use AWS Amplify for static web hosting** and usage of environment variables for React application. Writing this blog on similar topics for references. 

Let's understand about **AWS Amplify** first. 

### What is AWS Amplify ?

**AWS Amplify** is a package of tools and services. Before amplify came into picture AWS was providing static hosting using S3 bucket. Problem with S3 was that only any library installation like node modules had to do it before pushing code into S3 bucket. To solve this problem and make stronger and better solution come with Amplify console.

- To accelerate deploying app over AWS cloud
- Make more easier installation of dependent library
- built-in CLI

How to host web apps ( React, Angular, Static website, other JS Framework etc. ) using AWS Amplify can find steps here -  https://www.internetkatta.com/static-hosting-of-angular-build-using-aws-amplify

Now let's checkout how to use the Amplify environment variable console to pass variables to code like in React etc. I haven't try yet another framework like Angular. 

Mostly we always worried about where to keep environmental value like:

- Third-party API keys
- Different customisation parameters
- Secrets 

if web apps are going to host as static hosting. It is not recommended to keep under git repository or inside code. To solve this issue Amplify provide  environment console UI where we can set environment variable.Once we set those variable we have to do small changes in build configuration under `amplify.yaml` file as shown below image.

Environment variable setting console. Navigate to Amplify Console -> Select App -> App Setting -> Environment Variables 

![environment-variable.png](https://s3.amazonaws.com/demo.avinashdalvi.com/assets/environment-variable.png)

Example of `amplify.yaml for React`. Because React require environment variable should have prefix `REACT_APP`
```
version: 1
applications:
  - frontend:
      phases:
        preBuild:
          commands:
            - npm ci
        build:
          commands:
            - REACT_APP_ENV_API_KEY=${REACT_APP_ENV_API_KEY}
            - npm run build
      artifacts:
        baseDirectory: build
        files:
          - '**/*'
      cache:
        paths:
          - node_modules/**/*
    appRoot: demo-app
```
and this variable can be access in React code like `process.env.REACT_APP_ENV_API_KEY`

**Demo link**: https://jawspankration2021-demo.avinashdalvi.com

**Code** : https://github.com/aviboy2006/jaws-pankration-2021/blob/main/demo-app

Hope this blog helps you. If you like my blog please don't forget to like the article. It will encourage me to write more such AWS Cloud related articles. You can reach out to me over my twitter handle @aviboy2006


### References : 

- https://docs.aws.amazon.com/amplify/latest/userguide/environment-variables.html
- https://stackoverflow.com/questions/64072288/how-to-add-environment-variables-to-aws-amplify


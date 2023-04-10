---
title: "Why I build Virtual PhotoBooth using AWS Amplify ?"
datePublished: Thu Apr 06 2023 10:28:21 GMT+0000 (Coordinated Universal Time)
cuid: clg4z65ju000609mdbkmt8vtn
slug: why-i-build-virtual-photobooth-using-aws-amplify
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680379011949/fadcf1fb-f9c1-4488-9a3c-5b64d5ec0185.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1680379024935/e70d64a7-22fa-4446-9427-126d9983142a.png
tags: aws, awscommunity, amplifyhashnode, awsamplify, awsamplifyhackathon

---

### The history behind this:

> "Problems are nothing but wake-up calls for creativity" – Gerhard Gschwandtner

The above quote is the inspiration behind every solution which exists at this moment in the whole world.

Who would like to take photos with a Photo Booth?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680246344709/2d513dff-d928-4d86-ac5b-bd12984b5091.gif align="center")

To my knowledge everyone like it.

I am going to talk about Building a virtual photo booth with AWS Amplify.

Before we kick off, how do we develop this? Let's check out where this idea came into mind.

Last year I attended the Mauticon conference virtually. At the event, they introduced a virtual photo booth and they purchased this tool as part of virtual hosting. I like this concept so at the time of AWS Community Day South Asia 2021 thought of using this. Unfortunately, I couldn't find any free tool to use. Time was less so I dropped this idea. In the 2022 year when ACD India preparation started, I decided to build this for the community. I discussed this with the ACD India team and started working on it.

### Started building

As an initial process, I generally follow the below steps to build any application or website.

1. find out what is functionality needed to have in the application then write pseudo or working sample code for it.
    
2. Create a sample page or app using sample code.
    
3. The design is on rough paper or Figma whichever you are comfortable to visualise what the user interface should look like.
    
4. The last step is to integrate design and functionality to make the application work.
    

I started working on the first functionality wise. So, I wanted to write two different logic one is to capture a camera photo under frame and that same photo have to download as an image.

* I created one simple HTML-based page with Javascript.
    
* Created photo frame sample image from Canva.
    
* Created a nice logo for it.
    
* Decided tagline "Click me & frame me!"
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680287063304/da1bec98-ae4d-4134-8e9e-6099771bd77b.png align="center")

Functionality and design were ready but some UI fixes and photo frame adjustments took some time to fine-tune. Finally!! I made it and launched it on AWS Community Day India 2022 website for use on events to capture selfie and post it over social media.

Being a full-stack developer I used my all skill set to make this tool more useful and mobile-friendly. Here is a success story of a virtual photo booth used in the AWS Community Day India 2022 event.

%[https://youtube.com/shorts/b55GKmhTF-c?feature=share] 

### Idea got extended

During ACD India event preparation time CFP got opened for AWS Community Day, Pune. I thought to apply there and showcase the same story about How I built this virtual photo booth. I discussed this idea with @[Jones Zachariah Noel N](@zachjonesnoel) . he told me can we make a fully customisable tool which any community or group can use for their event. Then we decided to build this virtual photo booth with AWS Amplify studio with the same admin functionality. We decided to add some more functionality like :

* Admin login using Cognito
    
* Admin or user can upload their photo frame to a customised virtual photo booth
    
* Admin or user can upload multiple photo frames to give the option to end user to use any frame which they like to capture a photo.
    

**Tech stack we used** :

* Amplify hosting
    
* React for frontend
    
* Amplify Authentication
    
* Amplify API - GraphQL
    
* Amplify Storage -Assets
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680367799079/9de9b5c3-a1f4-4812-935d-135ccd31bd71.png align="center")
    

This application is available as IaC, so developers can easily clone the repository and deploy it as an Amplify app on their desired AWS account. Code is available on GitHub for contribution and cloning.

Don't forget to drop Star if you like this project.

[https://github.com/AvinashDalvi89/virtual-photobooth](https://github.com/AvinashDalvi89/virtual-photobooth)

### How to use it?

Just simply follow the command and run on the terminal. Make sure you have the right AWS credentials set in your machine. If you haven't set AWS credentials then check out this [https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

1. `amplify init`
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680771865162/dc117128-e5fb-453f-905f-673a8430f749.png align="center")

1. `amplify add auth`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680772224140/c04dd644-a9d6-4bb6-bf96-23a4ed0aad79.png align="center")
    
2. `amplify add api`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680772324218/4545f515-c4a6-4b3f-b9bf-4589c61fda32.png align="center")
    
3. `amplify add storage`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680772577374/a10da450-807c-461b-a2bc-4a7619069347.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680772581136/7886dcf1-54f5-4412-9f4d-a493d93c8e34.png align="center")
    
4. `amplify add hosting`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680772621921/eea3123e-4718-45a1-8517-d54d1b7bbad8.png align="center")
    
5. `amplify status`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680772652311/98cbff44-3277-4a6e-b114-0b8a0dbe466a.png align="center")
    
    Copy this URL for later reference to see the application.
    
6. `amplify publish`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680772683389/d8daf8bd-dfea-492b-9a72-dbca6fe1de49.png align="center")
    

### Demo

[https://main.dgqdyv9uzmpoc.amplifyapp.com/](https://main.dgqdyv9uzmpoc.amplifyapp.com/) - This is just the front end of the application.

### Why Amplify?

AWS Amplify has various tools available for Front end app development and deployment. With Amplify, you can configure app backends and connect your app in minutes, deploying static web apps in a few clicks.

I called it a Swiss army knife. Because it is a multipurpose and easy-to-use developer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680368297794/927fc72d-2b40-453b-9c7f-c7937f60f518.png align="center")

### What is the further plan?

* Improvement of some UI component
    
* Add some more customisation functionality like directly posting to social from the web page itself.
    
* Analytics of posts using hashtag count etc.
    

These are very high-level thoughts in my plan but depending on how I can make it based on availability. Anyone interested to contribute feels free to drop a message on the GitHub repository or Twitter handle [AvinashDalvi\_](https://twitter.com/avinashdalvi_)

I hope this blog helps you to learn. Feel free to reach out to me on my Twitter handle @[@AvinashDalvi_](@AvinashDalvi_) or comment on the blog.

### References :

* [https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)
    
* [https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html)
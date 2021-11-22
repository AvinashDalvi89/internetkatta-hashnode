## Static hosting of Angular build using AWS Amplify ?

Hello Developers, 

In previous blog  [Host Angular 2 or 4 or 5 version on AWS S3 using CloudFront](https://www.internetkatta.com/host-angular-2-or-4-or-5-version-in-aws-s3-using-cloudfront) you have learn about hosting using AWS S3 and CloudFront. In this blog I am going to explain about static hosting of Angular using AWS Amplify. There is AWS official blog available to host Angular app directly on Amplify - https://docs.amplify.aws/start/q/integration/angular

 [AWS Amplify](https://aws.amazon.com/amplify/) has various tools available for Front end app development and deployment. With Amplify, you can configure app backends and connect your app in minutes, deploy static web apps in a few clicks. 

![list of Amplify tools](https://cdn.hashnode.com/res/hashnode/image/upload/v1609089155585/EZH1pC03T.png)

Out of this above list we will explore **Manage hosting** tool which enables static website hosting by linking different ways like Github, Bitbucket, Gitlab, CodeCommit etc. This way you have to give permission using Oauth to access the list of repositories and their content. 
![list of ways to upload code](https://cdn.hashnode.com/res/hashnode/image/upload/v1609089477504/k22LTtek4.png)

Lets see how its works : 
![how its works](https://res.cloudinary.com/practicaldev/image/fetch/s--Xc1ItKES--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1609089854576/7OoCnsGbJ.png)

1. Authenticate git tools 
2. It will show a list of repositories from the git account. Choose which repository content would like to host 
![list of repositories](https://res.cloudinary.com/practicaldev/image/fetch/s--rQzOCNsG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1609090042243/vlxLh29RF.png)
3. Configure build setting ( Optional)
4. Review setting and deploy. Once click on Start Deploy it will show a message like "Creating app: angular-hosting-amplify-example in progress..." 
![Deployment review page](https://cdn.hashnode.com/res/hashnode/image/upload/v1609091010984/MH7i_6pa9.png)
5. You can track status or progress app status page like shown below 👇🏻 and once it's completed all stages will be green colour. 
![status page](https://res.cloudinary.com/practicaldev/image/fetch/s--nSJ_CA7Z--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1609091232376/KTEutRF1a.png)
6. This will give a link for the app to show.  Link will be like this [https://main.d26gqyfusfkmyj.amplifyapp.com](https://main.d26gqyfusfkmyj.amplifyapp.com) 
![Webpage](https://res.cloudinary.com/practicaldev/image/fetch/s--q_BZ7qSF--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1609091379084/919c6zwXY.png)
We have done till uploading code and deploying to URL. Completed half battle to host Angular app generated build. 

![move-it.gif](https://res.cloudinary.com/practicaldev/image/fetch/s--Ii3ONmXn--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1609091481619/Qc3CYsF5p.gif)

**Note**:  Amplify app frontend hosting doesn't require any routing understanding like we saw AWS S3 hosting. 

Last steps to move this app URL to a custom domain like amplifydemo.avinashdalvi.com. Let's see what we need to do to map custom domain to Amplify app. 
1.  In Amplify App console go to -> Domain management 
2. Click on Add Domain 
![Screenshot 2020-12-27 at 11.32.01 PM.png](https://res.cloudinary.com/practicaldev/image/fetch/s--5fQqXvMA--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1609092133540/YQ9bY54da.png)
3. Enter the root domain name like if you would like to use www.example.com then your root domain will be example.com. In demo i will chose avinashdalvi.com as root domain because app domain name will be amplifydemo.avinashdalvi.com.
4. Click on Configure Domain.
5. Exclude the root domain if you are not required. The subdomain name is amplifydemo. Once this finish it will status page like below 👇🏻
![status page of domain](https://res.cloudinary.com/practicaldev/image/fetch/s--oChjgkDl--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1609092455056/6SpRRlvvl.png)

**Note**: Domain name validation for certificate and DNS validation sometimes takes more than 8 hours. If its take more than 8 hours then create  [github issue here](https://github.com/aws-amplify/amplify-console/issues/new?body=%0A**App%20Id**:%20d26gqyfusfkmyj%0A**Region**:%20us-east-1%0A**Step**:%20SSL%20configuration%0A**Status**:%20Running%0A%0A%3E**Note**:%20Do%20not%20include%20information%20that%20is%20sensitive%20in%20nature%20such%20as%20your%20domain%20name,%20company%20etc.%0A%0A**Issue/question**%0AA%20clear%20and%20concise%20description%20of%20what%20the%20issue/question%20is.%0A%0A**Error%20message**%0AIf%20there%20is%20an%20error%20message,%20please%20include%20it%20here.%20%0A**Note**:%20Be%20sure%20to%20check%20the%20message%20for%20sensitive%20information.%20%0A%0A**Additional%20information**%0APlease%20add%20any%20other%20relevant%20information.%20Please%20feel%20free%20to%20include%20screenshots.%0A%0A&labels=Custom%20domain&template=domain_issue.md&title=SSL%20configuration%20is%20Running%20-%20[BRIEF%20DESCRIPTION])  

Then we are done with the final step.

![finally-completed.gif](https://res.cloudinary.com/practicaldev/image/fetch/s--TwtQzac5--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1609093190381/GiOtiG-JH.gif)

Hope you like my blog. Thanks for reading my blog. If you have any questions you can reach out to me at my twitter handle - @aviboy2006

**Note** : Mentioned sample demo app will work or not also. This is just for reference. This demo assume your Angular build is already there after running command `ng build`

References : 
- https://aws.amazon.com/amplify/
- https://aws.amazon.com/amplify/hosting/
- https://docs.aws.amazon.com/amplify/latest/userguide/custom-domains.htm
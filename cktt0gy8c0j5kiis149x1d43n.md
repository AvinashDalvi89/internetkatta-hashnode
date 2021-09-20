## How AWS CloudShell can help you ?

Today, I was doing some experiments on EC2 instances. While doing the experiment I faced one issue: launch failure. It is like launching template failed, to debug this and to know about what is this error, I went through  [AWS troubleshooting documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ts-as-instancelaunchfailure.html#ts-as-instancelaunchfailure-3) and in that document it was mentioned about trying out `describe-instance-type-offerings` command to see what instance types are offering. I was not using my personal machine where my AWS account or credentials are configured. I was thinking what should I do now ? 🤔🤔
![thinking-think-about-it.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1632160947884/dbu6RUse5.gif)

Then I saw a symbol on top navigation of AWS UI about launching an online AWS cloud console UI. Wow, it's superb... Now I no need worry about accessing AWS CLI even if I am not in front of my personal device. If I can access my AWS account from a browser then I can access AWS CLI. 

Interesting facts about AWS CloudShell is it was launched around Dec 2020 but I got to know now. Somebody said "**Problem come with solution and solution come with learning**" 

![Screenshot 2021-09-20 at 11.33.15 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632161021341/fwGtAKc62.png)

Let me walkthrough about AWS CloudShell. This is how its looks a like 👇🏻

![Screenshot 2021-09-20 at 11.41.27 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632161492951/2y9DEkx8Y.png)

### Benefits of AWS CloudShell : 

- No extra credentials to manage
- No cost ( 😅😅 ) - There is no additional charge for AWS CloudShell.
- Automatically manage your credentials
- Customisable
- Familiar tools ( Similar like our Windows or Linux or Mac terminal  👍🏻) 
- 1 GB of persistent storage ( storage enables you to store your frequently used scripts/commands and configuration files between AWS CloudShell sessions)

![Screenshot 2021-09-20 at 11.24.11 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632161089553/FIe8xfh9C.png)

### What comes under customisation of AWS CloudShell : 

- Row layout 
- Column layout ![Screenshot 2021-09-21 at 12.00.03 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632162609463/4BnB-eXaB.png)
- Font size 
- Light or Dark mode 
- Enabling safe paste ( This can ask for permission before pasting command or text)

![Screenshot 2021-09-21 at 12.04.08 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632162873473/zPAM2zjRf.png)

### Things to Know: 

- Each CloudShell session will timeout after 20 minutes or so of inactivity, and can be reestablished by refreshing the window. You will see something like this error 👇🏻

![Screenshot 2021-09-20 at 11.51.56 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632162139676/ndJ9V3PR8.png)
- AWS CloudShell is available today only in the `us-east-1`, `us-east-2`, `us-west-2`,`eu-west-1`, `ap-south-1`, `ap-southeast-2`, `ap-northeast-2` and `eu-central-1` Regions. Other region support on road map. 
- AWS CloudShell supports Python and Node runtimes, Bash, PowerShell, jq, git, the ECS CLI, the SAM CLI, npm, and pip.
- Concurrent 10 tabs you can open in each region.
- Default output of `pwd` is `/home/cloudshell-user` 
  ```
[cloudshell-user@ip-10-1-94-160 ~]$ pwd
/home/cloudshell-user
[cloudshell-user@ip-10-1-94-160 ~]$ 
  ```
- You can download files from the terminal or upload files to the terminal. 
![Screenshot 2021-09-21 at 12.03.25 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632162820330/QCsvgK92n.png)
- CloudShell home directory ($HOME) and files, scripts, and tools saved in $HOME will persist between sessions

Thanks to AWS services for providing such good tools. Hope you like my blog about AWS CloudShell. If you like my blog please don't forget to like the article, It will encourage me to write more such AWS Cloud related articles. You can reach out to me over my twitter handle @aviboy2006

### References: 
- https://aws.amazon.com/about-aws/whats-new/2020/12/introducing-aws-cloudshell/
- https://aws.amazon.com/blogs/aws/aws-cloudshell-command-line-access-to-aws-resources


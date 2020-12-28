## Code review process using AWS CodeGuru tool

Hello Devs,

In this article, we will learn how to do the code review process using AWS CodeGuru. Let me tell you first what is CodeGuru and how it works ?

**What is code review **: 

Code review process includes suggestions for optimization of the most expensive line of codes. It checks whether code is readable and understandable by a naive user or not. It includes how function naming has done or getting called etc. 


**AWS CodeGuru** : 

AWS CodeGuru is a developer tool that provides intelligent recommendations to improve your code quality and identify an application’s most expensive lines of code. This automation works based on generalised guidelines given by open source code communities and language based. CodeGuru Reviewer uses machine learning to identify critical issues, security vulnerabilities, and hard-to-find bugs during application development to improve code quality. It focuses more on the most expensive lines of code to improvisation. 

**How its works** : 

![CodeGuru](https://cdn.hashnode.com/res/hashnode/image/upload/v1609164859547/wU8MSaZxn.png)

**Benefits of CodeGuru** :

- Troubleshooting performance issues
- Discover inconsistency and common issues in your application performance
- Catch your most expensive line of code today 

**Steps to start of CodeGuru**: 

1. You should have an AWS console account to access AWS CodeGuru.
2. Go to Search Console -> Open CodeGuru 
3. Click on Associate Repository option which show on dashboard or console 
![Screenshot 2020-12-28 at 8.02.19 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609166072099/R2as-rf6D.png)
4. Select which repository management tool like Github, CodeCommit, BitBucket etc. You need to authorise respective account by login to site and give permission. 
![Screenshot 2020-12-28 at 8.05.37 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609166223264/IvSyZnsWw.png)
5. Click on Associate after selecting the repository from list of repository. 
6. Tag is an optional step to identify a project. It will show like below Associating...
![Screenshot 2020-12-28 at 8.10.21 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609166435789/eaMpsEy-9.png)
7. Then select Code Reviews from the left side menu. 
![Screenshot 2020-12-28 at 8.12.36 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609166566748/EM1ayZXr-.png)
8. Create Repository Analysis for each repository. 
![Screenshot 2020-12-28 at 8.14.09 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609166661720/QhP8EG9NU.png)
9. It will go to a pending state. Once it's completed its will show result like below 👇🏻

![Screenshot 2020-12-28 at 8.15.09 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609166730143/KYdDs9E5l.png)

Then we are done with the code review process. You can work on recommendations given based on analysis. Fix those issues and run it again. 

Hope you like my blog. Thanks for reading my blog. If you have any questions you can reach out to me at my twitter handle - @aviboy2006


**References**: 
- https://aws.amazon.com/codeguru/
- https://aws.amazon.com/blogs/devops/raising-code-quality-for-python-applications-using-amazon-codeguru/
- https://www.tutorialspoint.com/software_testing_dictionary/code_review.htm
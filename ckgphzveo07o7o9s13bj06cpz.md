## AWS Lambda monitoring mechanism using SNS

Hello Devs,

I am going to tell you about AWS Lambda monitoring failure or success mechanism using SNS and email notification. Let's understand first background behind this. 

Few months back, i developed one AWS Lambda code using Python. It was working perfect fine. I used one 30 days trial API for testing purpose. Generally API vendor keep sending reminder about expiring free trial period. On last day i didn't get mail or somehow i forgot to check my mail. Due to expiry of API service lambda service stop working this i got to know some of user respond back with failure of service. Then i stared exploring about how should i get information about any kind of failure if something happen wrong in execution of lambda. 

After reading AWS SNS documentation, I found interesting thing about AWS is they introduced lambda trigger end point to SNS services which has multiple communication end point to subscribe like email, SMS, Web hook , External endpoint etc. 👇🏻

![List of Endpoint](https://cdn.hashnode.com/res/hashnode/image/upload/v1603486338812/YMW358nHT.png)

**Lets start how to do this** ? 

Architecture flow diagram : 

![SNS SMS subscription](https://d1.awsstatic.com/product-marketing/SNS/Product-Page-Diagram_Amazon-SNS_2-SMS-How-it-works@1.5x.4c649e5fe667e525de01e8c88024d782d8a25ccf.png)


1.  Create lambda - can use any language like Python, Node, GoLang etc. 
![Lambda Creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1603489210987/q9xXK6Ft1.png)
2. Create SNS topic : Go to SNS -> Create Topic -> Select name and display name -> Create 
![Topic creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1603489305029/jUwBKpm4Z.png)
3. Creation subscription for topic: Select Topic -> Create subscription-> Select protocol show in above image-> Create 
4.  Go to SNS -> Mobile -> Text Messaging -> Text messaging preferences -> Edit -> Add budget per day ( $1 per day )  [SMS pricing](https://aws.amazon.com/sns/sms-pricing/) 
5. Make sure lambda execution role has access to publish to SNS topic. 
6. Go to lambda -> Add Destination -> Asynchronous invocation -> Select Success or Failure -> Destination as SNS -> Select topic 

They we are done! 👍🏻

![Screenshot 2020-10-24 at 3.17.36 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603489668293/whnkVjDQM.png)

**Sample SMS text** : 

![Setting for SMS.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603489492887/QkI2HknJS.png)
![SMS notification](https://cdn.hashnode.com/res/hashnode/image/upload/v1603488842742/cyFbXf9BI.png)

**Demo Video** : 

<iframe width="560" height="315" src="https://www.youtube.com/embed/FN77CJGy6Tk" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Thanks for reading article. Share your valuable feedback and suggestions! Don't forget like, share and subscribe.


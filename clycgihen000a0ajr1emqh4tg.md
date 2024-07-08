---
title: "Serverless: An Evolving Architecture, Not Just a Runtime"
seoTitle: "Serverless: Evolving Beyond Runtime"
seoDescription: "Explore the evolution of serverless computing, its benefits, and architectural significance beyond just a runtime environment"
datePublished: Mon Jul 08 2024 04:02:55 GMT+0000 (Coordinated Universal Time)
cuid: clycgihen000a0ajr1emqh4tg
slug: serverless-an-evolving-architecture-not-just-a-runtime
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1720410877638/5ee6f17f-4e86-4992-b3a3-a5ecf8fc6a79.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1720411027963/7b3c2e43-71e2-47cd-b1a8-eb217918025f.png
tags: lambda, aws, architecture, cloud-computing, serverless, aws-lambda, serverless-architecture, serverless-computing, serverless-consulting

---

> "The reports of my death have been greatly exaggerated." - Mark Twain

This famous quote applies just as well to technology as it does to people. In the fast-paced world of software development, we often hear proclamations about the death of one technology or another. But the truth is, technologies rarely die outright - they evolve, transform, and find new niches.

Take serverless computing, for instance. When it first emerged, some viewed it as merely a new runtime environment. But as the concept has matured, it's become clear that serverless represents something much more profound: a shift in how we architect and think about applications.

I am writing this blog to express my views about Serverless. I believe that every technological evolution happens to solve a problem or meet a specific need. I am always amazed by people who write or create content around the idea that "This technology is dead." I have been in the software development career for the past 13 years and have seen the evolution from VB to [VB.NET](http://VB.NET), ASP to [ASP.NET](http://ASP.NET), PHP, Python, Java, CSS to SCSS, HTML to Web3, and many more technological advancements. To be frank and realistic, even if we move from one point to another, the older point is still valid for certain use cases. They never die; they evolve with the demand. There's no one-size-fits-all solution; the perfect choice depends on the project. Currently, everyone wants to ride the wave of AI and Generative AI, but in reality, many people still haven't found the right use case for it. It can't be fitted everywhere, or maybe something different will come along that fits better. Serverless is the same. Serverless came into the picture almost immediately after the cloud era started because of the need for a situation where developers were more in number, and managing or scaling servers was painful or not easy to do.

To understand why serverless computing emerged as a solution, let's explore the specific challenges it was designed to address.

## The Birth of Serverless

To understand serverless, we need to look at the problems it was born to solve.

### Focus on code, not servers

As a developer who understands the challenges developers face, I can explain this better. I am a big fan of the cloud era and a dedicated full-stack developer. When the cloud era began, dedicated cloud operations or DevOps were not as popular as they are today. Now, Cloud Ops and DevOps are crucial parts of an application team. Back then, developers had to take on the challenge of creating, managing, and scaling servers based on thresholds. This required both time and effort, diverting them from their primary role as developers. Although I personally enjoyed that work, companies couldn't afford to invest that much time and effort.

Enter serverless, promising to abstract away infrastructure concerns and allow developers to focus solely on writing code. Beyond simplifying development, serverless also brought significant cost benefits.

### Cost efficiency:

Along with this, customers started complaining about getting huge bills without making much profit in their business. Cost was a major factor! They were paying for resources that weren't fully utilized and not getting any business value out of it. That's when the "Pay-per-use model" emerged with Serverless, and it was a game-changer! In addition to cost savings, serverless computing also revolutionized scalability.

### Improved scalability:

Scalability existed in traditional infrastructure, but it was a burden on developers. Serverless eliminates the need for developers to provision extra servers in anticipation of spikes or worry about under-provisioning during peak times. Unlike traditional architectures where you might hit a ceiling based on your provisioned servers, serverless can theoretically scale infinitely (though providers may impose some limits). Another critical advantage of serverless is the speed at which developers can deploy applications.

### Faster deployment :

Developers need to provision servers and manage them. All this server management slows down deployments. Developers can't focus on writing code and pushing updates because they're bogged down with infrastructure tasks. They wanted to push code frequently but lacked the time to do so.

## Microservice was perfect match

Microservices emerged from the frustration of dealing with monolithic codebases. The rise of microservices and serverless computing is indeed closely intertwined. That's why I said it was a perfect match! When you're running granular pieces of code, why keep a server running at full capacity? The pay-per-use model was perfect for running microservices. Focus on code, not on servers! Both evolutions shared the same goal: to help developers. That's where the magic happened!

At the same time, frontend frameworks like Angular, NextJS, and React were taking shape, focusing on API-based backends and frontends. This synergy boosted microservices, and microservices, in turn, supercharged serverless!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1720211183647/cd9b4bb7-f55e-45ca-8319-3ab32a792e42.jpeg align="center")

This synergy was highlighted by the launch of AWS Lambda, which became a key example of serverless computing.

## AWS Lambda = Serverless

So, when the first serverless applications started to get built on AWS, the initial approach was “let’s build microservices”…

The first serverless offerings, like AWS Lambda in 2014, were indeed focused on providing a runtime environment for small, event-driven functions. This led to the initial perception of serverless as simply a new way to run code. Again, it was a perfect match for microservices with the help of Lambda.

Serverless was creating pave to become concept to a way of building and deploying applications where you don't have to manage servers. The cloud provider handles all the infrastructure provisioning, scaling, and maintenance. Same time AWS Lambda was paving foot as function as a service: It's a specific offering from Amazon Web Services (AWS) that allows you to run code in a serverless fashion. You write your code, upload it to Lambda, and define events that trigger the code's execution. Lambda takes care of everything else.

One of my favorite analogies, which I've used in many talks about serverless, is **Ordering pizza is serverless**. Isn't that intriguing? Here's a short video about it.

%[https://youtube.com/shorts/J5tCpL4EbYY?feature=share] 

As serverless computing continued to evolve, its potential expanded far beyond initial expectations.

## Evolution of the Serverless Paradigm

As serverless matured, its true potential became apparent. It wasn't just about running functions; it was about reimagining how applications are built, deployed, and scaled. Key developments included:

* **Expansion beyond functions**: Serverless databases, storage, and other managed services emerged, enabling entire applications to be built on serverless principles.
    
* **Improved developer experience**: Tools and frameworks evolved to make serverless development more accessible and productive.
    
* **Performance enhancements**: Cold start times decreased, and providers introduced ways to keep functions "warm," addressing early criticisms.
    
* **Enterprise adoption**: Large organizations began embracing serverless for critical workloads, driving further innovation.
    

A key aspect of this evolution is the event-driven nature of serverless architectures."

## **Serverless is all about events**

While functions are a core component, events are the lifeblood that triggers those functions and makes everything work together. This statement captures a fundamental characteristic of serverless architectures. At its core, serverless computing is designed to respond to and process events efficiently.

Here's a deeper look at this concept:

### Event-Driven Architecture

Serverless platforms are founded on the principle of event-driven architecture. This means that serverless components are activated by specific events rather than running continuously. It is recommended to bypass the API Gateway whenever possible and use direct event triggers. This approach results in faster and more efficient execution, particularly for internal communication within AWS services. These events can originate from a wide range of sources. :

* HTTP requests (API calls)
    
* Database changes
    
* File uploads
    
* IoT sensor data
    
* Message queue events
    
* Scheduled tasks
    
* Custom application events
    

### Beyond Functions

While Functions-as-a-Service (FaaS) like AWS Lambda or Azure Functions are often the first thing people think of with serverless, the ecosystem is much broader:

* **Event Streams**: Services like AWS Kinesis or Azure Event Hubs allow for real-time processing of data streams.
    
* **Notification Services**: AWS SNS or Azure Notification Hubs enable push notifications triggered by events.
    
* **Message Queues**: Services like AWS SQS or Azure Service Bus decouple components and handle asynchronous processing.
    
* **Event Buses**: AWS EventBridge or Azure Event Grid route events between decoupled components.
    
* **Serverless Databases**: DynamoDB streams or Azure Cosmos DB change feed can trigger actions based on data changes.
    
* **API Gateways**: Managed services that handle API requests and can integrate directly with other serverless components
    

Understanding the event-driven nature of serverless helps us appreciate its broader architectural implications.

## Serverless: An Evolving Architecture

Today, serverless is best understood not as a specific technology but as an architectural approach emphasizing:

* Event-driven design
    
* Fine-grained scalability
    
* Pay-per-use pricing
    
* Managed services and infrastructure
    
* Focus on business logic over operational concerns
    

This shift in thinking has far-reaching implications for how we design, build, and operate software systems.

However, like any technology, serverless computing comes with its own set of challenges.

## Challenges:

* **Vendor Lock-In:** Reliance on a specific cloud provider's serverless platform can make it difficult to switch later.
    
* **Limited Control:** Developers have less control over the underlying infrastructure compared to traditional deployments.
    
* **Debugging Challenges:** Debugging serverless applications can be more complex due to their distributed nature.
    

Despite these challenges, serverless computing exemplifies a broader truth about the persistence and evolution of technology.

## The Persistence of Technology

The evolution of serverless illustrates a broader truth about technology: it rarely dies, but instead transforms and finds new applications. Other examples include:

* **Mainframes**: Once thought obsolete, they remain critical in industries like finance and government.
    
* **SQL databases**: NoSQL was supposed to kill SQL, but relational databases adapted and remain widely used.
    
* **Monoliths**: Despite the microservices revolution, monolithic architectures still have their place in certain scenarios.
    

These examples illustrate that technologies rarely die; they adapt and find new applications. This leads us to an important lesson for developers.

## The Lesson for Developers

As technologists, we should be wary of declaring any technology "dead." Instead, we should focus on understanding the strengths, weaknesses, and appropriate use cases for different approaches. Serverless isn't replacing all other architectures, but it's certainly earned its place in the modern developer's toolkit.

In conclusion, serverless computing demonstrates that true innovation often lies not in creating entirely new technologies, but in reimagining how existing concepts can be applied to solve real-world problems. As we continue to evolve our architectural approaches, let's remember that the most powerful solutions often come from synthesis and adaptation rather than revolution.

I hope this blog helps you learn! If you have any questions or just want to chat about serverless, feel free to reach out to me on my Twitter handle [@AvinashDalvi\_](https://twitter.com/AvinashDalvi_) or leave a comment on the blog.

### References :

* [https://www.antstack.com/blog/history-of-serverless/](https://www.antstack.com/blog/history-of-serverless/)
    
* [https://pauldjohnston.medium.com/serverless-and-microservices-a-match-made-in-heaven-9964f329a3bc](https://pauldjohnston.medium.com/serverless-and-microservices-a-match-made-in-heaven-9964f329a3bc)
    
* [https://aws.amazon.com/blogs/devops/refactoring-to-serverless-from-application-to-automation/?ref=dailydev](https://aws.amazon.com/blogs/devops/refactoring-to-serverless-from-application-to-automation/?ref=dailydev)
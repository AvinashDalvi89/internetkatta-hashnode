## Why is security important in infrastructure as code ?

Hello 👋🏻 Devs, 

Recently delivered session at DevSecOps Conference 2022 on **Infrastructure as code (IaC) and how to keep secure** and best practices to follow. Writing this blog on similar topics for references. To deliver this session I did a lot of research and read many blogs to collect all information. This information is totally based on research. 

Let's understand about **IaC** first what exactly it does and why etc. 


### What Problem Does IaC Solve?

With the “what” out of the way, let’s turn our focus to the “why” of infrastructure as code. Why is it needed? What problem does it solve?

**Infrastructure as code (IaC)** means to manage your IT infrastructure using configuration files.

**Infrastructure as Code** evolved to solve the problem of environment drift in the release pipeline. Without IaC, teams must maintain the settings of individual deployment environments.

Challenges of Managing IT Infrastructure. 

- Cost of infra
- Scalability and availability
- Monitoring and performance visibility

### What is an IaC? 

**Infrastructure as code (IaC)** means to manage your cloud or IT infrastructure using configuration files

![IacThoughts.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641669797600/-ct106-DI.png)

### Who are provider for IACs ?

- AWS CloudFormation
- Azure Resource Manager
- Google Cloud Deployment Manager
- Terraform

There are many providers who enable IaC but these are widely used providers. The first three are considered native IaC providers, and their offerings work best inside their own clouds. IaC templates from all four providers can be written in JavaScript Object Notation (JSON) format, but JSON syntax can be tricky to understand, and it’s also error prone. For this reason, three of the four IaC providers have enabled the use of YAML ( which humorously stands for **Y**et Another **M**arkup **L**anguage).

The only collective drawback for CloudFormation, ARM and Google Cloud Deployment Manager is they’re more suitable for their own clouds and not for organisations wishing to leverage multi-cloud environments. Here Terraform rocks. Terraform delivered IaC which encapsulates all major clouds in one.

### Keeping infrastructure as code is vulnerable ?

- Secrets and stuff in CloudFormation
- Push CF directly instead of going through Git and without versioning
- Without validating directly push nested config
- Learning Curve

Infrastructure as code is a powerful tool, but a risk of utilising it includes propagating small configuration mistakes across the cloud infrastructure. Misconfigurations may take several different forms, like
 - Insecure default configurations–including nearly half of CloudFormation templates. 
 - Other forms of misconfiguration include publicly accessible S3 buckets or unencrypted databases.

Detecting and fixing misconfigurations helps eliminate “environmental drift”, a scenario in which the configurations for different deployment environments fall out of sync with their templates. For some resources CF doesn't do proper cleanup of resources - so the templates should be broken down appropriately.

### What steps can be taken to keep secure ? 

- Prevent Hard Coded Secrets From Permeating IaC
- Reduce The Time And Impacts Of Code Leaks
- Restrict Access to Environments
- Prevent IaC Code Tampering
- Avoid Complexity
- Alert on Failures


### Best practices to keep IAC as secure as possible and scalable.

- Go native whenever possible 
- But consider multi-cloud
- Also consider vendor lock-in
- Terraform
- Use an Immutable Infrastructure Approach
- Use Version Control for IaC Files
- IaC Compliance Regulation
- Don’t Store Secrets in IaC Definitions
- IaC can be used to update resources once they are already running.
It’s a best practice to scan IaC files automatically and continuously, ensuring that validation occurs whenever an IaC definition is created or updated.

I generally call IaC a magician which helps to do all magic under cloud infra. 
![magic.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1642086248843/iwpkukKFF.gif)

Created one short introduction video for IaC. watch here 👇🏻

<iframe width="560" height="315" src="https://www.youtube.com/embed/bvkjaw2uEo0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Hope this blog helps you. If you like my blog please don't forget to like the article. It will encourage me to write more helpful articles. You can reach out to me over my twitter handle @aviboy2006

### Reference : 

* [What is infrastructure as code](https://docs.microsoft.com/en-us/devops/deliver/what-is-infrastructure-as-code)
* [What Is Infrastructure as Code? How It Works, Best Practices, Tutorials](https://stackify.com/what-is-infrastructure-as-code-how-it-works-best-practices-tutorials/)
* [AWS IaC CloudFormation](https://aws.amazon.com/cloudformation/)
* [Azure resource manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview)
* [Google Cloud manager](https://cloud.google.com/deployment-manager/docs)
* [Terraform](https://www.terraform.io/docs)


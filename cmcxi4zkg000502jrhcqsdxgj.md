---
title: "Migrating from EC2 to Containers: What Teams Miss"
seoTitle: "EC2 to Containers: Common Migration Challenges"
seoDescription: "Explore the challenges of migrating from EC2 to containers. Learn insights from real-world experience to optimize your deployment strategy"
datePublished: Thu Jul 10 2025 14:47:27 GMT+0000 (Coordinated Universal Time)
cuid: cmcxi4zkg000502jrhcqsdxgj
slug: migrating-from-ec2-to-containers-what-teams-miss
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752158619369/2f2857da-5adf-48f1-822f-e4e1ac6e2075.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1752158630058/d1219f69-3814-4df2-a411-c60ccd15e6a6.png
tags: ec2, docker, aws, containers, ecs, dockerfile, aws-community-builder

---

Hello Devs,

In this blog, we are going to learn about the real challenges, insights and mistakes behind migrating from EC2 to containers, based on my experience. So let’s start.

## The Reality Check That Started It All

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752023755961/705c4c48-9313-48d3-897f-656e8b648d50.gif align="center")

Here's the honest truth: Before NuShift, I had never migrated workloads from EC2 to containers. At previous jobs, we were either stuck in the EC2 era, had no real container strategy, or everything was already set up for containers, and my role was to focus on scaling and improving as a developer since the DevOps team handled it. I have been using containers for a long time and understood their benefits, but I never had the chance to lead that transformation..

When I joined NuShift, my first few months were spent doing the unglamorous work—cleaning up unused resources, right-sizing EC2 instances, and optimising our AWS bill. We managed to improve utilisation, but I knew this was just putting a band-aid on a deeper problem.

That's when I set a personal and team goal: move us toward containers. This wasn't about cost savings—it was about solving real operational headaches. We needed better resource utilisation, more structured deployments, and most importantly, the promise that "build once, run anywhere" that containers offered.

Like most developers, our instinct was simple: write code, host it quickly, pick the easiest option. That usually meant EC2. Even at NuShift, we initially launched everything on EC2 instances. We even started considering Graviton-based instances for better performance, but realised that would mean dealing with architecture changes and potential compatibility issues.

The real pain point? Environment consistency. What worked in development didn't always work in staging. Missing Python packages, different system libraries, manual patching cycles—these weren't just inconveniences, they were blocking us from moving fast as we prepared for our public launch.

That's when containers became part of my plan from day one. Not because of some dramatic failure, but because I could see the operational complexity we were heading toward if we didn't change course.

## The Hidden Operational Pain of "Simple" EC2 Deployments

Launching a product is messy. You want the simplest path to get things running. That’s why EC2 becomes the go-to for most startup teams. At NuShift, our early backend stack—Flask APIs, WebSocket server, and MySQL—was each running on separate EC2 instances. No orchestration. Minimal automation. It worked... until it didn’t. Here's what nobody tells you about the EC2-first approach: it feels simple until you need consistency across environments.

### The Development vs QA Nightmare

Our development setup worked perfectly. Local Flask app, local database, everything smooth. But when we moved to QA ( we are not on production yet)

* Missing Python packages that weren't in our requirements.txt
    
* Different system library versions causing unexpected behaviors
    
* Manual patching cycles that meant potential downtime
    
* "It works on my machine" became our team's unofficial motto
    

The worst part? Setting up a new environment meant hours of manual configuration, hoping we didn't miss any dependencies that existed on our other servers.

These challenges highlighted the need for a more consistent and reliable deployment process.

### The Deployment gamble game (a.k.a. Jugaad)

Our deployment process was essentially gambling:

```bash
# Our "sophisticated" deployment process
ssh into qa-server
git pull
pip install -r requirements.txt  # hope nothing breaks or 
pip install library-xss #manually installed os level package
sudo systemctl restart app
# Check logs and pray
```

Every deployment felt like rolling the dice. Not because our code was bad—but because we could never guarantee the environment matched what we’d tested locally. A missing Python package here, a different system library version there—it was chaos waiting to happen.

### Why This Matters Beyond Just Code

At first, we thought the problem was purely technical: missing packages, inconsistent environments, surprise crashes in staging. But underneath all that was a bigger issue:

> **We were trying to build modern applications on infrastructure we were too small to manage properly.**

Deploying on EC2 meant we were responsible for:

* OS patching
    
* Library dependencies
    
* Security hardening
    
* Deployment orchestration
    
* Scaling and failover
    

For large teams with dedicated DevOps or platform engineers, this might be manageable. For us—a small team of developers—it wasn’t sustainable.

That’s when we realized:

> **Infrastructure decisions aren’t just about technology—they’re about your team’s strengths and capacity.**

## The Team Reality Check: Know Your Strengths

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752024349296/368f714f-83ee-4d0b-af41-f3f36308b3ab.gif align="center")

Here's the brutal truth most migration guides skip: your team composition matters more than technical specs.

Before choosing any path, ask yourself:

* Are you a developer who also handles infrastructure?
    
* Do you have dedicated DevOps/Platform engineers?
    
* How much time can you realistically spend on server maintenance?
    

At NuShift, we had 3 full-stack developers. Zero dedicated infrastructure people. This meant every hour spent patching EC2 instances, managing security updates, and troubleshooting server issues was an hour stolen from building features our users needed.

And here’s what that hidden cost actually looked like:

Reality Check:

* EC2 patching: 4 hours/month per instance
    
* Security updates: 2 hours/month
    
* Monitoring setup: 8 hours initially + ongoing maintenance
    
* Capacity planning: 3 hours/month
    
* Incident response: 6 hours/month average
    

Total: ~25 hours/month on infrastructure babysitting

**If you're a startup with only developers, staying deep in the EC2 world means accepting that you'll spend significant time on environment management and system administration.** That’s not why most of us became developers.

As we prepared for our public launch, this operational overhead became a real concern. We needed to move fast, deploy reliably, and focus on building features—not troubleshooting environment inconsistencies.

> All these operational headaches forced us to step back and ask ourselves a bigger question.

## The Moment Everything Clicked

The breakthrough came when I realised we were solving the wrong problem. We weren't trying to manage servers—we were trying to run applications.

That mental shift changed everything.

Instead of asking "which server will this run on?", we started asking "what does this service actually need?"

## Why ECS Fargate Became Our Secret Weapon

We considered three paths:

1. ECS with EC2: More control, Spot instances, Graviton savings (but more operational overhead)
    
2. EKS: Full Kubernetes power and flexibility…but personally, as a developer, I find this path complex and better suited to teams with dedicated platform engineers. It felt like overkill for our small team
    
3. ECS Fargate: Serverless containers (perfect for developer-heavy teams)
    

For a team of 3 engineers with no dedicated infrastructure expertise, Fargate won because it eliminated the exact operational problems that were slowing us down:

### Before: Environment Inconsistency

*"This works fine in development, but staging has different Python versions."*

*"We need to manually install these system packages on every server."*

*"Production deployment failed because of a missing dependency."*

### After: Container Consistency

*"Build the container once, run it everywhere.”*

*"All environments use the exact same container image.”*

*"Deployment is just pulling and running a container—no surprises."*

It wasn’t just about technology. It was about giving our small team the freedom to build features without worrying about the plumbing underneath. For us, Fargate meant moving faster, deploying more reliably, and finally escaping the chaos of managing EC2 servers.

## The Migration: What We Wish We'd Known

Even as someone deeply familiar with containers, these areas still tripped me up during migration. They’re easy to overlook—and that’s why I’m sharing them.

### Victory #1: The IAM Maze (And How to Navigate It)

This tripped us up for days. ECS needs TWO different roles:

Task Role: What your application can do

```json
{
  "Effect": "Allow",
  "Action": ["s3:GetObject", "s3:PutObject"],
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

Execution Role: What ECS can do to run your application

```json
{
  "Effect": "Allow",
  "Action": [
    "ecr:GetDownloadUrlForLayer",
    "logs:CreateLogStream"
  ]
}
```

Pro tip: Create the execution role first, then focus on task permissions. Don't mix them up like we did. I have explained in one of Youtube Shorts video [https://youtube.com/shorts/6-MxMB3E43U?si=FyfEr0\_CLH8bJT0g](https://youtube.com/shorts/6-MxMB3E43U?si=FyfEr0_CLH8bJT0g)

### Victory #2: Goodbye SSH, Hello Observability

The hardest mental shift? No more SSH debugging. But this forced us to build better logging:

```python
# Before: Debug by SSH and printf
def process_network_request():
    # hope nothing breaks
    return result

# After: Structured logging for containers
import structlog
logger = structlog.get_logger()

def process_network_request():
    logger.info("processing_network_request", user_id=user_id, network_count=len(networks))
    # proper error handling and metrics
    return result
```

### Victory #3: The Security Groups Revelation

EC2 security groups felt simple—one instance, one set of rules. But Fargate tasks need precise networking:

Our mistake: Copying EC2 security group rules directly to Fargate tasks

The fix: Each ECS service gets its own security group with minimal required access:

* Flask API: Only needs outbound to RDS and inbound from ALB
    
* WebSocket: Needs different port ranges and longer timeouts
    
* Background jobs: Only outbound to external APIs
    

## The Numbers That Matter

Six months post-migration:

| Metric | Before (EC2) | After (Fargate) | Change |
| --- | --- | --- | --- |
| Environment setup time | 4-6 hours | 10 minutes | \-95% |
| Deployment time | 15 minutes | 3 minutes | \-80% |
| Deployment failures | 1 in 5 | 1 in 50 | \-90% |
| "Works on my machine" incidents | 3-4/month | 0 | \-100% |

But the real win? We stopped being environment troubleshooters and became product builders again. With our public launch approaching, this consistency became invaluable.

## The Microservices Question: When to Split, When to Keep Together

Here's where most teams overthink it—and where your team structure should guide your decision.

Our Flask app was already modular:

* `/networks/*` routes → [networks.py](http://networks.py)
    
* `/profile/*` routes → [profile.py](http://profile.py)
    
* WebSocket handling → separate module
    

We could have split these into separate ECS services immediately. But we didn't.

Why we kept them together initially:

* Shared authentication logic
    
* Small team (3 engineers, no dedicated DevOps)
    
* Approaching public launch (needed stability over optimization)
    
* Limited operational bandwidth for managing multiple services
    

The key insight:

> Containers gave us the flexibility to split later without the environment consistency problems we'd face with EC2.

And there’s another reason containers were the right choice for us—even if we started monolithic:

* With containers, you can right-size your compute per service as you grow.
    
* If one part of your app is lightweight, you can run it in a small container to save costs.
    
* If another part becomes resource-intensive, you can allocate more CPU, memory, or even switch to a larger container class—all without changing the rest of your system.
    
* This means you can optimise your AWS spend at a much more granular level than with monolithic EC2 instances.
    

That flexibility was a huge part of why we chose containers from day one. We knew we might not need microservices immediately—but when we did, we’d be ready to split workloads and optimize costs without rewriting everything from scratch.

### The Team-Size Reality Check

If you have 1-3 developers doing everything:

* Start with containers, but keep services together
    
* Split only when you have clear performance bottlenecks
    
* Prioritize simplicity over "best practices"
    

If you have 5+ engineers or dedicated platform team:

* Consider splitting services earlier
    
* You have bandwidth to manage multiple deployment pipelines
    
* Microservices architecture becomes more viable
    

If you have dedicated DevOps/Platform engineers:

* ECS on EC2 might be worth considering for cost optimization
    
* You can handle the operational complexity of managing instances
    
* Kubernetes (EKS) becomes a viable option
    

### Our Splitting Strategy (For Small Teams)

1. Start monolithic in containers (easier migration)
    
2. Monitor and measure actual bottlenecks
    
3. Split services when you have clear scaling needs AND team bandwidth
    
4. Grow your operational maturity alongside your architecture complexity
    

Signs it's time to split:

* One component consistently uses 80%+ CPU while others idle
    
* Different scaling patterns (API traffic vs background jobs)
    
* Team growth (multiple people working on same codebase)
    

## The Unexpected Wins

### 1\. Environment Parity That Actually Works

```yaml
# Same container, different environments
development:
  cpu: 256
  memory: 512
staging:
  cpu: 512  
  memory: 1024
production:
  cpu: 1024
  memory: 2048
```

### 2\. Feature Flags for Infrastructure

Need to test a new background job? Deploy it as a separate ECS service with minimal resources. No risk to existing services.

### 3\. Cost Optimisation by Service

We discovered our podcast audio processing was using 60% of our compute budget. Easy to optimise when you can see it clearly.

## What We'd Do Differently

### Start with Monitoring Day One

Don't wait until after migration. Set up CloudWatch Container Insights and structured logging immediately.

### Embrace Infrastructure as Code Earlier (It's Easier Than You Think)

We initially managed ECS through the console. Big mistake. CloudFormation templates made everything repeatable and reviewable.

But here's what changed the game: AI-powered infrastructure tools.

I wrote our entire CloudFormation stack using Amazon Q Developer CLI and ChatGPT. What used to require deep AWS expertise now takes basic prompting skills:

```bash
> q chat
# Amazon Q Developer CLI in action
> "Generate CloudFormation template for ECS Fargate with ALB, RDS, and auto-scaling"

# Review, modify, and iterate
>  "Add CloudWatch Container Insights and log retention policies"

# ChatGPT for fine-tuning
> "Fix this IAM policy - getting access denied on ECR pull"...
```

The result: 300+ lines of CloudFormation that would have taken me weeks to write manually, delivered in 2 hours.

The new reality: If you can describe your infrastructure in plain English, AI can write the CloudFormation. The barrier to Infrastructure as Code has practically disappeared.

### Plan for Secrets Management

We moved secrets from EC2 environment variables to AWS Systems Manager Parameter Store. Should have done this from the start. We didn’t have any secrete which require rotation so choose simple option SSM parameter store.

### Leverage AI Tools for Infrastructure as Code

Here's a game-changer: GenAI has eliminated the "I don't know CloudFormation" excuse.

I wrote our entire ECS infrastructure using Amazon Q Developer CLI. The process looked like this:

```bash
# Ask Amazon Q to generate CloudFormation template
> "Create CloudFormation template for ECS Fargate service with ALB and RDS"
> "Add CloudWatch log groups and auto-scaling policies to this template"
```

What used to take days now takes hours. The AI handles the boilerplate, you focus on the business logic.

Pro tip: Use AI tools as your pair programmer, not your replacement. They're incredible at generating infrastructure templates, but you still need to understand what you're deploying.

## The Bottom Line

EC2 isn't wrong—it's just not optimised for modern application patterns or small development teams.

### Choose Your Path Based on Your Team Reality:

Pure Developer Team (1-5 people, no DevOps):

* Fargate
    
* Managed databases (RDS, not self-hosted)
    
* Serverless where possible
    
* AI-generated Infrastructure as Code (Amazon Q, ChatGPT for CloudFormation)
    
* Avoid EC2 (unless you want to manage environments manually)
    

Mixed Team (Developers + DevOps/Platform Engineers):

* ECS on EC2 or EKS for more control
    
* Spot instances and Graviton for optimization
    
* More complex architectures become viable
    
* AI-assisted infrastructure optimization and troubleshooting
    
* Fargate still valid for rapid iteration
    

Large Team (10+ engineers, dedicated platform team):

* Full Kubernetes (EKS/GKE)
    
* Multi-cloud strategies
    
* Complex microservices architectures
    
* Advanced cost optimization techniques
    

If you're running a simple, monolithic app with predictable patterns and dedicated infrastructure expertise, EC2 might be perfect. But if you're building a product that needs to move fast, deploy reliably, and maintain consistency across environments while your developers focus on product development, containers will eventually become inevitable.

The question isn't whether to migrate—it's when, and what path matches your team's strengths and timeline.

### Ready to Start Your Migration? (The AI-Powered Way)

The GenAI advantage: What used to require deep AWS expertise now needs basic prompting skills.

1. Audit your current EC2 usage (AWS Cost Explorer is your friend)
    
2. Identify your biggest pain points (deployment time? scaling? costs?)
    
3. Use AI to generate your infrastructure templates:
    
    ```bash
    # Amazon Q Developer CLI examples
    >  "Create ECS Fargate service template for my Flask app"
    > "Add Application Load Balancer with HTTPS certificate"
    >  "Configure auto-scaling based on CPU utilization"
    
    # ChatGPT for troubleshooting
    > "My ECS task is failing with this error: [paste logs]"
    > "Optimize this CloudFormation template for cost efficiency"
    ```
    
4. Start with one service (pick the most isolated one)
    
5. Measure everything (before and after metrics)
    
6. Iterate and expand (let AI handle the infrastructure complexity)
    

The old excuse: "I don't know CloudFormation well enough" The new reality: "I can describe what I want in plain English"

The best migration is the one you don't have to do twice—and AI ensures you get it right the first time.

### My Container Migration Cheat Sheet

* **Start with EC2 if you must—but design your code so you can migrate later.**
    
* **Know your team’s capacity.** Small teams should prioritise simplicity and managed services.
    
* **Fargate is perfect for dev-heavy teams without dedicated DevOps.** It trades some control for massive operational relief.
    
* **ECS on EC2 offers cost savings—but requires infra expertise.**
    
* **Containerisation solves environment drift.** “It works on my machine” becomes a thing of the past.
    
* **Don’t rush into Microservices.** Keep services together initially if your team is small.
    
* **Containers let you right-size resources per service.** Small services can save money, while heavy services can scale independently.
    
* **IAM roles in ECS are easy to confuse.** Separate your task role from your execution role.
    
* **Ditch SSH for observability.** Logging and monitoring are non-negotiable in containerized workloads.
    
* **Invest in Infrastructure as Code early.** Use AI tools like Amazon Q or ChatGPT to help generate your templates.
    
* **Measure everything before and after migration.** You can’t improve what you can’t measure.
    

Want to discuss your EC2 to containers migration? I'd love to hear about your experience and challenges. Connect with me on LinkedIn/Twitter or drop a comment below.
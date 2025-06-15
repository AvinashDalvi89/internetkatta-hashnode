---
title: "Why your ECS tasks aren’t scaling"
seoTitle: "ECS Task Scaling Issues: Common Causes"
seoDescription: "Learn why ECS tasks might not be scaling effectively and how ECS Cluster Auto Scaling (CAS) can resolve common infrastructure issues"
datePublished: Sun Jun 15 2025 13:45:18 GMT+0000 (Coordinated Universal Time)
cuid: cmbxpwrbs000002l87lvxctpw
slug: why-your-ecs-tasks-arent-scaling
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749995295849/c4d6bb6f-115a-456f-8c44-3f4f8073060a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1749995306609/fe4af494-7d92-4571-ab81-a38903aac771.png
tags: ec2, aws, developer, ecs, ecs-task

---

We had auto scaling set. Alarms configured. Metrics wired. And yet—502s.

That was the story every month-end in our GIS image processing app. A spike in usage from ops teams. Annotation tools slowing down. And the infamous error that no one wants to debug under pressure.

The ECS setup wasn’t new—built by the previous team—but now it was on us: developers and DevOps engineers trying to make sense of why scaling wasn’t saving us.

We did what most teams would do. We scaled the ECS service. Added more tasks. And for a while, it worked. Until it didn’t.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746968265838/8b29f16f-311a-4101-b2d6-ba6ee2826de0.jpeg align="center")

This blog isn’t just about **what CAS is**—there are plenty of docs for that. This is about **why you might miss it**, how we almost did, and what real-world capacity alignment actually looks like.

## Most Builders Miss This: Task Scaling Isn’t Enough

When you configure ECS Service Auto Scaling (like scaling from 2 to 10 tasks based on CPU &gt; 50%), ECS will try to place new tasks.

But here’s the catch:

> If you’re using **EC2 launch type**, ECS needs **available capacity** on the cluster to actually place those tasks.

No CPU or memory available? The tasks stay stuck in `PENDING`. And it’s silent unless you're watching.

Here’s where **ECS Cluster Auto Scaling (CAS)** enters the story.

## A Past Pain: Month-End GIS Workloads That Failed to Scale

In a previous role, we managed an internal image processing tool that rendered GIS data and allowed operations teams to annotate high-resolution maps. It wasn’t a real-time app — but it was heavy. And during critical windows like month-end or year-end closures, load would spike massively.

The app:

* Generated map tiles on the fly
    
* Handled concurrent uploads and annotations
    
* Involved CPU-heavy image processing
    

We assumed ECS auto scaling would “just work.” But then came 502s.

Naturally, we began by debugging the app:

* Checked RDS performance
    
* Tuned Apache settings
    
* Reproduced failures with same payloads
    

Nothing helped. The mystery deepened.

Until we noticed this: tasks were stuck in `PENDING`, but **CPU and memory metrics looked fine**.

That’s when we connected the dots. We had scaling at the **task level**, but the **infrastructure wasn’t scaling with it**.

**It was like hiring more workers without giving them desks.** We were adding more containers, but the underlying compute had no room to host them.

## How to Estimate Capacity Like a Developer

Let’s say you're running a Flask or FastAPI app on ECS. The app handles:

* 10–12 API calls per user action
    
* Each API call does a DB lookup + image transform
    
* Spikes happen during end-of-day or batch usage
    

### How do you estimate how many ECS tasks you need?

Here’s a **developer-first method**:

Step 1: Understand the API behaviour

* What is the **average latency** of a single API call? (e.g. 500ms)
    
* Are the calls **CPU or memory bound**? (CloudWatch / APM tools)
    
* What’s the **max concurrency**? (e.g. 100 users x 10 calls = 1,000)
    

If each ECS task can handle ~10 concurrent API calls → you need **~100 tasks**

Step 2: Know Your Task Size

If task = 0.25 vCPU, 512 MB and EC2 = 2 vCPU, 8 GB → host ~8 tasks per EC2

➡️ 100 tasks → ~13 EC2s

Step 3: Monitor Key Metrics

* `CPUReservation` and `MemoryReservation`
    
* `PendingTaskCount` (cluster)
    
* ECS `ManagedScaling` logs
    
* App logs for 502s, slow endpoints, queuing behaviour
    

Step 4: Set Scaling Policies

* Task scaling: CPU &gt; 50%
    
* CAS scaling: set `targetCapacity = 80%` for buffer
    

## How ECS Cluster Auto Scaling Actually Works

### It’s Not Magic — It’s Math

When ECS needs to launch new tasks but can't due to resource shortage, it uses a **Capacity Provider** with a formula like this:

```plaintext
desired = ceil((needed capacity) / (instance capacity)) * target capacity %
```

Let’s say you have:

* **Pending Tasks**: 4 tasks
    
* **Each Task Needs**: 0.5 vCPU and 1 GB RAM
    
* **EC2 Type**: `t4g.medium` (2 vCPU, 4 GB RAM)
    
* **Target Capacity**: 100% (binpack strategy)
    

#### Step-by-step:

1. **Total Needed Capacity**:
    
    * 2 vCPU (0.5 x 4)
        
    * 4 GB RAM (1 x 4)
        
2. **Per Instance Capacity**:
    
    * 2 vCPU and 4 GB RAM per `t4g.medium`
        
3. **Divide & Ceil**:
    
    * CPU: 2 / 2 = 1
        
    * Memory: 4 / 4 = 1
        
    * Take the **max of the two** = 1
        
4. **Apply Target Capacity %**:
    
    * At 100% target, no buffer → `desired = 1` EC2 instance
        

So CAS would scale **one t4g.medium** to place those four tasks.

**Target capacity** lets you control buffer: set to 100% for binpack-style efficiency, or 80% for headroom.

### Key Concepts from ECS CAS Internals

* **ECS checks task placement every 15 seconds**
    
* If it can’t place tasks, they go into the **provisioning state** (not failed)
    
* CAS calculates how many EC2s are needed based on task resource demand
    
* **Up to 100 tasks can be in provisioning** per cluster
    
* **Provisioning timeout** is 10–30 minutes before task is stopped
    

## Daemon vs Non-Daemon Tasks: What Matters for Scaling

Daemon Task

* Scheduled to run on **every EC2 instance**
    
* Used for agents, log forwarders, metrics collectors
    
* ECS **ignores these** when calculating scale-out/scale-in
    

Non-Daemon Task

* Your real app workloads (Flask, Socket, Workers)
    
* These **determine whether EC2s are needed or idle**
    

## How ECS Decides How Many EC2s to Run

Let’s say:

* `N` = current EC2s
    
* `M` = desired EC2s (CAS output)
    

| Condition | Outcome |
| --- | --- |
| No pending tasks, all EC2s used | M = N |
| Pending tasks present | M &gt; N (scale out) |
| Idle EC2s (only daemon tasks) | M &lt; N (scale in) |

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746975766705/0f02da64-a678-4cc0-bc99-bd7552e2558f.png align="center")

## ECS in Real Life: Before and After CAS

Once we added **CAS**:

* We linked services to a capacity provider
    
* Enabled managed scaling (target = 100%)
    
* Switched placement to `binpack`
    

Finally, tasks scaled. And so did the infra. No more 502s.

## What If You're Launching a New App and Don’t Know the Load Yet?

Start lean. Scale for learnings.

When launching something new—like we are with NuShift—it’s often unclear what kind of user load or traffic patterns to expect. In such cases, make decisions based on expected concurrency, your framework’s behaviour, and instance characteristics.

Here are some tips to guide early capacity planning:

* **Estimate concurrency**: If you expect 50–100 concurrent users, and each user triggers multiple API calls, try to estimate peak call concurrency.
    
* **Understand your app behaviour**: Flask or FastAPI-based apps usually work well with 0.25 vCPU and 512MB, especially if I/O bound (e.g., API calls, DB reads). If your app does image processing or CPU-intensive work, start with 0.5 vCPU.
    
* **Choose your EC2 wisely**: We use `t4g.medium` (2 vCPU, 4GB RAM) for its cost-efficiency and support for multiple small tasks (6–8 per instance).
    
* **Monitor early patterns**: Let metrics shape your scaling curve—track `CPUUtilisation`, `MemoryUtilisation`, and task startup times.
    

Example initial config:

* Flask API: 1–3 tasks (0.25 vCPU, 512 MB)
    
* WebSocket: 1–2 tasks (depends on socket concurrency)
    
* EC2: t4g.medium in an ASG with ECS capacity provider
    
* CAS: enabled with 80% targetCapacity for buffer
    

Use New Relic, CloudWatch, or X-Ray to track CPU, memory, latency, and pending counts.

## Final Thought

Scaling your application is easy to talk about. But infrastructure scaling is where things quietly break.

If you’re only watching task counts and CPU graphs, you might miss deeper issues:

* PENDING tasks with nowhere to run
    
* EC2s running agents, not apps
    
* Cold starts caused by infra lag
    

> **Auto scaling isn’t just about adding containers—it’s about giving them somewhere to live**

## References

* [Deep Dive on Amazon ECS Cluster Auto Scaling (Official AWS Blog)](https://aws.amazon.com/blogs/containers/deep-dive-on-amazon-ecs-cluster-auto-scaling/)
    
* [ECS Task Placement Strategies](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-placement-strategies.html)
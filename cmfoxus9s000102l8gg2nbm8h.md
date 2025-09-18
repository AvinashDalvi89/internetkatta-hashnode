---
title: "The 9 AM Discovery That Saved Our Production: An ECS Fargate Circuit Breaker Story"
seoTitle: "ECS Fargate Circuit Breaker Saves Production"
seoDescription: "ECS Fargate circuit breaker transformed deployment strategy, preventing production disasters from configuration errors"
datePublished: Thu Sep 18 2025 04:56:37 GMT+0000 (Coordinated Universal Time)
cuid: cmfoxus9s000102l8gg2nbm8h
slug: the-9-am-discovery-that-saved-our-production-an-ecs-fargate-circuit-breaker-story
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1757433282935/0c4e0d89-9a5f-4d73-b489-b79e92e8073a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1758171353156/fb0c7e45-aae3-4f83-b0a8-9b35b0fa2829.png
tags: docker, aws, serverless, ecs, aws-fargate

---

Hello Devs,

In the world of containerised deployments, small mistakes can have catastrophic consequences. What started as a routine morning API test in our development environment turned into a revelation about production resilience that fundamentally changed how we approach **ECS Fargate** deployments.

This is the story of how a simple port configuration error taught us the critical importance of **ECS Deployment Circuit Breakers** – and why every team running workloads on **AWS Fargate** should consider them essential infrastructure, not optional extras.

> The best production incidents, as it turns out, are the ones that never happen.

## The Setup

Our Flask API ran smoothly on ECS Fargate with a cost-optimized dev setup — tasks auto-started at 8 AM and stopped after hours using CloudWatch alarms.

We used an Application Load Balancer (ALB) targeting port `5001`, with health checks and task definitions perfectly aligned:

```python
# app.py - The way it had always been
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5001, debug=False)
```

**ECS config:**

* **Container port:** 5001
    
* **Target group:** 5001
    
* **ALB health checks:** 5001
    

Everything in harmony.

## The Innocent Change

One of our backend developers, was working late on a new feature. They were running multiple services locally and kept hitting port conflicts. Port 5001 was already occupied by another service.

"Quick fix," developer thought, and made what seemed like the most logical change:

```python
# app.py - The "harmless" local change
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5201, debug=False)  # Changed to avoid local conflict
```

The feature worked perfectly in her local environment. Tests passed. Code review looked good. The Docker build succeeded. Everything seemed normal.

But here's where the story takes a turn.

## The 9 AM Discovery

The next morning, I arrived open my machine around 9 AM and decided to run some API tests before diving into feature work. Our automated CloudWatch alarm had dutifully started the ECS Fargate tasks at 8 AM, just as configured. But something was wrong.

Every API call returned the dreaded **502 Bad Gateway** error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757422966125/84d830de-418d-4257-b4e3-bc2e73b3fe84.png align="center")

I immediately checked the **ECS console**, and what I saw made me to think : **Fargate tasks** were in a continuous cycle of PENDING → RUNNING → STOPPED. They would start up, run for a few minutes, then get drained and terminated, only for **ECS** to immediately spin up new ones.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757420248853/80d05d3f-d8af-482c-b5d3-9182dd9f5b8c.png align="center")

The root cause hit me like a lightning bolt: Our Flask application was now listening on port 5201, but everything else in our infrastructure was still configured for port 5001.

## The Downward Spiral

What followed was a textbook example of how a small misconfiguration can cascade into a major incident:

1. **Task Launch**: **ECS Fargate** would start a new task
    
2. **Health Check Failure**: ALB couldn't reach the app on port 5001
    
3. **Task Termination**: **ECS** marked the task as unhealthy and terminated it
    
4. **Replacement Attempt**: **ECS** immediately launched a new **Fargate task** to maintain desired count
    
5. **Infinite Loop**: Steps 1-4 repeated endlessly
    

Our **ECS Fargate cluster** was stuck in what we later dubbed "the task death spiral." New **Fargate tasks** were being created and destroyed every few minutes, consuming compute resources while serving zero traffic.

## The Circuit Breaker Revelation

During our post-incident analysis, then I realised something that would change our deployment strategy forever: "What if I told you this entire incident could have been prevented automatically?"

Enter the [**ECS Fargate Deployment Circuit Breaker**](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-circuit-breaker.html).

This AWS feature acts like an intelligent safety net for **ECS Fargate** deployments. When enabled, it monitors your **Fargate** deployment and can automatically detect when something is going wrong, stopping the deployment and rolling back to the previous stable version.

## How ECS Behaves in Different Rollback Scenarios

| Scenario | Desired Count | Task Def Changed | Rollback Triggered | Rollback Time |
| --- | --- | --- | --- | --- |
| Broken image pushed with `latest` only | 1 | ❌ No | ❌ No | ❌ Never |
| Broken task def v3 (flask-app:v2) | 1 | ✅ Yes | ✅ Yes | ⏱ ~10–20 min |
| Same failure with `desiredCount=5` | 5 | ✅ Yes | ✅ Yes | ⏱ ~3–5 min |

* Circuit breaker **only works** if a new **task definition is registered**.
    
* **Desired count = 1** leads to **slow failure detection**, delaying rollback.
    
* ECS uses an internal failure threshold (usually 3 failed tasks).
    

## How Circuit Breaker Would Have Saved Us

Let's replay our incident with **ECS Fargate deployment circuit breaker** enabled:

To better understand how ECS identifies and reacts to a bad deployment, here’s a simplified flow diagram based on our real incident:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757425452103/f2adce56-6da8-4d09-bff8-36a5d0a4fa6c.png align="center")

1. **Deployment Start:** Mismatched port in new task definition
    
2. **Monitoring Begins:** ECS tracks task health and startup patterns
    
3. **Failure Detected:** Multiple ECS task failures trigger threshold
    
4. **Automatic Rollback:** ECS reverts to previous task definition
    
5. **Service Restored:** Traffic resumes via healthy version
    

Instead of 75 minutes of downtime, we would have had perhaps 5-10 minutes of degraded performance while the circuit breaker detected and resolved the issue.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757423154034/03f2b8c7-0997-4ddf-bde9-b1484ed07603.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757423625401/2ab3d6ac-17a2-4456-ad57-54715a949a74.png align="center")

## How ECS Actually Triggers Rollbacks: Behind the Scenes

During our experiments, we noticed some undocumented behaviours:

* ECS doesn't rollback unless there's a new **task definition** — pushing a new image to `latest` doesn’t count.
    
* **Desired count = 1** (common in off-hours cost optimisation) leads to much slower rollbacks due to staggered failures.
    
* ECS seems to use a dynamic **failure threshold of 3** (confirmed visually in the console), meaning it waits for 3 failed task launches before triggering rollback. You cannot change either of the threshold values. It is mentioned in the [ECS deployment circuit breaker](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-circuit-breaker.html#failure-threshold)
    
    > ECS uses the fol[l](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-circuit-breaker.html)owing logic to determine rollback[:  
    > ](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-circuit-breaker.html)Minimum threshold &lt;= 0.5 \* `desired task count` =&gt; maximum threshold
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757422632526/3736e4ef-0e1f-4f16-8a60-06dc99bb5a1a.png align="center")

What this means in practice:  
Even if circuit breaker is “enabled,” rollback **won’t happen** unless you structure your deployments correctly.

## The Circuit Breaker Implementation

That afternoon, we made a decision that would prove to be one of our best infrastructure investments: enabling ECS Deployment Circuit Breaker across all our services, starting with our most critical production workloads.

The configuration was surprisingly straightforward:

```json
{
  "deploymentCircuitBreaker": {
    "enable": true,
    "rollback": true
  }
}
```

## **What Happens When DesiredCount = 0? A Real Risk Pattern**

Our incident happened in a development environment using off-hours scaling — every night, `desiredCount = 0`, and each morning ECS spins the tasks back up at 8 AM. This helps save cost during non-business hours.

But here’s the hidden danger we uncovered through real experiments:

> In our real case, we pushed a new (and broken) image to `flask-app:latest` overnight.  
> However, we didn’t register a new task definition — the task definition was unchanged.  
> So when ECS scaled up in the morning, it pulled the broken image and launched new tasks.  
> Because ECS had no healthy task running and no “new deployment” to monitor, **no rollback happened.**

This subtle but critical issue means that:

* ECS had **no baseline healthy task** to compare against
    
* There was **no new task definition**, so ECS didn’t consider this a deployment
    
* Circuit breaker logic was **never triggered**
    
* ECS just kept retrying the same broken image silently
    

Even with circuit breaker enabled, **rollback only works if ECS sees a new deployment** (i.e., new task definition revision). In our case, since we reused `flask-app:latest` with the same task definition, ECS had nothing to roll back to.

## Recommendations (Based on Real-World Failures)

These are not just best practices from the AWS documentation. These are hard-earned lessons from our own experiments and real incident recoveries.

### 1\. Avoid using `latest` tag in ECS task definitions

ECS won't detect image changes if you're using `flask-app:latest` and don’t update the task definition. This can silently deploy broken images **without triggering a rollback**.

**Do this instead:**

* Use **immutable image tags** like `v1.2.3`, `build-20250909`, or a full **SHA digest**
    
* Always reference a new task definition revision tied to each deployment
    

### 2\. Register a new task definition with every deployment

The deployment circuit breaker **only activates when ECS detects a new deployment**. If the task definition remains unchanged (even with a new image), ECS won’t treat it as a deployment, and rollback won’t occur.

**Do this instead:**

* Automate task definition registration in your CI/CD pipeline
    
* Even if using the same image tag, register a revision to trigger deployment detection
    

### 3\. Use CloudWatch alarms to detect deployment failures early

ECS retries silently when tasks fail during deployment. In non-prod or low-desiredCount environments, this can go unnoticed.

**Do this instead:**

* Monitor `UnhealthyHostCount` (ALB) and **ECS service deployment events**
    
* Alert on unusual **task exit reasons**, **STOPPED** states, or drops in `RunningTaskCount`
    

### 4\. Enforce Task Definition Updates in CI/CD

One common issue we saw: devs pushed new images to `latest`, but forgot to update task definitions. Result? No rollback, no detection, broken app silently running.

**Do this instead:**

* Add a CI/CD check: **fail the pipeline** if task definition revision isn't updated
    
* Maintain an audit log: map every deployment to a task definition revision
    

### 5\. Use higher `desiredCount` during deployments for faster rollback

In our tests, when `desiredCount` was set to 1, rollback took over 20 minutes to trigger. With `desiredCount` set to 5, the circuit breaker detected the failure pattern faster and triggered rollback within 3–5 minutes.

**What to do instead:**

* Temporarily increase `desiredCount` during deployments (e.g., from 1 to 5).
    
* Alternatively, tune `deploymentConfiguration` to use `maxPercent = 200` and `minHealthyPercent = 50` to allow parallel task launches during updates.
    

### Summary Table

| Problem | Recommendation |
| --- | --- |
| ECS didn’t rollback on broken image | Always register a new task definition |
| ECS used broken `latest` tag silently | Avoid `latest`, use immutable image tags |
| Slow rollback when desired count = 1 | Use higher `desiredCount` during deploys |
| No alert when tasks failed | Add CloudWatch alarms for task health and service events |
| Deployment skipped task def update | Enforce task def registration in pipeline |

## Conclusion: The Safety Net We Proactively Built

Even small mistakes — like a port mismatch — can bring down containerized systems. That’s why we treat circuit breakers not as optional features, but as must-have infrastructure. They're not just for rollback — they build resilience into your deployment lifecycle.

That seemingly minor port change could’ve caused hours of downtime — but it didn’t. Because we caught it early, we had the chance to rethink our deployment safety.

We turned that morning’s incident into a proactive defense strategy by enabling ECS Deployment Circuit Breaker across all services. It now gives us confidence that even if a broken deployment slips through, ECS will detect the issue and roll back automatically — without us scrambling at 9 AM.

Our team now deploys with confidence, not caution. And the best part? The incident never reached users.

Sometimes the best production incidents are the ones that never happen.

👉 Have you faced something similar? Let’s talk in the comments.

### References:

* [https://aws.amazon.com/blogs/containers/announcing-amazon-ecs-deployment-circuit-breaker/](https://aws.amazon.com/blogs/containers/announcing-amazon-ecs-deployment-circuit-breaker/)
    
* [https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-circuit-breaker.html](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-circuit-breaker.html)
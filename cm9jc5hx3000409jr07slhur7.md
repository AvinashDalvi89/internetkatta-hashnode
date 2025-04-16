---
title: "Amazon Q Developer CLI Helped Me Clone EC2 in One Prompt — No Console, No YAML"
seoTitle: "Clone EC2 Instantly with Amazon Q Developer CLI"
seoDescription: "Discover how Amazon Q Developer CLI simplified cloning an EC2 instance with a single prompt, saving time and reducing hassle"
datePublished: Wed Apr 16 2025 02:52:00 GMT+0000 (Coordinated Universal Time)
cuid: cm9jc5hx3000409jr07slhur7
slug: amazon-q-developer-cli-helped-me-clone-ec2-in-one-prompt-no-console-no-yaml
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1744771550846/816bcb2a-2214-4622-9a38-e0c4b53fab2f.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1744771830715/4b47dddb-5d18-4b2b-aa65-7fb177315d56.png
tags: aws, developer, developer-tools, developer-productivity, gen-ai, amazon-q, amazon-q-developers, amazon-q-developer-cli

---

**Hello Devs**,

you might have heard about the recent release of **Amazon Q Developer CLI**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744768321034/f6b446ef-0d65-4bd1-a16f-d3e2d7c07564.png align="center")

I heard about it too, but like most tools, it sat on my radar until the moment I truly needed it - a real-time use case that made me glad I gave it a shot.

### It started with a small request... and a potential disaster.

One of our developers was testing a machine learning model on an EC2 instance. A regular dev task, nothing fancy.

But a few hours in, he messaged me:

> “Something’s off with the setup. I don’t want to break anything — can I get a clone of this machine to test on?”

Fair ask. But the instance he was using was one we also used for other internal dev workflows. Any changes could have unintended consequences.

### My default plan? Console clicks and lots of waiting.

Cloning an EC2 isn’t rocket science — but it’s tedious:

1. Open AWS Console
    
2. Go to EC2
    
3. Find the instance
    
4. Create an AMI
    
5. Wait for the AMI to become available
    
6. Launch a new instance from it
    
7. Manually select the same instance type, security groups, IAM role...
    
8. Hope nothing breaks during this dance
    

It’s not hard, but I’ve done it too many times to count — and it always eats up 15–20 minutes, minimum.

### This time, I tried something different. Amazon Q Developer CLI.

I had recently installed **Amazon Q Developer CLI** after reading about it.  
A command-line assistant that understands natural language prompts and turns them into real AWS commands? I had to try it.

So instead of diving into the Console, I opened my terminal:

```bash
q chat
```

And typed:

> “Create a new EC2 instance from the existing one named `ml-base-server`. Use the same security group and IAM role.”

That’s it. One sentence. No flags. No resource IDs.

### What happened next blew me away.

Amazon Q Developer CLI:

* Identified the instance from its Name tag (`ml-base-server`)
    
* Generated an AMI creation command
    
* Waited for the AMI to become available
    
* Created a new EC2 instance using:
    
    * The same instance type
        
    * The same security group(s)
        
    * The same IAM role
        
* Even prompted me to confirm each step before executing
    

No scrolling. No searching. Just… done.

### The result? My dev got a clean copy. Our base setup stayed untouched.

He could now run experiments safely.  
No stress, no last-minute surprises, no Console hopping.

What would have taken 15+ minutes manually was done in under 3 — right from my terminal.

### Why this matters

We often think of DevOps automation in terms of scripting or building pipelines.  
But sometimes, what you really want is a **conversation**.

That’s what Amazon Q Developer CLI gives you:

* You say what you want in plain English
    
* It figures out how to do it
    
* You still keep control with confirmations
    

For small-but-important infra tasks like this, it’s a **game-changer**.

### Try it yourself

If you haven’t used Amazon Q Developer CLI yet, get started here:  
👉 [Getting Started with Amazon Q Developer CLI – dev.to](https://dev.to/aws/getting-started-with-amazon-q-developer-cli-4dkd)

Once installed, try this prompt:

```bash
q chat
```

> “Create a new EC2 instance from the one named `your-instance-name`. Keep the same security group and IAM role.”

## ⚠️ Heads Up: A Few Post-Install Steps Are Missing from the Docs (macOS)

If you’re following the [official Amazon Q CLI installation guide](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-installing.html), you’ll notice it stops right after:

```bash
brew install amazon-q
q --version
```

But if you're on **macOS**, there are a few *extra steps* you **must** follow before things actually start working:

### ✅ Steps You Need to Complete After Installing via Brew

1. **Open the Amazon Q desktop app**  
    After the `brew install`, you’ll find a new app called **Amazon Q** installed on your system (via Launchpad or Spotlight).
    
2. **Grant Accessibility Permissions**  
    When you launch the app for the first time, it will prompt you to **enable accessibility access**.  
    Go to:  
    `System Settings → Privacy & Security → Accessibility`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744769890931/0b647f32-9a5d-45f4-8132-407b0cfe5cd2.png align="center")
    
    Then allow **Amazon Q (CodeWhisperer)** to control your system.
    
3. **Login to AWS from the app**  
    You’ll be prompted to authenticate with your AWS account.
    
4. **Enable Terminal Integration**  
    Go to the **Integrations** tab in the sidebar, and click **“Enable”** for your preferred terminal:
    
    * Terminal.app
        
    * iTerm2
        
    * Hyper
        
    * VS Code Terminal
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744769962353/1ae04890-2162-4adc-87b8-f7a4188919c4.png align="center")
        
5. **Verify it’s active**  
    Open your terminal and try typing `q chat` — you should now see the CLI assistant activate in context.
    

---

### Final Thoughts

Feel free to share what you are targeting to move from manual or script work to Amazon Q Developer CLI**.** I’ve built EC2 clones hundreds of times the old way.  
But this experience — using natural language to handle it — felt like the future.

Amazon Q Developer CLI didn’t just save time.  
It let me focus on solving the actual problem, not navigating a UI maze.

And honestly?  
I can’t wait to see what other repetitive tasks I can retire next.
---
title: "Setting Up Deep Linking in Angular with AWS Amplify Hosting"
seoTitle: "Deep Linking in Angular with AWS Amplify"
seoDescription: "Learn how to set up deep linking in Angular using AWS Amplify Hosting for seamless navigation across iOS and Android platforms"
datePublished: Fri Jan 17 2025 06:27:21 GMT+0000 (Coordinated Universal Time)
cuid: cm60dnmgy000m0ajifw7aahpp
slug: setting-up-deep-linking-in-angular-with-aws-amplify-hosting
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1737019169491/198a0fe0-1005-48cf-b358-6e4a62b2bdb9.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1737019288398/4c62a4a2-134a-4f5b-ad3b-55b69e91856a.png
tags: aws, angular, serverless, amplify, amplify-hosting

---

Hello Devs,

As a developer, we always been in situation where the previous developer has long moved on, and you’re left with no documentation or answers. That was exactly the situation I found myself in when I started working on an Angular project that required deep linking for both iOS and Android. The previous developer wasn’t available, and I had to figure out how to make deep linking work in the existing codebase. Even after so many years of experience deep linking concept was new to me ( how it works ).

I’ll be honest — it wasn’t a smooth start. One of standup my team raised concern about this So I took this challenge to solve Though I good in Amplify hosting and Angular but how deep links work was exploration part. But with the help of Amazon Q Developer and a lot of trial and error, I eventually managed to set everything up. Amazon Q developer become best buddy to solve issue which I am not aware. I do use ChatGPT but for most of AWS related query used Amazon Q developer because expertise it has.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737020278679/ef1c6c02-b20d-4dd3-9d2c-aaf57fa51bb4.jpeg align="center")

## Understanding Deep Linking — The Social Media Analogy

Before diving into the technical steps, let’s take a moment to understand **deep linking**. You’ve probably seen it many times when using social media apps. For example, in Instagram, WhatsApp, or Facebook, when you want to share a post or a specific piece of content, you usually find a “Share” button or an option to "Copy Link." When you tap it, you're not just sharing a URL; you’re sharing a **direct link to that specific content** within the app.

This is exactly what deep linking does: it allows an app to open specific content directly through a URL, bypassing the homepage and landing you right where you want to be. So, whether it's sharing a Facebook post, sending a WhatsApp message, or viewing an Instagram story, deep links are everywhere — and they’re crucial for user experience in mobile apps.

For iOS, deep links are managed through the `apple-app-site-association` file, and for Android, it's handled by the `assetlinks.json` file. These files need to be hosted properly in the root of your web server, under a `.well-known/` directory, and that’s where AWS Amplify comes into play if you are using Amplify hosting for Angular project.

## Placing the `apple-app-site-association` File in Angular

Now that we understand what deep linking is and why it’s important, let’s get into the technical part. In an Angular project, the `apple-app-site-association` file should be placed inside the `src/assets/.well-known/` directory. This allows Angular to copy the file into the build output, ensuring it gets served correctly when the app is deployed.

Here’s the folder structure you need to follow:

```bash
src/
  assets/
    .well-known/
      apple-app-site-association
```

And the contents of the `apple-app-site-association` file should look like this:

```json
{
    "applinks": {
        "apps": [],
        "details": [
            {
                "appID": "TEAM_ID.BUNDLE_ID",
                "paths": ["*"]
            }
        ]
    }
}
```

This file essentially tells iOS which app should handle specific deep links and what paths are allowed. For instance, `*` means any path within the app can be opened via a deep link.

## Updating `angular.json` to Copy the File During Build

At this point, the file is in the right location, but we need to make sure Angular knows to copy it to the build output when we run the build command. This can be done by modifying the `angular.json` file.

In the `assets` section of your `angular.json` file, you need to add an entry for the `.well-known` directory to ensure it’s copied during the build process:

```json
{
  "architect": {
    "build": {
      "options": {
        "assets": [
          "src/favicon.ico",
          "src/assets",
          {
            "glob": "**/*",
            "input": "src/assets/.well-known",
            "output": "/.well-known/"
          }
        ]
      }
    }
  }
}
```

This ensures that when you build the app, the `.well-known` folder (and everything inside it) is copied into the root of the final build output.

Is it done ? No there few more steps are pending.

## Configuring Rewrite Rules in AWS Amplify

Here’s where things got a bit tricky for me. AWS Amplify doesn’t automatically serve files like `apple-app-site-association` with the correct headers, and it also doesn’t automatically rewrite URLs to `.json` format when needed.

In my initial attempt, I added the rewrite rule for `.well-known/apple-app-site-association` to be rewritten to `.well-known/apple-app-site-association.json`. While this worked fine for that specific URL, the rest of the website went down for about two hours. It took me a while to realize that the order of the rewrite rules was causing the problem.

### **The Mistake**

I had added the `.well-known` rule first, followed by other rules for the rest of the website. However, this sequence made the Amplify rewrite engine get stuck, causing an issue where the website would not load as expected. Although the `.well-known` URL was working fine, the other pages were not. It was a frustrating situation, and I realized that the sequence of the rewrite rules was the root cause of the issue.

### **The Solution**

After some troubleshooting and a bit of trial and error, I figured out that the correct order of rewrite rules was essential for everything to work properly. I deleted all the previous rules and started fresh, ensuring the rewrite rules were correctly ordered.

Here’s how I corrected it:

1. I placed the `.well-known` rule **first** in the sequence.
    
2. Then, I added the rewrite rules to **exclude** `.well-known` and include other URLs for the rest of the website.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737018145017/882b3202-7cd7-46be-bab0-04357523c489.png align="center")

I added a rewrite rule to ensure that requests to `/apple-app-site-association` are correctly rewritten to `.apple-app-site-association.json`. This step was important because iOS expects the file to be in `.json` format, but users might request it without the extension.

I added the following rewrite rule to handle that:

```json
{
  "source": "/.well-known/apple-app-site-association", 
  "target": "/.well-known/apple-app-site-association.json",
  "status": "200"
}
```

Additionally, I configured a default rewrite rule to handle non-asset URLs and redirect them to the main `index.html` for proper routing in the Angular app:

```json
{
  "source": "/^(?!\.well-known/)(?!.*\.(css|js|jpg|jpeg|png|gif|svg|woff|ttf|eot|ico|mp4|mp3|json)$).*",
  "target": "/index.html",
  "status": "200"
}
```

This ensures that non-asset URLs are routed to the `index.html` page, which is typical for single-page applications (SPAs) built with Angular.

## Deploying and Verifying

Once everything was set up, I deployed the application to AWS Amplify. After deployment, I tested the URL where the `apple-app-site-association` file should be served, and everything worked as expected:

```bash
https://your-amplify-app-url/.well-known/apple-app-site-association
```

I verified that the file was accessible and confirmed that it was being served with the correct content type and caching headers.

#### iOS Verification

To verify that the `apple-app-site-association` file is being served correctly, you can use Apple's verification tool:

```bash
https://app-site-association.cdn-apple.com/a/v1/yourdomain.com
```

This URL should return the contents of the `apple-app-site-association.json` file and show that it's being served with the correct `Content-Type` (`application/json`). If it works, your iOS deep linking is correctly configured.

## Android Deep Linking — A Similar Process

While this blog focuses on iOS deep linking with AWS Amplify, it’s worth mentioning that Android also requires a similar configuration. For Android, you’ll need to set up the `assetlinks.json` file in the same `.well-known/` directory. The process is very similar to what we’ve done for iOS, and once set up, deep linking will work seamlessly across both platforms.

## Conclusion: A Learning Experience

Setting up deep linking wasn’t easy, but it was incredibly rewarding. I learned how to handle iOS and Android deep linking in an Angular app, how to host the necessary files on AWS Amplify, and how to configure the right rewrite rules for proper routing. What seemed like a daunting task at first ended up being a great learning experience.

By following these steps, you should be able to set up deep linking for your Angular app hosted on AWS Amplify. I hope my journey helps you navigate this process more smoothly and efficiently!

Happy coding, and may your deep links always work perfectly!

### References :

* [https://dev.to/developeralamin/deep-linking-for-andriod-and-apple-in-reactjs-1hjc](https://dev.to/developeralamin/deep-linking-for-andriod-and-apple-in-reactjs-1hjc)
    
* [https://developer.android.com/training/app-links/deep-linking](https://developer.android.com/training/app-links/deep-linking)
    
* [https://yeeply.com/en/blog/mobile-app-development/deep-linking-android-ios-apps/](https://yeeply.com/en/blog/mobile-app-development/deep-linking-android-ios-apps/)
---
title: "How to Implement Authorization in React JS"
datePublished: Wed May 01 2024 09:39:49 GMT+0000 (Coordinated Universal Time)
cuid: clvnmltfo000309l7ce2r4dyt
slug: how-to-implement-authorization-in-react-js
canonical: https://www.cerbos.dev/blog/how-to-implement-authorization-in-react-js
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714556207419/20d3871c-6113-42dd-bc02-636a26543c4d.jpeg
tags: authorization, learning, reactjs, integration, cerbos, cerbos-hub

---

Hello devs,

In this blog, we will learn how to implement an authorization mechanism in ReactJS applications using Cerbos. Before, let's understand the basics of authorization and why it is needed. 

Any application that requires a user login needs an authorization mechanism to control user access to different components of the application. For example, which user can access the admin page, which user can edit, update and delete operations, etc. Authorization is like rules for traffic control and road design. Cars go in the car lanes, bikes use bike lanes, and pedestrians use sidewalks. Pedestrians can enter the car lanes via crosswalks when there is a green walk signal and a corresponding red light for the cars. . Imagine a bustling city without traffic control. Chaos, right? The authorization mechanism creates rules in an application to place different types of users in their own lanes. . Let's check out the critical elements for authorization below. 

## **Key elements for authorization** 

* **User** - Entity who is involved in acting upon the application.
    
* **Role** - A collection of permission groups for user management. One role can be assigned to many users, or many roles can be assigned to one user. It is a one-to-one, one-to-many, or many-to-one relationship. As per the above example, the traffic police will be in one role, the traffic signal will be in another role, and travelers who are driving or walking on the road will be in another role. 
    
* **Permission**—This defines what kind of action users can perform on which resources. For example, when a signal is red, you should stop and not cross the road. It also manages whether users have  add, edit, update, or delete access. 
    
* **Resources** - These are protected entities within applications like pages, data, or specific functionalities. 
    
* **Policies**—These rules define a user's ability to perform certain actions within a specific duration or condition. For example, when the walking signal is ON, only walkers can cross the road in zebra crossing areas. At other signals, walkers should not attempt to cross the road. 
    

To develop this kind of authorization mechanism, key elements need to be designed from scratch or used in conjunction with a third-party tool or library that can provide this mechanism. This can be a time-consuming and complex task. Fortunately, there are alternative solutions! 

Here, [**Cerbos**](http://cerbos.dev/) comes into the picture.

## **Authorization as service**

Cerbos is an authorization as a service provider. Cerbos empowers you to implement robust authorization in your applications with ease. It centralizes authorization logic, eliminating the need for custom code and simplifying access control management.  It replaces your lengthy code with just a few lines by calling Cerbos APIs. Cerbos has multiple features: 

* Fine-grained access control with RBAC, ABAC, and beyond
    
* Streamlined access policy management
    
* Permissions aware data filtering
    
* Stateless and scalable
    
* Policyplayground to test your policy before implementing it.
    

There are many tools like this available on the market, so why Cerbos? Why not others? So let's understand this. 

## **Why Cerbos?** 

Cerbos is designed and developed to be used as a service rather than just a library, which is compiled into an application. This design thinking process provides several benefits. 

* **Instant updates** - To make changes in access control, you don’t need to change any application code. Just play around with policy in Cerbos Hub. 
    
* **Increase visibility** - Cerbos makes defining access rules easy! Instead of complex code, you can write them in a simple format called YAML. Even better, It provides a management interface called a [**Cerbos Hub**](https://www.cerbos.dev/product-cerbos-hub), which has a user-friendly console that lets anyone, not just developers, experiment with changes to these rules. This way, everyone involved can see how access permissions work within the application.
    
* **Don’t repeat yourself** - Since it is not written in application code, you don’t need to repeat that logic in the same application workflow or other services, even if you use a different programming language for different services. With the advent of modern-day microservice technology-driven applications, developers use various programming languages for different microservices, depending on the need for and compatibility of features. Cerbos is your centralized access control for applications.
    
* **Use best practices**—Follow standard integration patterns and the development best practices you use daily to develop and deploy authorization logic.
    

Detailed audit trails- It provides good audit trails where you can see access logs.

If you wish to run the Cerbos server locally as a docker container, you can skip the “**Setup Cerbos Hub**” step below. I’m using the Cerbos hub because it provides me with some extra benefits and ease of access. There are three ways to test Cerbos policy. 

1. **Cerbos Playground** - [**https://play.cerbos.dev/**](https://play.cerbos.dev/) ( it is not recommended to use in production. Just for testing ) 
    
2. **Cerbos Hub** - [**https://hub.cerbos.cloud/**](https://hub.cerbos.cloud/) ( One place to manage everything) 
    
3. **Cerbos API** - [**https://docs.cerbos.dev/cerbos/latest/api/**](https://docs.cerbos.dev/cerbos/latest/api/). You can keep the policy under code and run docker containers to call Cerbos API.
    

### **Setting up Cerbos Playground**

[**Cerbos Playground**](https://play.cerbos.dev/) is a utility for creating and testing policies online. If you aren’t sure how to write a policy, try this policy generator tool: [**https://play.cerbos.dev/new?generator**](https://play.cerbos.dev/new?generator). You just need to know which page you will control access to and which roles you will have, like admin, user, sales, etc.

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F6ca2b6f89ec84f94839534258070b476?width=740 align="left")

Once you create a policy using the generator, just copy it and add it to your playground.

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F70193eb81ecc457fab002814da8777cf?width=740 align="left")

Once you are done with this exercise, you can test the policy by clicking on “Try the API,” which will make you click on the “Save” button. You can also import the same API in Postman and test. Will use the API in React code later in this blog.

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F50108fd1e79b4aa1a27350388aad056c?width=740 align="left")

## **Setting Up Cerbos Hub**

Before setting up Cerbos Hub, let's understand what it is exactly. 

Cerbos PDP (Policy Decision Point ) is an open-source authorization solution. This is where your application authorization decision is made. A place where all policies, roles, and access are defined, and where authorization is checked while a user accesses the application. PDP can be a cloud instance or a local Docker instance. In other words, it is the endpoint. 

Cerbos Hub is a UI console that manages your PDP instances. There are multiple benefits to using Cerbos Hub, But let’s keep that for later. Today, we will use it for its CI/CD implementations that provide Github integration so our policies can automatically get deployed. How does it work? 

### **Step 1: Creating an organization and a workspace**

* Go to [**Cerbos Hub**](https://hub.cerbos.cloud/) to sign up, or if you are an existing user, then log in.
    
* Sign up using OAuth like Google or Github or simply email address and password.
    
* Create your organization by filling in details
    

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F83f53564523541e1b0438eac9893757f?width=740 align="left")

* Then, you will have other options, like seeing a demo playground to check how policy control works, creating a new playground, and creating a new workspace. The creation of the playground will be the same as the above “Setting up Cerbos Playground” step. The only difference here is you can customize the folder structure as you like. But try to use the recommended structure.
    

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F585dd4d72fa54656acc4ca7cc7560646?width=740 align="left")

* The next step is to create a workspace. Your application PDP is going to run in this workspace. It will ask you to connect a GitHub repository. Because I selected Github in step 3 I will automatically get the option to connect to Github. If you selected different options like Code commit, Bitbucket, or Gitlab, you will get those options.
    
* Create a new repository for code, or if you have an existing repository, create a file `.cerbos-hub.yaml` in the root folder and copy the following code into the file. You can choose whichever branch you wish to use. In this example, I used the main branch: 
    

```yaml
apiVersion: api.cerbos.cloud/v1
labels:
  latest:
    branch: main
```

* After selecting the relevant repository tool, such as Github, you will be prompted to authorize it. If you prefer, you can skip step 6 and clone my [**repository**](https://github.com/AvinashDalvi89/cerbos-policy-demo) instead, or you can create your own policy and repository. 
    
* Select an organization from the dropdown and then choose whether `.cerbos-hub.yaml` is in the root directory or another subdirectory. Then click on “Create a workspace”. 
    
* This is an important step. Clicking “Create a workspace” will open a modal showing a secret key. Copy that key and store it securely so it will not be lost. Then you can see a dashboard like this:
    

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F4be6f0d8febf4b6f902ab8a178a76919?width=740 align="left")

* To set up client credentials, follow these simple steps: 
    

1. Go to Settings and click on "Client credentials."
    
2. Generate a client credential by clicking on the relevant button.
    
3. Copy the values that appear and store them securely. You will need them in the next step.
    
4. Whenever you make changes to the policy in GitHub or commit changes, new builds will be generated under "Builds."
    
5. You can check the status of these builds in the "Builds" section. 
    

### **Step 2: Deploy a PDP**

* Click on “Decision Point” to create a PDP instance. Then click on “Deploy Decision Point”. It will show multiple options for running a PDP instance.
    

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F9da88c3d1ac94ab1861d03b8e9f6ba38?width=740 align="left")

* We will choose to run as a Docker container. Copy the above value and replace the workspace secret key you got in the above steps.
    

```bash
  docker run --rm --name cerbos \
  -p 3592:3592 -p 3593:3593 \
  -e CERBOS_HUB_BUNDLE="latest" \
  -e CERBOS_HUB_WORKSPACE_SECRET="..." \
  -e CERBOS_HUB_CLIENT_ID="..." \
  -e CERBOS_HUB_CLIENT_SECRET="..." \
  ghcr.io/cerbos/cerbos:0.34.0 server
```

* Run this command in the local terminal. Make sure you have already installed Docker Desktop, and it is running. 
    
* Then follow the steps and open this in the browser [**http://localhost:3592/**](http://localhost:3592/) and you can check all API details [**http://localhost:3592/**](http://localhost:3592/). You will use this URL in the React client code.
    

```bash
curl --location 'http://localhost:3592/api/plan/resources' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data '{
  "action": "read",
  "principal": {
    "id": "cltjvfunf000014nby8zppt1x",
    "roles": [
       "admin"
    ]
  },
  "resource": {
    "kind": "contact"  
  }
   
}'

```

Once you call this API, the response will look like this.

```json
{
    "action": "read",
    "resourceKind": "contact",
    "filter": {
        "kind": "KIND_ALWAYS_ALLOWED"
    },
    "cerbosCallId": "01HS1R87057YXZ0PBC657MN622"
}

```

* Whenever you make changes in the Github repository, you will see a deployment status, including checks and alerts if a policy statement or syntax is wrong. To reflect changes, you need to stop the container and restart it again.
    

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2Fb679a19b107f4d678593f3a33b2a27a8?width=740 align="left")

### **Alternative Deployment Option: Run Cerbos locally (Without Cerbos Hub)** 

If you wish to run a Cerbos server within the demo code locally, create the folder “cerbos” and under that create a file .cerbos.yaml and copy the  code below.

```yaml
storage:
  driver: disk
  disk:
    directory: /basic-crud/policies
    watchForChanges: true
```

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F5685abe3d4004a888cd0d7d3467849d7?width=666 align="left")

You can define the directory file name as you like. I named mine “basic-crud/policies”. You can refer to this file here. Add your resource policies after creating the folder “policies” inside the “cerbos” folder shown in the point #1 screenshot.

Run the command below to use the Cerbos local API. Make sure the mounting drive name “basic-crud” matches the “directory” attribute from the `.cerbos.yaml` file.

```bash
docker run --rm --name cerbos \
 -v $(pwd)/cerbos/:/basic-crud \
 -p 3592:3592 \
 -p 3593:3593 \
 ghcr.io/cerbos/cerbos:latest \
 server --config=/basic-crud/.cerbos.yaml
```

Run a test on http://localhost:3592 for HTTP or http://localhost:3593 for GRPC. Check this API with the below CURL request, or just use the URL in the request to take you to the API documentation page.

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F8cfbe68328414ac786a57c44a586da83?width=740 align="left")

Use this CURL command in your terminal or Postman to test the policy:

```bash
curl --location 'http://localhost:3592/api/check/resources' \
--data '{
    "requestId": "061dfbb1-4d85-4911-925a-b54287ad7879",
    "principal": {
        "id": "demoUserID",
        "roles": [
            "marketing"
        ]
    },
    "resources": [
        {
            "actions": [
                "read"
            ],
            "resource": {
                "kind": "contact",
                "id": "1"
            }
        }
    ]
}'
```

We have explored all three possible options for running the Cerbos API. You can use this API URL under the Cerbos configuration, or whichever framework you choose, like React, PHP, or other languages. In this blog, we will cover React. Let's check out how to use Cerbos in React. 

## **Integrating Cerbos with React** 

In my example code, I will use these roles to explain how access control works.

![](https://cdn.builder.io/api/v1/image/assets%2F2517c53399d846eea97f6c5ae8804b7f%2F77c2bfb1ad5c4d309ad399a53eec2227?width=740 align="left")

Install the Cerbos client package in your React code. You can use either GRPC or REST packages.  I will choose REST. 

```bash
npm i @cerbos/http
```

Create a new file under the lib folder and name it cerbos.ts. Add the below code for Cerbos client integration. There are two ways to call the Cerbos API: one is using the Cerbos Playground instance ID and the second one is directly calling the local API, which we ran under “Setting Up Cerbos Hub” [**http://localhost:3593**](http://localhost:3593/). 

For a Cerbos Hub PDP instance or Cerbos local API, use this code:

```js
//cerbos.ts
import { HTTP } from "@cerbos/http";

export function getCerbosClient(){
    const cerbos = new HTTP("http://localhost:3592");
    return cerbos;
}
```

For Cerbos Playground use the following code in the demo created for both options with the switch.

```js
 const cerbos = new HTTP("https://demo-pdp.cerbos.cloud", {
            playgroundInstance: "LnQCrZuCzZ660bda75BB9iF2qQdLutza", 
       });
```

Import the following file into the required page file. For example, if you have `/contact`, then use a relevant .ts file. Refer to the demo code for an example. [**https://github.com/AvinashDalvi89/react-cerbos-demo**](https://github.com/AvinashDalvi89/react-cerbos-demo)

```js
import { getCerbosClient } from "../lib/cerbos";
```

Then initialize a variable to call the `getCerbosClient` function.

```js
const cerbos = getCerbosClient();
```

Use this code to call cerbos API. Refer to this [**https://github.com/AvinashDalvi89/react-cerbos-demo/blob/main/src/pages/Admin/Admin.tsx**](https://github.com/AvinashDalvi89/react-cerbos-demo/blob/main/src/pages/Admin/Admin.tsx) for sample code. The `isAllowed`method gives responses as True or False based on access.

```js
await cerbos.isAllowed({
  principal: {
    id: "user@example.com",
    roles: ["USER"],
    attr: { tier: "PREMIUM" },
  },
  resource: {
    kind: "document",
    id: "1",
    attr: { owner: "user@example.com" },
  },
  action: "view",
})
```

You can place this code under `useEffect()` to add a conditional block to the run page with this check once you get a response back from Cerbos API. You can refer to an example here. [**https://github.com/AvinashDalvi89/react-cerbos-demo/blob/main/src/pages/Admin/Admin.tsx**](https://github.com/AvinashDalvi89/react-cerbos-demo/blob/main/src/pages/Admin/Admin.tsx) or see below. The below code uses user “admin” under the “kind” attribute, but you can refer to the actual logic in the demo code. This page code is for the `/admin` page. This page will have access to only admin. “User” and “marketing” roles will not have access.

```js
import React, { FC, useState } from 'react'; 
import { getCerbosClient } from '../../lib/Cerbos'; 
import { useEffect } from 'react';

const Admin: FC = () => {

    const [data, setData] = useState(Boolean);
    
    async function checkAccess() {
        const cerbos = getCerbosClient();
    
        const contactQueryPlan = await cerbos.isAllowed({
            principal: {
            id: "demoUserID",
            roles: ["admin"],
            attr: { },
            },
            resource: {
            kind: "admin",
            id: "1",
            },
            action: "read",
        }); // => true
        setData(contactQueryPlan);
        return contactQueryPlan;
        
    }
    useEffect(() => {
       checkAccess();
     }, [userDetails]);  
     if (data) {
        return (
            <> 
              <div className='container'>
                <h2>Admin Access</h2>
                
              </div>
            </>
          );
     }
     else{
        return (
            <> 
              <div className='container'>
                <h2>Sorry don't have access to this page</h2>
                
              </div>
            </>
          );
     }
 
}

export default Admin;
```

Run your React app with a `npm start`  and try different pages with  different roles and add similar code on different pages. 

As part of the integration with React, users can choose either Cerbos Hub, local container, or playground options. The choice depends on your use cases. 

Want to see how this works? Explore the  [**demo**](https://github.com/AvinashDalvi89/react-cerbos-demo) to get a feel for it. Or, if you're interested in diving deeper, you can clone the repository and follow the instructions in the readme. 

This blog is originally published on [Cerbos](https://www.cerbos.dev/) official blog and written by me and Rohit Ghumare.

### **References :** 

* Demo : [**https://github.com/AvinashDalvi89/react-cerbos-demo**](https://github.com/AvinashDalvi89/react-cerbos-demo)
    
* Documentation: [**https://docs.cerbos.dev/cerbos/latest/installation/container**](https://docs.cerbos.dev/cerbos/latest/installation/container)
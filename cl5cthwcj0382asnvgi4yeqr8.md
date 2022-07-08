## How to troubleshoot browser network call using har file ?

Hello Devs, 

In this blog we will learn about how to get API network log and debug those for triaging issues if any. Few days back my client was facing some issues with the website. On this issue I wasn't able to debug on the client machine. For this I ask the client to export the `.har` file from the chrome browser network call tab. Then I used that file to triage issues and it helped me to triage requests and responses for particular use cases. 

Let's see what is the `.har` file and what is it ? 

### What is the .har file format ? 
har is a short form of HTTP ARchive format, which tracks all network logs from the browser console ( chrome, firefox, safari ). This can be used to troubleshooting follow issues : 
- Performance issue 
- Triage API request and response
- Page rendering

Below are the HAR files generated depending on the browser variant you are using. 

- Chrome
- Firefox
- Internet Explorer
- Microsoft Edge
- Safari

### How to get this har file ? 
1. Go to webpage right click and select inspect or press F12
![Screenshot 2022-07-08 at 11.54.54 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657304659459/lVg12AKVi.png align="left")
2. Go to network tab 
3. Click the export button or label. 
![Screenshot 2022-07-08 at 11.56.42 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657304769099/GOp5bXn1n.png align="left")

### How to access or open this file ? 
1. har file can open in any text file 
2. can import to the same empty browser tab. By doing inspect element -> Network tab -> Import 
![Screenshot 2022-07-09 at 12.03.49 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1657305220709/zrRUPINF4.png align="left")

### How does this help ? 
This file helps us to debug or troubleshoot API request call request and response. After importing this API calls look exactly similar to the way it shows while loading the page. It includes everything: wait time, response time, initiator etc. 

I hope this blog helps you to troubleshoot such issues if you come across them. Feel free to reach out to me on my twitter handle @aviboy2006 or comment in the blog. If you like my blog please don't forget to like the article. It will encourage me to write more helpful articles. 

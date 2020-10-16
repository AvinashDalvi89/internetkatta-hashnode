## Share cookies or local storage data between cross domain ?

**Hello Devs**, 

Going to explain about how to share resources like cookies or local storage between subdomain and cross domain if domain owns by same owner or user.

**But why cookies sharing required **? 

Four years back i came across one problem while developing two website www.example.com ( in WordPress) and customer.example.com( in Angular) and requirement was both domain have to shared same login among them. Website should not ask customer to login again if he/she already login to one of domain. 

**What is problem ?**

Now problem was there is as per www protocol cookies and session information is not directly available between subdomain and cross domain as per privacy of data. So,`www.example.com` and `customer.example.com` even they belong to same domain `example.com` they can't read each other cookie information or local storage. 

**Would like to know how to share data between subdomain or cross domains ? **. 

So, there is way to solve this issue. Have u heard about  [Publish-Subscribe Model](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) ? 

In short Publish-Subscribe Model is where is two endpoint can be act as publisher and listener(subscriber) and they will communicate to each other via medium. lets look at below image 👇🏻 where first image explain how two people communicate and second image express how two domain can communicate.

![pub-sub-pattern.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602787664177/MxtdWXACF.png)


**How we can develop this pattern using Javascript and HTML ?** : 

This code is in javascript and html so that can used in any javascript framework and html website. I have used this in combination of WordPress and Angular project.

Consider we have two domain www.example.com and www.example2.com.  

- Step1 :  Add `postCrossDomainMessage` function in landing page like index.html

`www.example.com/index.html`

```
function postCrossDomainMessage(msg) {
  var win = document.getElementById('ifr').contentWindow;
  win.postMessage(msg, "http://abc.com/");
}
var postMsg = {"login": "user"}; // this is just example
postCrossDomainMessage(postMsg);
``` 


- Step2:  Add iframe tag in landing page where everywhere reflected. For Angular/React you can add it in index.html

```
<iframe style="display:none;" src="http://abc.com/getlocalstorage.html" id="ifr"></iframe>
```

- Step3: On recipient domain `www.example2.com` create `getlocalstorage.html` file and put this code 👇🏻 in file `getlocalstorage.html`

```
var PERMITTED_DOMAIN = "http://example.com";
/**
 * Receiving message from other domain
 */
window.addEventListener('message', function(event) {
    if (event.origin === PERMITTED_DOMAIN) {
        //var msg = JSON.parse(event.data);
        // var msgKey = Object.keys(msg)[0];
        if (event.data) {
            localStorage.setItem("localstorage", event.data);
        } else {
            localStorage.removeItem("localstorage");
        }
    }

});
```
**Note** : If you want to use `cookies` then you can set using `setCookies`  instead of `localStoarge`

We are done with one side communication.Now www.example.com data can accessible using `localStorage.getItem("localstorage");` at `www.example2.com` side. Same steps are applicable in reverse way from `www.example2.com` to `www.example.com`

Now if customer or user login to one website then no need to login to another website which same products owns. This way login can share across two cross domain.

You can find detail code example at my  [GitHub Example](https://github.com/aviboy2006/cross-domain-cookie-sharing ) . Click a star if you find this useful.

Thank you 🙏🏻 for reading article, I would 👍🏻 to connect with you at  [Twitter](https://twitter.com/aviboy2006) .

Share your valuable feedback and suggestions!


## Cross domain sharing resources like login cookies

Hello devs, would like to know how to share one login between subdomain or different domain which one user owns domains. As per www protocol cookies and session information is directly available between subdomain and different domain as per privacy of content. `www.example.com` and `beta.example.com` even they belong to same domain `example.com` they can't read each other cookie information. 

There is way to solve this issue. Have u heard about  [Publish-Subscribe Model](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) ? 

In short what is Publish-Subscribe Model is there is two endpoint where both can be act as publisher and listener(subscriber).

![pub-sub-pattern.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602787664177/MxtdWXACF.png)


`www.example.com/index.html`

```
function postCrossDomainMessage(msg) {
  var win = document.getElementById('ifr').contentWindow;
  win.postMessage(msg, "http://abc.com/");
}
var postMsg = {"login": "user"}; // this is just example
postCrossDomainMessage(postMsg);
``` 


add iframe tag in main page where everywhere reflected

```
<iframe style="display:none;" src="http://abc.com/getlocalstorage.html" id="ifr"></iframe>
```

On recipient domain `example2.com` put this code in file `getlocalstorage.html`

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
Then this data can accessible using `localStorage.getItem("localstorage");` at `example2.com` side.

You can find detail code example at my  [GitHub Example](https://github.com/aviboy2006/cross-domain-cookie-sharing ) . Click a star if you find this useful.

Thank you 🙏🏻 for reading article, I would 👍🏻 to connect with you at  [Twitter](https://twitter.com/aviboy2006) .

Share your valuable feedback and suggestions!


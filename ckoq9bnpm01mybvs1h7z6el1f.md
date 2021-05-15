## Nullish Coalescing support in Angular template

Hello Devs, 

I am going to explain about **Nullish Coalescing (??)** . Few days back while reading release details about Angular 12 I just got to know about this new word and how to write cleaner code in typescript. So, now Angular 12 view template is supporting **Nullish Coalescing(??)**

Let's first understand the meaning of **Nullish Coalescing (??)**. Then how it is supported in Angular 12 version view template. 

**What is Nullish Coalescing (??)** ? 

Nullish - means `null` or `undefined`

Coalescing - means combine (elements) in a mass or whole.

The nullish coalescing operator (??) is a logical operator that returns its right-hand side operand when its left-hand side operand is null or undefined, and otherwise returns its left-hand side operand.

![nullish-coalescing.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1621108050074/J0-WXwpNM.png)

```
const a = null ?? 'hello world';
console.log(a);
// output: "hello world"

const b = 0 ?? 2;
console.log(b);
// output: 0
``` 
Syntax to use -  
``` 
(Left side expression)   ?? ( Right side expression) 
```


**Note** : The nullish coalescing operator avoids this pitfall by only returning the second operand when the first one evaluates to either null or undefined (but no other falsy values) (e.g., '' or 0).

Another point to mentioned is `&&` or `||` operator cannot pair directly with `??` operator. You have to provide parenthesis to explicitly indicate precedence is correct. 

Not allow 🚫
```
null || undefined ?? "Hello World"; // raises a SyntaxError
true || undefined ?? "Hello World"; // raises a SyntaxError
```
Allow ✅
```
(null || undefined) ?? "Hello World "; 
// Output "Hello World"
```

Now you understand what is **Nullish Coalescing (??)**. Lets understand how this is supported in Angular 12.

Currently if you are using a statement in a template like this. Where `imageUrl` is either set by component or child component. if `imageUrl` is not set then go for `getRandomImages()` function call. 

```
{{imageUrl !== null && imageUrl !== undefined ? imageUrl : getRandomImages() }}
```
Can be written using **Nullish Coalescing (??)**
```
{{ imageUrl ?? getRandomImages() }} 
```

<iframe src="https://codesandbox.io/embed/nullish-coalescing-example-hzbc9?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="nullish-coalescing-example"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

 [Github Code Link ](https://github.com/aviboy2006/angular12-nullish-coalescing-example) 

Thanks for reading this blog. Hope you understand this concept. And if you have any query related to this concept can reach out to me over my twitter handle  [@aviboy2006](https://twitter.com/aviboy2006) or can raise an issue on GitHub link. 

**References** : 
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator
- https://blog.angular.io/angular-v12-is-now-available-32ed51fbfd49
## Implement async call in PHP using guzzle promise

Few days back I came across the same situation where I wanted to execute a piece of parallel. I never used parallelisation  before (async promise) in PHP. Went through multiple options, lots of developers suggested upgrading to PHP 7 first and using a compatible library. But upgrading 20+ applications on PHP 5 to PHP 7 can be difficult to upgrade at the last minute. Further investigation I found one package which suited for PHP5 is `guzzle/promise`. Soon we will see how the `guzzle/promise` works and how to use it. Before that we will understand what is promise in javascript.

**What is Promise in Javascript?**

A **Promise** is magic for values without knowing when promise is created. Its allows you to handlers with an asynchronous process with success or failure reason.  
A Promise is in one of these states:

- pending: initial state, neither fulfilled nor rejected.
- fulfilled: meaning that the operation was completed successfully.
- rejected: meaning that the operation failed.


**How to deal with async programming in PHP?** ( Server side language ) ? 

Using `guzzle/promise` can deal with async. In this its works like Javascript but there is extra effort need to taken care to create promise function to deal with async calls. 

![concurrency graph.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1608663992412/W6KAVOPaL.png)

Most of you are familiar with Javascript and generally in a framework like Angular or react, its default option available in services.

** [Guzzle Promises](https://github.com/guzzle/promises) **

Promises/A+ implementation that handles promise chaining and resolution iteratively, allowing for "infinite" promise chaining while keeping the stack size constant. Read this blog post for a general introduction to promises.

**How to use promise without external end point by using local function as promise. 
**
**Prequisite** : 

- `composer require guzzlehttp/guzzle` 

**Steps for example** : 

1. Use package in file : 
```
use GuzzleHttp\Promise\Promise;
use GuzzleHttp\Promise\EachPromise;
```
2.  Create promise function : 
```
function promiseFunction($sample_array,$index){  // index is optional params 
      $promise =  new Promise(
                 function () use ($sample_array,$index,&$promise) {
                            $response = secondFunction($sample_array);
                            $promise->resolve($response);
                 },
                 function ($sample_array,$index,&$promise) {
                            // do something that will cancel the promise computation (e.g., close
                            // a socket, cancel a database query, etc...)
                            $promise->reject($index); // just to know which index has failed or rejected.
                  }
               );
        return $promise;
    }
```
For debugging purpose you can add `echo` inside `onFulfilled` after `secondFunction()` and before to know hows pipelines works. Below are sample log to get idea 
```
0..starting function resolve for promise.....2020-12-02 10:03:21....
0..ending function resolve for promise.....2020-12-02 10:03:22....
1..starting function resolve for promise.....2020-12-02 10:03:22....
1..ending function resolve for promise.....2020-12-02 10:03:23....
2..starting function resolve for promise.....2020-12-02 10:03:23....
2..ending function resolve for promise.....2020-12-02 10:03:23....
3..starting function resolve for promise.....2020-12-02 10:03:23....
3..ending function resolve for promise.....2020-12-02 10:03:24....
4..starting function resolve for promise.....2020-12-02 10:03:24....
4..ending function resolve for promise.....2020-12-02 10:03:25....
5..starting function resolve for promise.....2020-12-02 10:03:25....
5..ending function resolve for promise.....2020-12-02 10:03:26....
6..starting function resolve for promise.....2020-12-02 10:03:26....
6..ending function resolve for promise.....2020-12-02 10:03:26....
7..starting function resolve for promise.....2020-12-02 10:03:26....
7..ending function resolve for promise.....2020-12-02 10:03:27....
8..starting function resolve for promise.....2020-12-02 10:03:27....
8..ending function resolve for promise.....2020-12-02 10:03:28....
9..starting function resolve for promise.....2020-12-02 10:03:28....
9..ending function resolve for promise.....2020-12-02 10:03:28....
```

3. Call promise function where you have to optimise or call promise. 

On `fulfilled` can pass parameters as call by reference `&$sample_array` to access inside `eachPromise` using `use` operation can pass multiple parameters. 
```
function sampleFunction($sample_array_set){
        
            $promises = array();
            foreach($sample_array_set as $index => $sample_array){
                $promises[$index] = promiseFunction($sample_array,$index); 
            }

           $eachPromise = new EachPromise($promises, [
          // how many concurrency we are use
          'concurrency' => count($promises),  // can set to 4 ,10,20 depend on how many iteration but better to used same as count
          'fulfilled' => function ($response,$index) use (&$sample_array)
           {
             //logic for other operation.
               
            },
          'rejected' => function ($reason) {
          }
        ]);
         
    }
```
There is other way to achieve this but performance will be higher than this approach. Like using `$results = Promise\settle($promises)->wait();` to resolve promises. Using `settle` can iterate result set see example and promise function can be same as per step 1  : 👇🏻
```
  foreach ($results as $key => $value) {
     $response = value['value']->getBody()->getContents();
     $header = value['value']->getHeader();
 }
```
**Conclusion**:
This approach help me to reduce execution of time of expensive function from 50 secs to 10s to 20s. Not much impressive but while handling 100 of parallel request its can see performance changes. 

Hope this helps you. Thanks for reading this blog. If any feedback feel free to reach out to me.

**Here is Github gist** : https://gist.github.com/aviboy2006/0c0c3f554ad6da3931b2463b2bfb32b2

**References** : 

- https://stackoverflow.com/questions/65033343/how-to-improve-response-time-using-guzzle-promise
- https://medium.com/@ardanirohman/how-to-handle-async-request-concurrency-with-promise-in-guzzle-6-cac10d76220e
- https://stackoverflow.com/questions/51958450/when-settling-multiple-async-promises-in-guzzle-im-only-able-to-get-result-fro


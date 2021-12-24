## Difference between export as class and object in javascript ?

Hello Devs,

Here I am going to share what I learn from my fellow colleague while working on react application. I generally start using some concept whichever I learn recently ( generally whichever I learned I kept like arrows in quiver and whichever depends on the instance I used it ) then implement whatever requirement comes. But sometimes that concept may not be useful or best to use. This is what I learned. 

Long back ago I learned to use singleton class for some third party initiation code. So,  I used many times as class but one problem was addressed by colleague is everything is Javascript is object based even if we use finally it converted to object based only then if can be done simple object based approach will help to reduce lines of code and simpler approach.  Instead we should use export as object which will be a simpler approach and less code line compared to class based approach. 

Let me explain how to export as a class and object.

First see how to export as class or singleton class : 
Below sample code is singleton class and it exports as default singleton class. 

```
// ExampleClass.js

export default class ExampleClass {
  constructor() {
    if (this.constructor.instance) {
      return this.constructor.instance;
    }
    this.constructor.instance = this;
  }

  /**
   * @name ExampleClass#setData
   * Role of this function is to set data
   * @param {Object} data
   */
  setData(data) {
    this.data = data;
  }

  /**
   * @name ExampleClass#getData
   * Role of this function is to get data
   * @returns {Object} data
   */
  getData() {
    return this.data;
  }
}
```
This class we will import in another file. 

```
// Samplefile.js
import ExampleClass from 'ExampleClass';
const obj = new ExampleClass();
const data = {};
obj.setData(data);
```

```
// Samplefile1.js

import ExampleClass from 'ExampleClass';
const obj = new ExampleClass();
const data = obj.getData();
```
So, when we use an import statement and create a new object whether class is singleton or normal class. When bundling whole code either Angular or React or other framework it adds code lines from `ExampleClass.js` to each file wherever instance got created. 

## How to avoid this ? 

![hmm-thinking.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1639750816229/pEMEIq78m.gif)

Answer is we can export as objects. Because import as an object we don't need to create an instance of it. Occurrence of code line from exported object will be one time in bundle. 

``` 
// Example.js

const example = {
  data: {},
};

export default {

  setData(token, isIVToken = false) {
    example.data = {
      key: 'value'
    }
  },

  getData() { 
    return example.data
  },
};

```
Actually as I said earlier everything is object only in Javascript. Only other approaches evolve based on condition and demand of projects like class, singleton or object based. 

**Note** :  This is what I learned recently. We have to use an approach based on the situation. Nothing like which one is best. 

Hope this blog helps you. If you like my blog please don't forget to like the article. It will encourage me to write more such learning related blogs. You can reach out to me over my twitter handle @aviboy2006.

Feel free to comment if anything is wrong in this blog. I am happy to learn and correct.



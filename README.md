首先， 我们知道，Javascript 是没有模块的概念，只有服务器端才有模块的概念。

![image](https://user-gold-cdn.xitu.io/2019/1/9/1683175166043985?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

一开始前端的工作内容不多，html中如图中引入js文件，有很多缺点：

必须按顺序引入，如果 1.js中要用到jquery，那就将jquery.js放到1.js上方。

同步加载各个js，只有1.js加载并执行完，才去加载2.js。

各个js文件可能会有多个window全局变量的创建，污染。

......还有很多缺点

总之，上面的结构，在前端内容越来越多，尤其ajax的趋势、前后端分离、越来越注重前端体验，js文件越来越多且互相引用更复杂的情况下，真心乱套了，所以需要有一个新的模块化工具。
我们当然是希望像现在这样，文件之间互相 import、export 就行了，但遗憾的是，这是es6配合node的用法，需要服务端做支撑处理文件，而一开始仅通过静态文件去模块化。

## AMD and CMD

[详解](https://juejin.im/post/5c3592b26fb9a049aa6f4456)

AMD(Asynchronous Module Definition, 异步模块定义) 是 RequireJS 在推广过程中对模块定义的规范化产出。
CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。
CommonJS 是 BravoJS 在推广过程中对模块定义的规范化产出。
还有不少⋯⋯这些规范的目的都是为了 JavaScript 的模块化开发，特别是在浏览器端的。目前这些规范的实现都能达成`浏览器端模块化开发的目的`。

区别：
1. 对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。

不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。

CMD 推崇 as lazy as possible.2. CMD 推崇依赖就近，AMD 推崇依赖前置。

看代码：

```js
// CMD

define(function(require, exports, module) {  
var a = require('./a')   
a.doSomething()  
// 此处略去 100 行  

var b = require('./b') // 依赖可以就近书写   
b.doSomething()   
// ... 
})
```
代码在运行时，首先是不知道依赖的，需要遍历所有的require关键字，找出后面的依赖。具体做法是将function toString后，用正则匹配出require关键字后面的依赖。显然，这是一种牺牲性能来换取更多开发便利的方法。

而AMD是依赖前置的，换句话说，在解析和执行当前模块之前，模块作者必须指明当前模块所依赖的模块，表现在require函数的调用结构上为：

```js
// AMD 默认推荐的是
define(['./a', './b'], function(a, b) {  // 依赖必须一开始就写好    
a.doSomething()    
// 此处略去 100 行    

b.doSomething()    
...

})

```



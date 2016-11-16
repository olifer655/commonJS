# 1.CommonJS规范
   CommonJS 规范是为了解决 JavaScript 的`作用域`问题而定义的模块形式，可以使每个模块它自身的命名空间中执行。该规范的主要内容是，模块必须通过
module.exports 导出对外的变量或接口，通过 require() 来导入其他模块的输出到当前模块作用域中。

   CommonJS 是同步加载模块，但其实也有浏览器端的实现，其原理是现将所有模块都定义好并通过 id 索引，这样就可以方便的在浏览器环境中解析了，可以参考
require1k 和 tiny-browser-require 的源码来理解其解析（resolve）的过程。

# 2.CommonJS 模块的特点

* 所有代码都运行在模块作用域，不会污染全局作用域。
* 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
* 模块加载的顺序，按照其在代码中出现的顺序。

# 3.module对象
```
function Module(id, parent) {
  this.id = id;
  this.exports = {};
  this.parent = parent;
  // ...
```
每个模块内部，都有一个`module`对象，代表当前模块。它有以下属性。
* module.id 模块的识别符，通常是带有绝对路径的模块文件名。
* module.filename 模块的文件名，带有绝对路径。
* module.loaded 返回一个布尔值，表示模块是否已经完成加载。
* module.parent 返回一个对象，表示调用该模块的模块。
* module.children 返回一个数组，表示该模块要用到的其他模块。
* module.exports 表示模块对外输出的值。

### 3.1 module.exports属性

`module.export`s属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取`module.exports`变量。

### 3.2exports变量

为了方便，Node为每个模块提供一个exports变量，指向module.exports。这等同在每个模块头部，有一行这样的命令。

   var exports = module.exports;
   
造成的结果是，在对外输出模块接口时，可以向exports对象添加方法。

```
exports.area = function (r) {
  return Math.PI * r * r;
};

exports.circumference = function (r) {
  return 2 * Math.PI * r;
};
```

注意，不能直接将exports变量指向一个值，因为这样等于切断了exports与module.exports的联系。

# 4.require命令
### 4.1 基本用法




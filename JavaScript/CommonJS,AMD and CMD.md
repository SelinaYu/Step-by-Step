#  CommonJS,AMD and CMD

标签（空格分隔）： javascript

---
## JavaScript中的模块规范
  网站逐渐发展，JavaScript的代码也越来越庞大，越来复杂。JavaScript模块化在此显得就尤为重要。
  模块化编程的好处：
  
- 给系统解耦，代码易于维护
- 更方便使用别人代码
- 想要什么功能加载什么模块

## CommonJS

1）`require()`全局方法，加载模块
```
var math = require('math');//导入模块
```
2） `exports`全局变量，用来导出模块方法
```
//math.js
exports.add = function(){}
```
CommonJS 最早是叫 ServerJS，node.js 的模块系统就是参照 CommonJS 规范实现的。通过上面可以看出 `require()` 是同步的，在服务器端，请求的文件都存放在本地硬盘，同步加载等待的时间就是硬盘读取时间。
但是在浏览器端，同步就会出现问题，浏览器中的模块放在服务器端。要请求就会有网络请求，若依然同步加载模块，请求的时间太长会导致浏览器假死。
要实现浏览器端的，就要制定新的标准，而在制定新标准的过程中，CommonJS 社区中有了分歧，于是就分出了 AMD 规范。

## AMD
1) `define()` 全局函数，定义模块
> define(id?, dependencies?, factory);
```
define('math',['jquery'],function($){});
```
2） `require()`全局函数，加载模块，不同于CommonJS
>require([module]?, callback);

- [module]: 数组，成员是它要加载的模块
- callback: 加载成功后的回调

AMD规范主要是异步的，保证不会阻塞到浏览器，通过回调来管理依赖与被依赖。

## CMD
CMD兼容CommonJS规范，它与AMD有很多相似之处。CMD 推崇的是依赖就近，当依赖真正使用的时候，才去加载这个模块并执行。
关于CMD的规范可以[参考这里][1]
**`require()`**
1.同步加载
```
require('./a.js')
```
2.异步加载
```
require.async(id,callback)
```
## AMD和CMD的区别 

- AMD的模块预加载，CMD的模块按需加载-懒加载
```
//AMD 的预加载
require(['jquery','math','exception'],function($,math,exception){})
```
```
//CMD的按需加载 -- 懒加载
define(function(require,exports,module){
  dosomething();
  ...
  require('./a.js');
  doothersomething();
  ..
  require('./b.js');
  ....
})
```

需要知道的是：
AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。
[关于SeaJS和RequireJS的异同可以看这里][2]

扩展阅读：
[CMD 模块定义规范][3]
[AMD (中文版)][4]


  [1]: https://github.com/seajs/seajs/issues/242
  [2]: https://github.com/seajs/seajs/issues/277
  [3]: https://github.com/seajs/seajs/issues/242
  [4]: https://github.com/amdjs/amdjs-api/wiki/AMD-%28%E4%B8%AD%E6%96%87%E7%89%88%29
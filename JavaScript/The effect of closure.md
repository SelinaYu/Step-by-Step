# The effect of closure

标签（空格分隔）： javascript

---
## 什么是闭包？
来自[MDN的定义][1]
> Closures are functions that refer to independent (free) variables (variables that are used locally, but defined in an enclosing scope). In other words, these functions 'remember' the environment in which they were created.
>闭包是指那些能够访问独立(自由)变量的函数 (变量在本地使用，但定义在一个封闭的作用域中)。换句话说，这些函数可以“记忆”它被创建时候的环境。

我的理解是，闭包可以读取其他函数内部变量的函数。创建闭包最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量。
```
function sayHello(){
  var text = 'hello';
  return function(){
    console.log(text)
  }
}
var foo = sayHello();
foo();
```
## 闭包的作用

**1) 模块化代码**

使用匿名闭包(IIFE模式)创建模块
```
var Module = (function(){
  var name = "selinayu";
  var sayName = function(){
    console.log(name)
  }
  return {sayName: sayName}
})()
Module.sayName();
```
这种构造模式，让我们的匿名函数有自己的作用域，可以在函数内部使用局部变量，而不会意外覆盖同名全局变量，并且还能访问到全局变量。

**2) 模拟私有成员**

JavaScript 中没有私有成员的概念。所有属性都是公有的。但是函数有私有变量的概念，函数外部不能访问私有变量。可以利用闭包的特性创建访问私有变量的公有方法(特权方法)
```
function Person(value){
  var name = value;
  this.sayName = function(){
    return name;
  }
}
var per = new Person("selinayu");
per.sayName();
```

**3) 函数的柯里化**

curry的概念：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
```
var add = function(x) {
  return function(y) {
    return x + y;
  };
};
var increment = add(1);
var addTen = add(10);
increment(2); // 3
addTen(2);  // 12
```
这里为什么要固定一个参数返回一个新函数，而不是直接定义一个新函数？
如果直接定义一个新函数，虽然特定的功能也能完成，但是代码的通用性缺降低了。

##  闭包的缺点
1) 内存泄漏问题

一般函数执行完毕后，局部活动对象就被销毁，内存中仅仅保存全局作用域。但闭包的情况不同！闭包会使函数中的变量都保存在内存中，内存消耗大，所以不能滥用闭包，在IE中可能会导致内存泄漏。解决办法把不使用的数据，将其值设置为null来释放其引用(解除引用)

2) 性能问题

执行闭包函数后，父函数保留下来的活动对象并不是闭包函数的作用域链的首位，首位存放的是闭包的活动对象，当频繁访问跨作用域的标识符时，每次都会造成性能的损失，解决办法把常用的跨作用域变量存储在局部变量中直接访问。


扩展阅读：
[学习Javascript闭包（Closure）][2]
[Javascript 的模块化][3]
[js 闭包的使用技巧][4]


  [1]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures
  [2]: http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html
  [3]: http://mertensming.github.io/2016/10/29/javascript-modularization/
  [4]: https://segmentfault.com/a/1190000007840125
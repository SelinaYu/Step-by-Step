# null和undefined的区别

标签（空格分隔）： javascript

---
<h3>Null和Undefined</h3>
Undefined类型只有一个值，即undefined。当声明的变量还未被初始化时，变量的默认值为undefined。
Null类型也只有一个值，即null。null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象。
区分这两个值可以认为`undefined`代表一个意想不到的没有值，而`null`作为预期没有值的代表。

``` javaScript
console.log(typeof undefined);//undefined
console.log(typeof null);  //object

```
**null和undefined**
JavaScript的最初版本是这样区分的：
null是一个表示"无"的对象，转为数值时为0；
undefined是一个表示"无"的原始值，转为数值时为NaN。

``` javaScript
Number(null)   //0
Number(undefined)  //NaN
```

**null**表示"没有对象"，即该处不应该有值。典型用法是：
（1） 作为函数的参数，表示该函数的参数不是对象。

（2） 作为对象原型链的终点。
```
Object.getPrototypeOf(Object.prototype)//null 
```
(3)试图获取一个不存在的元素时，返回null值。
(4)垃圾收集机制，将不再有用的数据设置为null来解除引用，确保回收内存。

**undefined**表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：
（1）变量被声明了，但没有赋值时，就等于undefined。
```
var foo;  
```
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
```
function foo(x){console.log(x)};
foo();
```
（3）对象没有赋值的属性，该属性的值为undefined。
```
var obj = new Object();
console.log(obj.color);
```
（4）函数没有返回值时，默认返回undefined。
```
var value ;
value = (function(){})();
```
(5)`void`操作符也可以返回一个`undefined`值。因为它是不可变的，可以在任何上下文依赖返回`undefined`:
```
function isUndefined(obj){
  return obj === void 0;
}
```

扩展阅读
----------
[探索JavaScript中Null和Undefined的深渊][1]
[undefined与null的区别--阮一峰][2]


  [1]: http://yanhaijing.com/javascript/2014/01/05/exploring-the-abyss-of-null-and-undefined-in-javascript/
  [2]: http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html
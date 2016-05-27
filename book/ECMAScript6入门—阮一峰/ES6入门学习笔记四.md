# ES6入门学习笔记四

标签（空格分隔）： ES6

---
<h3>Symbol</h3>
引入Symbol的原因保证每个属性的名字独一无二。
注意：
1）不能使用new命令，否则报错。
2）不能与其他类型的值进行运算，会报错。
3）可以转换成字符串，也可以转换成布尔值，不能转换成数值。
4）Symbol值作为对象属性名时，不能用点运算符。
<h3>Proxy</h3>
Proxy用于修改某些操作的默认行为，即对编程语言进行编程，ES6原生提供Proxy构造函数，用来生成实例。
```
var proxy = new Proxy(target, handler)
```
**注意：要使得Proxy起作用，必须针对Proxy实例进行操作，而不是目标对象。**
<h3>二进制数组</h3>
1）ArrayBuffer对象代表原始的二进制数据
2）TypedArray视图用来读写简单类型的二进制数据
3）DataView视图用来读写复杂类型的二进制数据。
<h3>Set</h3>
Set类似于数组，但是成员的值都是唯一的，没有重复的值。类似精确运算符（`===`），但是NaN等于自身，两个对象不相等。
**WeakSet**结构与**Set结构**类似，也是不能重复的集合。区别在于：
1） WeakSet的成员只能是对象，而不能是其他类型的值。
2） WeakSet中的对象都是弱引用，垃圾回收机制不考虑WeakSet对该对象的引用。
3） WeakSet不能遍历，因为成员都是弱引用，随时可能消失。
<h3>Map</h3>
Object结构提供了“字符串——值”的对应，Map结构提供了“值——值”的对应。
注意只有对同一个对象的引用,Map结构才将其视为同一个键。
```
var map = new Map();
map.set(['a'],111);
map.get(['a']);   //undefined
```
上面set和get操作的对象实际上是两个对象，内存地址是不一样的。如果Map的键是简单类型的值，则只要两个值严格相等，Map将其视为一个键，包括0和-0。NaN虽不严格相等自身，但Map将其视为同一个键。
**WeakMap**与**Map**结构基本相似，唯一的区别是它只接受对象作为键名（null除外）。当对象被回收后，WeakMap自动移除对应的键值对，有助于防止内存泄漏。
<h3>Generator函数</h3>
1)Generator是一个状态机，还是一个遍历器对象生成函数。调用Generator函数后，该函数并不执行，返回的是指向内部状态的指针对象。
2)任意一个对象的`Symbol.iterator`方法，等于该对象的遍历器生成函数。
3)`for...of`循环可以自动遍历Generator函数，但当返回对象的属性为true是，for...of循环会中止，且不包含该返回对象。
4)使用`yield*`语句，用来在一个Generator函数里执行另一个Generator函数（`yield*`不过是`for...of`的一种简写，任何数据结构只要有Iterator接口，就可以被`yield*`遍历）
5)Generator函数不会返回结果，而是返回一个指针对象
<h3>Promise函数</h3>
1）Promise新建后就会立即执行。
2）一般来说，不要在then方法里面定义Reject状态的回调函数（即then的第二个参数），总是使用catch方法。
```
promise
  .then(function(data){
  })
  .catch(function(err){
  })
```
<h3>Async函数</h3>
async函数对Generator函数的改进：

- async自带执行器，Generator函数借助co
- 返回Promise对象，Genetator返回Iterator对象
注意：
1) await命令后面是一个Promise对象，否则被转成Promise,运行结果可能是rejected，所以把`await`命令放在`try..catch`中
2)多个`await`命令后面的异步操作，不存在继发关系，最好让它们同时触发
```
//写法一
let [foo,bar] = await Promise.all([getFoo(),getBar()]);
//写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

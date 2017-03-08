# Inheritance

标签（空格分隔）： javascript

---

## prototype,constructor,[[Prototype]]
- prototype: 每个函数都具有一个 prototype 属性，指向原型对象
- constructor: 默认情况，原型对象会自动获得 constructor 属性，该属性包含一个指向 prototype 属性所在的函数指针
- [[Prototype]]: 实例的属性，指向构造函数的原型对象，浏览器通过 `__proto__` 访问

## 原型链继承
核心思想： 子类的原型是父类的实例。
```
function Super(){
  this.colors = ["red","blue","yellow"]
}
function Sub(){}
//继承Super
Sub.prototype = new Super();
var instance1 = new Sub();
var instance2 = new Sub();
instance2.color.push("green");
console.log(instance1.color); //["red", "blue", "yellow", "green"]
```
原型链看下图
![此处输入图片的描述][1]
由图可以看出： instance 指向了 Sub.prototype,Sub.prototype 指向了 Super.prototype 。
缺陷： 

- 引用类型值的原型属性会被所有实例共享  
- 子类不能向父类构造函数传递参数

## 借用构造函数

核心思想： 子类构造函数的内部调用父类构造函数。
```
function Super(name){
  this.name = name;
  this.colors = ["red","blue","yellow"]
}
function Sub(){
//继承Super，传递参数
  Super.call(this,"selinayu");
}
var instance1 = new Sub();
var instance2 = new Sub();
//解决共享原型对象的引用属性问题
instance1.colors.push("green");
console.log(instance1.colors);
console.log(instance2.colors);
```
缺陷：

- 父类定义的方法，子类无法复用，子类实例中都是新的父类方法
- 父类原型定义的属性和方法，子类无法继承 

## 组合继承
核心思想： 使用原型链实现对原型属性和方法的继承，借用构造函数实现实例属性的继承。
```
function Super(name){
  this.name = name;
  this.colors = ["red","blue","yellow"]
}
Super.prototype.sayName = function(){
  console.log(this.name)
}
function Sub(name,age){
//继承属性
  Super.call(this,name);
  this.age = age;
}
//继承原型链的方法和属性
Sub.prototype = new Super();
Sub.prototype.constructor = Sub;
var instance1 = new Sub("Bob",15);
instance1.colors.push("green");
console.log(instance1.colors);
var instance2 = new Sub("Greg",27);
console.log( instance2.colors );
```
缺陷： 两次调用构造函数。子类原型上有一份多余的父类实例属性，调用子类构造函数时，会在新对象创建实例属性，这两个属性屏蔽原型上的同名属性，造成内存浪费。

## 原型式继承
核心思想：
```
function object(o){
  function F(){}
  F.prototype = o;
  return new F();  
}
```
ECMAScript5新增Object.create()方法来规范化了原型式继承。这个方法接收两个参数，一个是作为新对象原型的对象和(可选的)一个为新对象定义额外属性的对象。

- 优点： 不用借助构造函数，直接通过Object.create()传入一个父类对象，即可让子类继承这个父类对象的属性和方法。
- 缺陷： 和使用原型模式意义，也会共享父类对象的引用属性。

## 寄生式继承
核心思想：创建一个仅用于封装继承过程的函数，在函数内部以某种方式增强对象。
```
function createAnother(o) {
    function F() {};
    F.prototype = o;
    var clone = new F();        //创建一个对象(使用原型式继承)
    clone.sayName = function() {  //增强对象
        console.log(this.name);
    }
    return clone;               //返回对象
}
```
缺陷： 函数不能复用，效率降低，共享属性问题
 
## 寄生组合式继承
核心思想：所谓寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。
```
function inheritePrototype(subType, superType) {
    var prototype = Object(superType.prototype);    //核心，创建对象（创建一个包含父类原型中所用方法的对象）
    prototype.constructor = subType;                //核心，增强对象（将这个对象的constructor指向子类）
    subType.prototype = prototype;                  //核心，指定对象

}
```
普遍认为寄生组合式继承是引用类型最理想的继承范式。

## 小结
多看红宝书！
多看红宝书！
多看红宝书！
重要的事情说三遍！
 
  [1]: http://7xq2ky.com1.z0.glb.clouddn.com/%E5%8E%9F%E5%9E%8B%E9%93%BE%E7%BB%A7%E6%89%BF.png
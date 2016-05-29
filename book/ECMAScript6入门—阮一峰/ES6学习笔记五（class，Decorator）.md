# ES6学习笔记五（class，Decorator，module）

标签（空格分隔）： 未分类

---

<h3>Class</h3>
1）类(`Class`)的内部所有定义的方法，都是不可枚举的，在ES5中是可枚举的。
2）`constructor`方法会被默认添加，也可以指定返回另外一个对象
```
Class Foo{
  constructor(){
    return Object.create(null);//返回全新的对象，导致实例对象不是Foo类的实例
  }
}
new Foo() instanceof Foo //false
```
3）使用表达式定义类的时候注意class 后面的变量只在内部代码使用，类名为声明的那个。
```
const MyClass = class Me{
  getClassNamez(){
    return Me.name;
  }
}
```
4) 不存在变量提升

**<h4>Class的继承</h4>**

在ES5中，实质是先创造子类的实例对象，再将父类的方法添加到`this`上。
在ES6中，实质是先创造父类的实例对象`this`（先调用`super`方法）,再用子类的构造函数修改`this`。
注意：

- 子类的构造函数，只有调用`super`后，才可以使用`this`，。子类实例的构建基于对父类实例加工，`super`返回父类实例
- 子类的`__proto__`表示构造函数的继承，指向父类
- 子类`prototype`属性的`__proto__`属性,表示方法的继承，总是指向父类的`prototype`属性。
```
class A {
}
class B extends A{
}
B.__proto__ === A //true
B.prototype.__proto__ = A.prototype //true
```

**<h4>三种特殊情况的继承</h4>**

1） 子类继承Obeject类,子类其实就是Object的复制，子类的实例就是Object的实例
2）不存在继承
```
class A {
}      //相当于普通函数
A.__proto__ === Function.prototype //true
A.prototype.__proto__ === Object.prototype //true A调用后返回一个空对象
```
3）子类继承null
```
class A extends null{
}
A.__proto__ === Function.prototype //true
A.prototype.__proto__ === undefined     //true
```
**<h4>原生构造函数的继承</h4>**

ES5 中因为子类新建实例，再将父类的属性添加到子类上，由于无法获得原生构造函数的内部属性，所以无法继承原生的构造函数。
ES6 中因为父类新建实例对象this,然后子类的构造函数再修饰`this`，使得父类的所有行为都可以继承
**<h4>Class的静态方法</h4>**

类相当于实例的原型。当方法添加`static`关键字（静态方法）**不会被实例继承，可以被子类继承**，通过**类或者super()对象**来直接调用。
**<h4>new.target</h4>**

1) 如果不是通过`new`命令调用的，`new.target`会返回`undefined`
2) `Class`内部调用会返回当前`Class`
3) 子类继承父类时，`new.target`会返回子类。
<h3>Decorator修饰器</h3>
**<h4>类的修饰</h4>**
```
function decorator(target){        //target指的是A,修饰器函数的第一个函数是所修饰的目标类
}

@decorator
class A {}
//等同于

class A{}
A = decorator(A) || A;
```
**<h4>方法的修饰</h4>**

修饰器函数一共可以接受三个参数，第一个是所要修饰的目标对象，第二个参数是索要修饰的属性名，第三个参数是该属性的描述对象。
注意：
修饰器只能用于类和类的方法，不能用于函数，因为存在函数提升
<h3>Module</h3>
1） ES6的模块自动采用严格模式，并且是编译时加载。
2）使用`export default`命令，为模块指定默认输出。使用`import`命令时，可以使用任意名称指向刚刚`export`的内容，此时不使用大括号。
3） 
```
export * from 'circle';
```
`export *`表示输出`circle`模块的所有属性和方法。但是`export *`命令会忽略`circle`模块的`default`方法。
4）**ES6模块输出的是值的引用，而CommonJS模块输出的是一个值的拷贝。**
5） 如果默认输出一个函数，函数名的首字母应该小写，对象的首字母应该是大写。
<h3>使用ES6</h3>
**字符串**
静态字符串一律使用单引号或反引号，不适用双引号
。动态字符串使用反引号。
**解构赋值**
以下情况优先使用解构赋值：
1） 使用数组成员赋值
2）函数的参数是对象的成员
```
function getFullName({firstName,lastName})
```
3）函数返回多个值
**对象**
1）单行定义的对象，最后一个成员不以逗号结尾。多行定义的对象，最后一个成员以逗号结尾。
```
const a = {k1:v1,k2:v2};
const b = {
  k1:v1,
  k2:v2,
};
```
2)使用`Object.assgin`添加新的属性
**数组**
1) 使用扩展运算符`...`拷贝数组。
```
const itemsCopy = [...items];
```
2) 使用`Array.from`将类似数组的对象转为数组。

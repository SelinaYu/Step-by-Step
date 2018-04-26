## 基本数据类型

五种原始数据类型： 
```
boolean, number, string, null, undefined, symbol
```
注意：`null` 和 `undefined` 是所有类型的子类型，也就是说 `null` 和 `undefined` 类型的变量，可以赋值所有类型的变量。


除了原始数据类型，此外还有其他的：

### 1.数组(Array)

有如下两种写法

**方式一，数组元素的类型 + []**
```
let list: number [] = [1,2,3]
```

**方式二，Array<elemType>**

```
let list: Array<number> = [1,2,3]
```
### 2.元组(Tuple)

主要合并不同类型的对象

```
let xcatliu: [string, number] = ['Xcat Liu', 25];
```
注意： 可以赋值其中一项，当直接对元组类型的变量进行初始化或者赋值的时候，需要提供所有元组类型中指定的项。

### 3.枚举(Enum)

用于取值被限定在一定范围内的场景。

```
enum Color {Red, Green, Blue}
```

默认情况下，枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射；我们也可以手动设置 开始递增的数字不为0。或者设置该值不是数字，但此时需要设置类型断言让tsc 无视类型检查。

### 4.Any

一些动态生成的内容，不希望类型检查器检查直接通过编译的，可以使用 Any 类型

```
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

注意，如上， `Object` 类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法。

### 5.void

可以用来表示没有任何返回的函数,定义变量时只能定义为 null 或者 undefined.

### 6.never

never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。这意味着声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无线循环）

```
// 返回值为 never 的函数可以是抛出异常的情况
function error(message: string): never {
    throw new Error(message);
}

// 返回值为 never 的函数可以是无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}
```
### 7.类型断言

类型断言（Type Assertion）可以用来手动指定一个值的类型。

**两种方式**

`<类型>值`  或者   `值 as 类型`


```
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
let strLength: number = (someValue as string).length;
```
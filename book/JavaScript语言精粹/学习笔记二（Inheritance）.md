# 学习笔记二（Inheritance,Array）

标签（空格分隔）： JavaScript语言精粹

---
<h1>Inheritance</h1>
(一)对象说明符，当构造器接受一大串参数，可以让它接受一个简单的对象说明符。这样多个参数可以按任何顺序排列。
```
//bad
var myObject = maker(f,l,m,c,s);
//good
var myObject = maker({
  first:f,
  middle:m,
  city:c,
  last:l
})
```
(二)Functional模式
1.创建一个新对象
2.有选择地定义私有实例变量和方法
3.给这个新对象扩充方法。
4.返回那个新对象
<h1>Array</h1>
(一)JavaScript数组的length是没有上限的。超过length的数字来作下标存储元素不会发生越界错误。




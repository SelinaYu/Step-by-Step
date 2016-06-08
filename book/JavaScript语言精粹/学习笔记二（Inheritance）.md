# 学习笔记二（Inheritance）

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





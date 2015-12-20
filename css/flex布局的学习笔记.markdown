# flex布局的学习笔记

标签（空格分隔）： css


---

一、flex布局的常用术语
=============

     Flex容器(容器)：采用flex布局的元素
     Flex项目(项目)：Flex容器的子元素
     容器默认的两根轴：水平的主轴(main axis)和垂直的侧轴
     主轴的起始及占据的空间：main start 、 main end 和miansize
     侧轴的起始及占据的空间：cross start、 cross end和cross size
![flex布局的相关术语解析图][1]


  [1]: http://www.w3.org/html/ig/zh/wiki/images/b/bf/Flex-direction-terms-new.zh-hans.png "flex布局的相关术语解析图"

二、 flex容器的相关属性
============

1、 **flex-direction**
决定主轴的方向，即**flex项目**的排列方向
```
.container{
flex-direction: row | row-reverse | column | column-reverse;
}
```

     row(默认值)：主轴为水平方向，起点在左端
     row-reverse: 除了主轴起点与主轴终点方向交换,其他同"row"的属性作用
     column: 主轴为垂直方向，起点在上方
     column-reverse:除了主轴起点与主轴终点方向交换，其他同"column"的属性值作用
2、**flex-wrap**
控制Flex容器是单行还是多行，也决定了侧轴方向 ― 新的一行的堆放方向。
```
.container{
flex-wrap: nowrap | wrap | wrap-reverse;
}
```

     nowrap(默认值)：不换行
     wrap: 换行，第一行在上方
     wrap-reverse: 换行，第一行在下方，除了侧轴起点与侧轴终点方向交换以外同wrap所起作用相同。
3、**flex-flow**
`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。
```
.container{
flex-flow: <flex-direction> || <flex-wrap>
}
```
4、**justify-content**
定义项目**在主轴上**的对齐方式。
```
.container{
justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

     flex-start(默认值)：项目向一行的起始位置靠齐
     flex-end: 项目向一行的结束位置靠齐
     center: 项目向一行的中间位置靠齐（如果剩余空间是负数，则保持两端溢出的长度相等）
     space-between: 两端对齐，项目之间的间隔都相等。(如果剩余空间是负数，或该行只有一个伸缩项目，则此值等效于「flex-start」。)
     space-around:每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
5、 **align-items**
定义项目**在侧轴上**的对齐方式
```
.container{
align-items: flex-start | flex-end | center | baseline | strech;
}
```
     flex-start: 侧轴的起点对齐
     flex-end: 侧轴的终点对齐
     center： 侧轴的中点对齐
     baseline: 项目的第一行文字的基线对齐
     stretch(默认值)：如果项目未设置高度或auto,将占满整个容器的高度
6、 **align-content**
调准伸缩行在伸缩容器里的对齐方式。如果是项目是单行，该属性不起作用。
```
.container{
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

     flex-start: 与侧轴的起点对齐
     flex-end: 与侧轴的终点对齐
     center: 与侧轴的中点对齐
     space-between: 与侧轴的两端对齐，轴线之间的间隔平均分布
     space-around: 每根轴线两侧相等，所以轴线之间的间隔比轴线与边框的间隔大一倍
     stretch(默认值):轴线沾满整个侧轴

三、flex项目的属性
===========
1、**order**
定义项目的排列顺序。数值越小(可以取负值)，排列越靠前，默认0。
```
.item{
    order: 1
 }
```

2、 **flex-grow**
定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
```
.item{
flex-grow: <number>;
}
```

3、**flex-shrink**
定义项目的缩小比例，默认为1，该项目将缩小。
```
.item{
  flex-shrink: <number>；
  }
```
如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
负值对该属性无效。
4、**flex-basis**
定义在分配多余空间之前，项目占据的主轴空间。默认值为auto，即项目的本来大小。
```
.item{
  flex-basis: <length> || auto;
}
```
可以设置width和height属性一样的值，项目将占据固定空间。
5、**flex**
flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
```
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```
该属性有两个快捷值：`auto (1 1 auto)` 和 `none (0 0 auto)`。
优先使用这个属性。
6、**align-self**
允许单个项目与其他项目不一样的对齐方式。可覆盖`align-items`,默认值`auto`,表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`
```
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```
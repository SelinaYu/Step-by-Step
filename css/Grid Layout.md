# Grid Layout

标签（空格分隔）： css

---
[<h3>Basics and Browser Support</h3>][1]

Grid Layout在最新的浏览器Chrome,Firefox,Safari都没有支持，可手动开启：

 - chrome 在地址栏输入“chrome://flags”，找到"experimental web platform
   features"开启
 - firefox在地址栏输入"about:config"，找到"layout.css.grid.enabled"开启

**注意:** column,float,clear,and vertical-align对该父容器都没有影响。

**<h3>grid-template-columns&grid-template-rows</h3>**

首先这两个属性是设置Grid有几行几列的，设置父元素，并设置父元素`display:grid`
```
.containter{
  display:grid;
  grid-template-columns: 100px auto 100px;
  grid-template-rows:100px 120px 100px ;
}
```
这里的Grid的列有三列，第一列和最后一列是100px，中间的是自适应，至于行，有三行分别是`100px 120px 100px`。我们还可以使用单位`fr`，这个是按比列表示宽度的。例如：
```
.container{
  grid-template-columns: 1fr 50px 1fr 1fr;
}
```
这里的列除了第二列是50px，其余的列则是分别占剩余宽度的三分之一。
**<h3>两种合并单元格的方法</h3>**

上面定义了Grid的三行三列，共有九个单元格，如果我们要把其中的某些单元格合并的话，两种方法：

**对子元素使用grid-column和grid-row**
```
.item {
  grid-column: 2;
  grid-row:1;
}
```
这里表示该元素在第一行第二列，也可以使用`grid-column:1 /3`表示第一条`column-line`到第三条，即前两列，还可以使用`grid-column: 1 /span 2`表示从第一条线开始占据两列。这样子就能合并前两列了。
**分别对父子元素使用grid-template-areas和grid-area**
直接上代码简洁明了：
```
.container{
  grid-template-columns: 50px 50px 50px ;
  grid-template-rows: auto;
  grid-template-areas: "header  header header"
                       " main . sidebar"
                       "footer footer  footer"
}
.item-a{
  grid-area: header;
}
.item-b{
  grid-area: main;
}
.item-c{
  grid-area: sidebar;
}
.item-d{
  grid-area: footer;
}
```
上面需要注意的是`grid-template-areas`里有个值为`.`，这个点表示空的单元格。

**<h3>其他属性</h3>**

上面提过的属性就不再提了。
**父容器的属性**
分两类，一类关于对齐的方式的属性，一类不关于对齐方式的属性
关于对齐方式的属性：

- ustify-items：item在水平行中的对齐方式
- align-items：item在垂直栏中的对齐方式
- justify-content：整个水平行在grid范围的对齐方式
- align-content：整个垂直栏在grid范围的对齐方式

不关于对齐方式的属性:

- grid-column-gap：定义垂直栏与垂直栏之间的间距
- grid-row-gap：定义水平行与水平行之间的间距
- grid-gap：上面两个栏与行间距的缩写
- grid-auto-columns：定义多出的item的自动column的宽度大小
- grid-auto-rows：定义多出的item自动row的高度大小
- grid-auto-flow：定义自动item是按照先水平方向排列还是垂直方向排列

**子元素item的属性**
关于对齐方式的属性

- justify-self：自定义item的水平方向对齐方式
- align-self：自定义item的垂直方向对齐方式

不关于对齐方式的属性:

- grid-column-start：item的起始栏
- grid-column-end：item的结束栏
- grid-column：起始栏和结束栏的简写
- grid-row-start：item的起始行
- grid-row-end：item的结束行
- grid-row：起始行与结束行的简写
- grid-area：item所在区域



参考链接：
[A Complete Guide to Grid][2]


  [1]: http://caniuse.com/#search=grid
  [2]: https://css-tricks.com/snippets/css/complete-guide-grid/
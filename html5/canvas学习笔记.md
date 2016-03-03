# canvas学习笔记

标签（空格分隔）： html5

---
<h3>绘图步骤</h3>
使用canvas绘图，绘图方式有两种：一类是进行图形操作，另一类是图像操作。使用API进行绘图的步骤如下：
1. 获取canvas元素，`document.getElementById()`取得元素
2. 获取canvas元素的环境上下文，`canvas.getContext("2d")`获取2D图像上下文。
3. 确定绘图模式。两种绘图模式：`fill`和`stroke`。`fill`会填充整个图形，`stroke`值会描绘边框。
4. 设定绘图样式。`fillStyle`和`strokeStyle`指定绘图样式，分别对应`fill`模式和`stroke`模式,默认是黑色。
5. 指定线宽。通过`lineWidth`设定绘制线宽，默认值为1.0像素。

<h3>关于canvas的填充</h3>
1. **填充纯色`fillStyle`**
2. **填充渐变色**
canvas提供两种渐变对象，一种线性渐变，一种径向渐变
1) `createLinearGradient(x0,y0,x1,y1)`创建线性渐变对象，开始点(x0,y0)，结束点(x1,y1);
2)`createRadialGradient(x0,y0,r0,x1,y1,r1)`创建径向渐变对象，开始以(x0,y0)为圆心， r0为半径，结束以(x1,y1)为圆心，r1为半径。
渐变对象创建完成后，可以通过该对象的`addColorStop()`在某一点中增加颜色值，这个点可以认为是关键点，关键点之间就会出现渐变效果。
 `addColorStop(offset,color)`offset表示偏移，值在0.0~1.0之间。
当使用`fillStyle`指定渐变对象的时候，就可以使用渐变的方式填充路径。
3.**贴图填充**
`createPattern(image,repetition)`image表示填充的图像,可以是img、canvas、video等 ，repetition表示贴图方式，有如下几种方式：
repeat:水平垂直方向重复贴图，默认值
repeat-x:水平方向贴图
repeat-y:垂直方向贴图
no-repeat:使用一次贴图
通过`createPattern()`创建模式对象后，可以通过`fillStyle`和`strokeStyle`指定，然后就可以使用指定的图形进行填充。
<h3>关于canvas的一些基本知识</h3>
1. 默认的，画布元素是300像素，150像素,黑色是画布的默认填充颜色

2. 使用css设置画布的宽度和高度，和使用元素的width和height的区别
使用css设置时，图像会拉伸或缩小，使用元素的width和height时，画布上的内容依然会正常绘制

3. 关于2d上下文，为什么不能直接绘制
上下文是与画布关联的一个对象，它定义了一组属性和方法，可以用来在画布上进行绘制，甚至保存上下文的状态。画布设计用来支持多个接口，除了2d,还有3d(现在还没有)等。通过上下文，就能在同一个画布元素中处理不同的接口
4. 在canvas标签内部添加不支持canvas标签的浏览器的说明 
5. 理解lineWidth的工作原理
当使用canvas绘制图形是，它是由路径向两本扩展的，各占绘制线条宽度一半。当绘制1像素的直线时，如果路径刚好在两个像素之间时，由于显示屏不可能绘制半个像素，这时会把旁边的两列都绘制。所以，如果需要绘制一像素大小的县，只需要把canvas的路径定位到某个像素中间位置即可。
6. 如果不需要路径闭合，closePath可以不用，如果使用了fill则路径则会自动闭合，不需要再使用closePath了 
7. 图像透明度可以用globalAlpha = transparency value或者rgba颜色值来表示
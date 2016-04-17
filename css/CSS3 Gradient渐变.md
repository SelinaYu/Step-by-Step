# CSS3 Gradient渐变

标签（空格分隔）： css3

---

语法：
<h3>线性渐变：</h3>
>background:linear-gradient(direction,color-stop1,color-stop2..)


*  第一个参数指明颜色渐变的方向：
可以角度，可以关键字；其中top,right,bottom,left分别表示0deg, 90deg,180deg,270deg。默认情况是从上到下。

        linear-gradient(to right,red,blue)
* 第二个参数指明了颜色断点。第一个默认为0%；第二个默认为100%；中间没有指定，则求均值

        linear-gradient(red 40%, white 60%, black 80%, blue 100%)

**浏览器如何绘制渐变线**
一个待渐变的box，以其对`角线的交点开始以CSS中样式定义的角度向两侧延伸，终点是相近的box的顶点到渐变延伸线的垂足,起点也一样。两点的距离就是渐变线的长度。

渐变线的长度计算公式是：

     abs(W * sin(A)) + abs(H * cos(A))
     A是角度，W是gradient box的宽，H为高
     
<h3>径向渐变：</h3>
>background:radial-gradient(center,shape size,start-color,...,last-color)

* 第一个参数是指定渐变圆心的位置
* shape size可以是circle或者elipse,如果省略，则默认与size有关，指定一个值就是圆形，否则椭圆
* 默认情况下，渐变的中心是 center（表示在中心点），渐变的形状是 ellipse（表示椭圆形），渐变的大小是 farthest-corner（表示到最远的角落）。
* size可以是具体的值，也可以用关键字指定，关键字如下：
     
 * closest-side是指待渐变的box某一边到盒子中心最近的距离；
 * farthest-side是指待渐变的box某一边到盒子中心最远的距离；
 * closest-corner是指待渐变的box某一顶点到盒子中心最近的距离；
 * farthest-corner是指待渐变的box某一顶点到盒子中心最远的距离；

**重复渐变**
重复的规则简单说就是用最后一个颜色断点的位置减去第一个颜色断点位置的距离作为区间长度，不断的重复。

今晚测试关键字属性的时候，一直不显示，原因是**没添加前缀**。所以说使用css3的时候，记得**添加前缀！！！**
  




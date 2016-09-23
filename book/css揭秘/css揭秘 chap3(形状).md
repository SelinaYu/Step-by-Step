# css揭秘 chap3(形状)

标签（空格分隔）： css

---

<h3>9.自适应的椭圆</h3>
`border-radius`一些鲜为人知的真相：
1)可以单独制定水平半径和垂直半径，用斜杠分隔；
2)可以接受长度值还可以接受百分比值(基于元素的尺寸解析)；
3)`border-radius`是个简写属性，可以为元素每个角设定不同的值(左上角开始以顺时针的顺序)
4)为四个角提供完全不同的水平和垂直半径，方法是斜杠前指定1~4个值
自适应的半椭圆
```
div {
	display: inline-block;
	width: 16em;
	height: 10em;
	margin: 1em;
	background: #fb3;
	border-radius: 50% / 100% 100% 0 0;
}
```
四分之一的椭圆：

```
border-radius:100% 0 0 0;
```
<h3>10.平行四边形</h3>
使用`skew()`的属性对矩形拉伸，会导致它的内容也发生斜向变形。如何只让容器变形而内容保持不变？
**嵌套元素方案**：对内容应用一次反向的skew()变形，从而抵消容器的变形效果。
**伪元素方案**：把所有样式应用到伪元素上，然后对伪元素进行变形。希望伪元素保持良好的灵活性，自动继承宿主元素的尺寸。这个方案同样适应于其他任何变形样式(变形一个元素而不想变形它的内容)
```
.button {
    position:relative;
    /*其他样式*/
}
.button :: before {
    content:'';
    position:absolute;
    top:0;right:0;left:0;bottom:0;
    z-index:-1;//伪元素生成的方块重叠在内容之上，使用z-index使堆叠层次推到宿主元素后
    background:#58a;
    transform:skew(45deg);
}
```
<h3>11.菱形图片</h3>
**基于变形的方案**：与平行四边形的嵌套元素方案差不多，把图片用一个`<div>`包裹起来然后对齐应用相反的`rotate()`
```
.pic {
	width: 400px;
	transform: rotate(45deg);
	overflow: hidden;
}

.pic img {
	max-width: 100%;
	transform: rotate(-45deg) scale(1.42);
	//这里使用scale()变形，因为使用max-width:100%;是与容器的变长相等，从而导致裁成了八角形 
}
```
**裁切路径方案**：`clip-path`属性，现在浏览器不完全支持。
```
clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);
```
<h3>12.切角效果</h3>
**css渐变方案**：特点可维护性差不理想，改变背景颜色时需要修改五处，而且繁琐冗长，不利于让各个切角的尺寸以动画的方式发生变化。
```
div{
	background: #58a;//回退机制
	background: linear-gradient(135deg, transparent 15px, #58a 0) top left,
	            linear-gradient(-135deg, transparent 15px, #58a 0) top right,
	            linear-gradient(-45deg, transparent 15px, #58a 0) bottom right,
	            linear-gradient(45deg, transparent 15px, #58a 0) bottom left;
	background-size: 50% 50%;
	background-repeat: no-repeat;//避免平铺
}
```
**弧形切角**：和上面的css渐变方案差不多，唯一的区别在于用**径向渐变**来替代上述线性渐变。
**内联svg与border-image方案**
```
div {
	border: 21px solid transparent;//勾股定理计算出边框的宽度
	border-image: 1 url('data:image/svg+xml,\
	                      <svg xmlns="http://www.w3.org/2000/svg" width="3" height="3" fill="%2358a">\
	                      <polygon points="0,1 1,0 2,0 3,1 3,2 2,3 1,3 0,2" />\
	                      </svg>');//1所对应的是SVG文件的坐标系统
	background: #fb3;
	background-clip: padding-box;//背景默认延伸到边框所在的区域下层
	}
```
**裁切路径方案**：最不DRY的，好处在于我们可以使用任意类型的背景或者替换元素进行裁切。
```
background: #58a;
clip-path:
	 		polygon(20px 0, calc(100% - 20px) 0, 100% 20px, 100% calc(100% - 20px),
	 		calc(100% - 20px) 100%,
	 		20px 100%, 0 calc(100% - 20px), 0 20px);
```
**关于未来**
css第四版将引入新的属性 `corner-shape`可以解决这个痛点。
<h3>梯形标签页</h3>
由于透视关系，三维世界中旋转一个矩形往往是一个梯形。对元素使用了3D变形后，其内部的变形效应是“不可逆转”的。所以，我们唯一可行的途径是把变形效果作用在伪元素上。
```
nav > a {
	position:relative;
	display:inline-block;
	padding:.3em 1em 0;
}

nav a::before {
	content: ''; /* To generate the box */
	position: absolute;
	top: 0; right: 0; bottom: 0; left: 0;
	z-index: -1;
	border-bottom: none;
	border-radius: .5em .5em 0 0;
	background: #ccc linear-gradient(hsla(0,0%,100%,.6), hsla(0,0%,100%,0));
	box-shadow: 0 .15em white inset;
	transform: scale(1.1, 1.3) perspective(.5em) rotateX(5deg);//旋转后后，高度缩水，scale()通过变形改变尺寸
	transform-origin: bottom;//3D空间旋转时，可以把它的底边固定住。
}
```


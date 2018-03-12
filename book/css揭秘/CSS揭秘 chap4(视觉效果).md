# CSS揭秘 chap4(视觉效果)

标签（空格分隔）： css

---
<h3>15.单侧投影</h3>

**单侧投影**

如果我们应用一个负的扩张半径，而它的值刚好等于模糊半径，那么投影的尺寸就会与投影所属元素的尺寸完全一致。

```
box-shadow:0 5px 4px -4px black;
```

**双侧投影**

```
box-shadow: 5px 0 5px -5px black,
           -5px 0 5px -5px black;
```

<h3>16.不规则投影</h3>
当元素添加了一些伪元素或半透明的装饰之后，因为border-radius会无耻地忽视透明部分，box-shadow在这方面显得力不从心。

**解决方案**


CSS滤镜基本就是SVG滤镜，只需要一些函数就可以很方便地指定滤镜效果，比如`blur()`,`grayscale()`,`drop-shadow`。
drop-shadow属性和text-shadow属性差不多，但有一个需谨记的是任何非透明部分都会被一视同仁打上投影，包括文本。

<h3>17.染色效果</h3>

为一幅灰度图片增加染色效果，一直以来，有用两个版本的图片来实现，或者在图片的上层覆盖一层半透明的纯色，或者图片设为半透明并覆盖在一层实色背景之上。
更简单，纯CSS的解决方案：

**1)基于滤镜的方案**

`sepia()`会增加一种降饱和度的橙黄色染色效果；
`saturate()`给每个像素提升饱和度；
`hue-rotate()`每个像素的色相以指定的度数进行偏移。

```
img {
	transition: 1s filter, 1s -webkit-filter;
	filter: sepia() saturate(4) hue-rotate(295deg);
}
```

**2）基于混合模式的方案**

对一个元素设置混合模式，有两个属性派上用场：`mix-blend-mode`为整个元素设置混合模式，`background-blend-mode`可以为每层背景单独指定混合模式。两种选择：
第一种：把图片包裹在一个容器，并把背景色设为我们想要的色调

```
<a href="#someting">
  <img src="pic.jpg"  alt="图片" />
</a>
```

```
 a{
    background: hsl(335,100%,50%);
  }
 img{
   mix-blend-mode:luminosity;
 }
```

第二种：不用图片元素，用`<div>`把该元素的第一层背景设置为要染色的图片，第二层设置我们想要的主色调。

```
<div style="background-image:url(pic.jpg)" class="tinted-image"></div>
```
```
.tinted-image {
	width: 640px; height: 440px;
	background-size: cover;
	background-color: hsl(335, 100%, 50%);
	background-blend-mode: luminosity;
	transition: .5s background-color;
}

.tinted-image:hover {
	background-color: transparent;
}
```

<h3>18.毛玻璃效果</h3>
使用半透明色做背景导致可读性很差，传统的设计是把所覆盖的那部分图片区域作模糊处理，直接使用blur()会对整个元素模糊，导致文本更加无法阅读。如何实现只对元素的背层(即被该元素遮住的那部分背景)应用滤镜？
  对一个伪元素进行处理，然后将其定位到元素的下层，它的背景将会无缝匹配`<body>`背景。
  
```

  body, main::before {
	background: url("http://csssecrets.io/images/tiger.jpg") 0 / cover fixed;
}

main {
	position: relative;
	margin: 0 auto;
	padding: 1em;
	max-width: 23em;
	background: hsla(0,0%,100%,.25) border-box;
	overflow: hidden;
	border-radius: .3em;
	box-shadow: 0 0 0 1px hsla(0,0%,100%,.3) inset,
	            0 .5em 1em rgba(0, 0, 0, 0.6);
	text-shadow: 0 1px 1px hsla(0,0%,100%,.3);
}

main::before {
	content: '';
	position: absolute;
	top: 0; right: 0; bottom: 0; left: 0;
	margin: -30px;
	z-index: -1;
	-webkit-filter: blur(20px);
	filter: blur(20px);
}
```

<h3>折角效果</h3>

**45度折角的解决方案**


基本原理： 在右上角增加三角形：一个三角形用来体现折页的形状，另一个白色的三角形遮住元素的一角

```
div{
	background: #58a; /* 回退机制  */
	background:
		linear-gradient(to left bottom, transparent 50%, rgba(0,0,0,.4) 0) 100% 0 no-repeat,
		linear-gradient(-135deg, transparent 1.5em, #58a 0);//是沿着渐变轴进行度量的
	background-size: 2em 2em, auto;
}
```

**其他角度的解决方案**

其他角度的解决是要调整三角形的形状，它的尺寸是由宽度和高度定义的。可以用伪元素设置一个合适的三角形，围绕着右下角旋转一定的角度，再做适当的偏移以便使斜边重合 。
[代码参考这里][1]


  [1]: http://dabblet.com/gist/bc32dc20adea2261c731

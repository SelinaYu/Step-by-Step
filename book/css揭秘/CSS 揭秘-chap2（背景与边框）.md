# CSS 揭秘-chap2（背景与边框）

标签（空格分隔）： css

---
<h1>chap 1</h1>
css编码技巧：
1) 尽量减少代码重复(DRY,Don't Repeat Yourself)，减少改动时要编辑的地方
2) 某些值相互依赖时，应该把它们的相互关系用代码表达出来
3)css有史以来第一个变量 `currentColor`

<h1>chap 2</h1>

<h3>1. 半透明边框</h3>
默认情况下，背景会延伸到边框所在的区域下层。所以，设置白色背景和半透明白色边框的时候，白色背景会从半透明边框透上来。
**使用background-clip属性：**设置元素的背景（背景图片或颜色）是否延伸到边框下面。
```
div {
    border: 10px solid hsla(0,0%,100%,.5);
    background: white;
    background-clip: padding-box;
}
```
<h3>2. 多重边框</h3>
一) box-shadow方案
box-shadow支持逗号分隔语法，我们可以创建任意数量的投影。
**但是需要注意：**
1.box-shadow是层层叠加的
2.投影的行为跟边框不完全一致，不会影响布局，也不会受到box-sizing的影响
3.box-shadow所创建的加边框，并不响应鼠标事件。
```
div {
	width: 100px;
	height: 60px;
	margin: 25px;
	background: yellowgreen;
	box-shadow: 0 0 0 10px #655,
            0 0 0 15px deeppink,
            0 2px 5px 15px rgba(0,0,0,.6) ;
}
```
二）outline方案
假设你只需要两层边框，一层常规边框，一层outline产生外层的边框。优点：边框样式(虚线)灵活，还可以使用outline-offset控制它跟元素边缘之间的间距。
**需要注意：**
1.只适用于双层“边框”，不接受逗号分隔多个值
2.outline边框不一定贴合border-radius属性产生的圆角
```
div {
	width: 100px;
	height: 60px;
	margin: 25px;
	background: yellowgreen;
	border: 10px solid #655;
	outline: 5px solid deeppink;
}
```
<h3>3. 灵活的背景定位</h3>
`background-position`指定背景图片距离任意角的偏移量，默认情况是以padding box为准的。常见情况：偏移量和容器的内边距一致。
一般代码实现如下：
```
padding: 10px;
background:url(picture.jpg) no-repeat #58a;
background-position:right 10px bottom 10px;
```
上面的代码不够DRY，可通过background-origin自动跟着我们设定的内边距走。
```
padding : 10px;
background: url(picture.jpg) no-repeat #58a bottom right;
background-origin: content-box;
```

<h3>4. 边框内圆角</h3>
实现这个可以使用两个div，外面的设置边框，里面的div设置`border-radius`。这个方案更加灵活，但是，如果只需要达成实色效果还可以用另一种方法：
```
background: tan;
border-radius:.8em;
padding:1em;
box-shadow:0 0 0 .6em #655;
outline: .6em solid #655;
```
上面代码的实现基于两个事实：
1.描边不会跟着元素的圆角走(前面提到过)
2.box-shadow会跟着边框走

**注意：** box-shadow属性指定的扩张值并不一定 等于描边的值，，推荐使用稍小一些的值。
<h3>5. 条纹背景</h3>
创建不等宽的条纹
```
background: linear-gradient(#fb3 30%,#58a 30%);
background-size: 100% 30px;
```
上面不符合DRY,修改条纹宽度的时候要修改两个数字。
>规范：如果某个色标的位置比整个列表中在它之前的色标的位置都小,则该色标的位置值会被设置为它前面所有色标位置值的最大值。

所以，当我们把上面第二个色标的位置值设置为0，那它的位置总是会被浏览器调整为前一个色标值。
**斜向条纹**
设置单个贴片(tile)包含四条条纹，实现无缝拼接。
```
background:linear-gradient(45deg,#fb3 25%,#58a 0,#58a 50%,#fb3 0 ,#fb3 75%,#58a 0);
background-size: 30px 30px;
```
我们还有更好的方法创建斜向条纹：`repeating-linear-gradient`和`repeating-radial-gradient`。
大多数情况下，我们想要的条纹图案往往属于同一个色调，如果是这样，我们可以不用为每种条纹单独指定颜色，而是把最深的颜色指定为背景色，同时把半透明白色的条纹叠加在背景色之上来得到。
```
background:#58a;
background-image: repeating-linear-gradient(30deg,hsla(0,0%,100%,.1),hsla(0,0%,100%,.1) 15px,transparent 0 ,transparent 30px);
```
上面两个代码是一样的，现在只需要修改一个地方就可以改变所有颜色了。而且对于不支持的浏览器还起到了回退的作用。
<h3>6.复杂的背景图案</h3>
**网格**
把水平和垂直的条纹叠加起来。
```
        #test2{
        	width:500px;
        	height: 500px;
        	border:2px solid ;
        	background:  #58a;
        	background-image: 
        	    linear-gradient(white 1px,transparent 0),
        	    linear-gradient(90deg,white 1px,transparent 0),
        	    linear-gradient(hsla(0,0%,100%,.3) 1px,transparent 0),
        	    linear-gradient(90deg ,hsla(0,0%,100%,.3) 1px,transparent 0);
        	background-size: 75px 75px,75px 75px,
        	                 15px 15px,15px 15px;
        }
```
**波点**
使用径向渐变，使用两层圆点阵列图案，并把它们的背景定位错开，就可以得到波点图案。
```
background: #655;
background-image: 
   radial-gradient(tan 20%,transparent 0),
   radial-gradient(tan 20%, transparent 0);
background-size: 30px 30px;
background-position: 0 0,15px 15px;
```
**棋盘**
只用一层css渐变无法创建四周有空隙的方块。这里使用两个直角三角形拼合我们想要的方块。
```

background: #eee;
background-image: 
	linear-gradient(45deg, rgba(0,0,0,.25) 25%, transparent 0, transparent 75%, rgba(0,0,0,.25) 0),
	linear-gradient(45deg, rgba(0,0,0,.25) 25%, transparent 0, transparent 75%, rgba(0,0,0,.25) 0);
background-position: 0 0, 15px 15px;
background-size: 30px 30px;
```
**角向渐变**
css图像(第四版)定义了一种新的渐变形式，可以生成角向渐变(圆锥渐变)。生成方式：所有色标的颜色变化是由一条射线绕着端点旋转来推进的。可以很简单的实现上面棋盘的效果。
```
background: conic-gradient(#bbb 0,#bbb 25%,#eee 0,#eee 50%);
background-size: 30px 30px;
```
<h3>7.伪随机背景</h3>
**Alex Walker**提出通过质数来增加随机真实性的想法，这个技巧被定名为“蝉原则”。模拟条纹的随机性：把这组条纹从一个平面拆散为多个图层：一种作为底色，另外的作为条纹以不同间隔平铺。因为有可能各层背景以不同间距重复数次后会再次统一对齐，可以使用质数来把贴片的尺寸最大化。
```
background:hsl(20,40%,90%);
background-image:
   	linear-gradient(90deg, #fb3 11px, transparent 0),
	linear-gradient(90deg, #ab4 23px, transparent 0),
	linear-gradient(90deg, #655 23px, transparent 0);
background-size: 83px 100%,61px 100%,41px 100%;
```
<h3>8.连续的图像边框</h3>
把一幅图案应用为边框，而不是背景。先了解border-image的原理九宫格伸缩法：把图片切割成九块，应用到元素边框相应的边和角。这样无法适配尺寸稍有差异的其他元素，而是希望出现在拐角处的图片区域随着元素宽高和边框厚度的变化而变化。
方法一：使用两个元素，一个设为背景，一个存放内容，并设置白色背景，这里略。
缺点：把结构和表现混合起来。
方法二：使用一个元素达成完全一样的效果。主要思路在图片之上，叠加一层纯白的实色背景，使用`background-clip`设置不同的值，这里需要注意的是图片的显示是放置在`padding-box`，需要使用`background-origin`设置。
```
div {
	border: 1em solid transparent;
	background: linear-gradient(white, white) padding-box,
	            url(http://csssecrets.io/images/stone-art.jpg) border-box  0 / cover;
	width: 21em;
	padding: 1em;

}
```


# CSS揭秘 chap5(字体排印)

标签（空格分隔）： css

---

<h3>20.连字符断行</h3>
传统的文本两端对齐`text-align:justify`,会出现单词孤岛现象，损伤可读性。可以使用新属性`hyphens: none || manual || auto`,这个为了确保它奏效，你需要在HTML属性lang中指定合适的语言。
```
width: 8.7em;
font: 180%/1.4 Baskerville, serif;
text-align: justify;
hyphens: auto;
```
<h3>21.插入换行</h3>

使用`<br/>`,可维护性差，污染了结构层的代码。专门代表换行符的Unicode字符：`0x000A`,可以简写为`\000A`或简化为`\A`。保留源代码中空白符和换行，`white-space:pre;`
```
dt, dd {
	display: inline;
}
dd + dt::before{
	content: "\A";  //换行符与相邻的其他空白符进行合并
	white-space: pre;
}
```
<h3>文本行的斑马条纹</h3>
几年前，使用`:nth-child()`和 `nth-of-type()`来解决。导致许多开发者把每行代码包裹进一个个`<div>`元素中，运用上述技巧来实现，而且过多的DOM元素拖累整个页面的性能。
**解决方案**:使用渐变做背景，设置`background-size`是`line-height`的两倍，使用`background-origin:content-box` 为基准，可以让背景自动跟着内边距的宽度走。
```
pre { 
	padding: .5em;
	line-height: 1.5;
	background: hsl(20, 50%, 95%);
	background-image: linear-gradient(
	                  rgba(120,0,0,.1) 50%, transparent 0);
	background-size: auto 3em;
	background-origin: content-box;
	}
```
<h3>调整tab的宽度</h3>
浏览器会把tab宽度设置为 8个字符，css3中`tab-size`可以设置tab的长度。
<h3>24.连字</h3>
字形与字形发生冲突，导致两者都显示不清，例如f和i。使用css3字体：`font-variant`被升级成一个简写属性。`font-variant-ligatures`专门用来控制连字效果的开启和关闭。启用所有可能的连字，需要同时指定这三个标识符。
```
body{
  font-variant-ligatures: common-ligatures discretionary-ligatures historical-ligatures;
}
```
当酌情连字可能会干扰到正常文字，只想启用通用连字：
```
font-variant-ligatures:common-ligatures;
```
如果是复位初始值，应该使用`normal`而不是`none`;`none`会把所有连字效果都关掉。
<h3>25.华丽的&符号 </h3>
`@font-face`引入WEB字体，但是生成字体麻烦，而且还增加额外的HTTP请求，所以我们可以用本地字体来实现。
```
@font-face {
	font-family: Ampersand;
	src: local('Baskerville-Italic'),
	     local('GoudyOldStyleT-Italic'),
	     local('Garamond-Italic'), local('Palatino-Italic');
	unicode-range: U+26;//描述符，使用"&".charCodeAt(0),toString(16)//返回26，16进制码位
}
```
<h3>26.自定义下划线</h3>
传统的`text-decoration:underline`,没办法直接定义文本下划线的样式，使用`border-bottom`模拟文本下划线，下划线跟文本之间的空隙很大，而且还可能阻止正常的文本换行行为。
**解决方案**
使用`background-image`及其相关属性。
```
a {
    background: linear-gradient(gray,gray) no-repeat;
    background-size: 100% 1px;
    background-position: 0px .9em;
    text-shadow: .05em 0 white,-.05em 0 white;//当下划线穿过某些字母(p和y)的降部,使用text-shadow自动断开避让
}
```
<h3>现实中的文字效果</h3>
**凸版印刷效果**
点击按钮按下或浮起的效果;技巧：浅色背景使用深色文字时，在底部加上浅色投影；深色背景浅色文字使用深色投影。
**空心字效果**
在未来可以使用`text-shadows`的扩张参数可让投影变大，目前浏览器的支持有限，而且性能消耗高(模糊算法);
```
h1 {
text-shadow: 
    1px 1px black, -1px -1px black,
    1px -1px black, -1px 1px black;
    }
```
可以使用终极方案SVG,直接附源码
```
<h1><svg overflow="visible" width="2em" height="1.2em">
  <use xlink:href="#css" />
  <text id="css" y="1em">CSS</text>
</svg></h1>
```
```
h1 text { fill: currentColor }
h1 use {
	stroke: black; 
	stroke-width: 6;
	stroke-linejoin: round;
}
```
**文字发光效果**
最简单的方法：准备几层重叠的`text-shadow`即可，不需要考虑偏移量 ，颜色也只需和文字保持一致。
```
a {
	color: #ffc;
	text-decoration: none;
	transition: 1s;
}

a:hover { text-shadow: 0 0 .1em, 0 0 .3em; }
```
**文字凸起效果**
主要思路是使用一长串累加的投影 ，不设模糊并以1px的跨度逐渐错开，使颜色逐渐变暗，然后在底部加一层强烈的模糊的暗投影，从而模拟完整的立体效果。
```
	text-shadow: 0 1px hsl(0,0%,85%),
	             0 2px hsl(0,0%,80%),
	             0 3px hsl(0,0%,75%),
	             0 4px hsl(0,0%,70%),
	             0 5px hsl(0,0%,65%),
	             0 5px 10px black;
```
<h3>28.环形文字</h3>
有一些脚本把每个字母包裹在独立的`<span>`元素，然后把各个字母分别旋转，增加了臃肿的脚本和冗余的 DOM元素。可以借助一点内联SVG来轻松解决难题。SVG原生支持以任意路径排队的文字。在SVG中，让文本按照路径排列的基本方法就是用一个`<textPath>`包裹文本。(不是很懂SVG，直接上代码)
HTML代码
```
 <div class="circular">
        <svg viewBox="0 0 100 100">
          <path d="M 0 ,50 a 50 ,50 0 1 ,1 0,1 z" id="circle"/>   
          <text>
                  <textPath xlink:href="#circle"> circular reasoning works because</textPath>
          </text>   
        </svg>

</div>
```
CSS代码
```
.circular{
        height: 30em;
        width:30em;
        margin: 3em auto 0;
}
.circular path {
        fill:none;//把黑色的填充效果去掉
}
.circular svg{
        display: block;
        overflow: visible;//避免内容的溢出部分裁切掉
}
```
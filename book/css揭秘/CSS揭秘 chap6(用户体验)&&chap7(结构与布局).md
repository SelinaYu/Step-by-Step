# CSS揭秘 chap6(用户体验)&&chap7(结构与布局)

标签（空格分隔）： css

---

<h3>29.选用合适的鼠标光标</h3>
有些光标很突出，只需花费极少量的代码，可以迅速提升网页应用的可用性。
1)提示禁用状态
`<button disabled>Disabled button</button>`
```
:disabled, [disabled], [aria-disabled="true"] {
	cursor: not-allowed;
}
```
2)隐藏鼠标光标
公共场所的信息查询台，看视频，光标会碍事,设置`cursor:none;`。
<h3>30.扩大可点击区域</h3>
**最简单的方法**是为它设置一圈透明边框，因为鼠标对元素边框的交互也会触发鼠标事件，这是描边和投影所不及的。由于背景默认情况下会蔓延到边框的下层，记得使用`background-clip`把背景限制在原本的区域之内。
```
border: 10px solid transparent;
background-clip:padding-box;
```
缺点：当使用`box-shadow`来模拟一道边框时，因为外部投影是绘制在border box外部的，所以得到怪异的效果。
所以我们可以使用另一个特性：**伪元素同样可以代表宿主元素响应鼠标交互**
```
button{
  position:relative;
}
button:before {
	content: '';
	position: absolute;
	top: -10px; right: -10px;
	bottom: -10px; left: -10px;
}
```
<h3>31.自定义复选框</h3>
单选框和复选框在绝多数浏览器中几乎或完全无法设置样式。`<label>`元素和复选框息息相关。当`<label>`元素与复选框相关联之后，也可以起到触发开关的作用。由于`<label>`不是复选框那样的替换元素，我们可以为它添加生成性内容美化一番，用来顶替原来的复选框。
```
<input type="checkbox" id="awesome" />
<label for="awesome">Awesome</label>
```
```
input[type="checkbox"] + label::before{
  content:'/a0';/*不换行空格*/
  width:.8em;
  height: .8em;
  margin-right: .2em;
  border-radius: .2em;
  background: silver;
}
input[type="checkbox"]:checked + label::before{
  content:'\2713';/*选中打勾*/
  background:yellowgreen;
}
input[type="checkbox"]{//原来的复选框隐藏，使用display:none会使它从键盘tab切换焦点的队列删除
  position: absolute;
  clip:rect(0,0,0,0);
}
```

<h3>32.通过阴影来弱化背景</h3>
最常见的方法：增加一个额外的HTML元素用于遮挡背景，添加如下样式：
```
.overplay{  /*用于遮挡元素*/
  position:fixed;
  top:0;
  left:0;
  right:0;
  bottom:0;
  background:rgba(0,0,0,.8);
}
.lightbox{
  position:absolute;
  z-index:1;
}
```
**伪元素方案**
我们可以用伪元素来消除额外的HTML元素
```
body.dimmed::before {
    content:" ";
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 1;
    background: rgba(0,0,0, 0.8);
}
```
缺点：
1)可移植性不够好，因为`<body>`元素可能有其它需要占用::before伪元素。
2)需要一点JS添加dimmed这个类
3)伪元素无法绑定独立的JavaScript事件处理函数。
**box-shadow方案**
具体做法生成一个巨大的投影，不偏移不模糊，使用扩张参数。使用视口单位vmax(1wmax相当于两者中较大值，100vw等于整个视口的宽度，100vh是整个视口的高度)。由于投影是四个方向扩展的，所以
```
box-shadow : 0 0  0  50vmax rgba(0,0,0,.8)
```
缺点：
1)当我们滚动页面的时候，遮罩层的边缘就露出来了。除非在给它加上position: fixed;
2)只能视觉上气道引导注意力的作用，确无法阻止鼠标交互。
**backdrop方案**
使用模态`<dialog>`元素(由它的showModal()方法显示出来)，借助`::backdrop`伪元素，这个原生遮罩层设置样式。
```
dialog::backdrop {
  background:: rgba(0,0,0,.8)
}
```
缺点：兼容性不好
<h3>33.通过模糊来弱化背景</h3>
动用一个额外的HTML元素把除关键元素之外的包裹起来，对这个容器 进行模糊处理。
```
<main>
  这里是正文
</main>
<dialog>
  这里是弹出窗口的内容
</dialog>
```
通过`showModal()`显示弹出窗，并同时给`<main>`设置样式进行模糊。
```
main.de-emphasized {
  filter:blur(5px);
}
```
<h3>34.滚动提示</h3>
当侧栏有滚动条的时候，容器的顶部或底部出现一层层淡淡的阴影。
**解决方案**
 使用两层背景，一层生成阴影，一层用来遮挡阴影的白色矩形。生成阴影的那层背景具有`background-attachment` 的默认值（scroll），相对元素固定，而遮罩阴影的用 local(背景会随内容而滚动),他会在滚动到顶部时盖住阴影。
 
```
 background:
   linear-gradient(white 30%,transparent),//为了有更好的过度效果，采用white到transparent的渐变作为过渡
   radial-gradient(at top,rgba(0,0,0,.2),transparent 70%);
 background-repeat:no-repeat;
 background-size:100% 50px,100% 15px;
 background-attachment:local,scroll;
 
```
<h3>35.交互式的图片对比控件</h3>
图片对比滑动控件，把两张图片叠加起来，允许用户拖动分割条来控制这两张图片的显露区域。
**CSS resize方案**
图片对比滑动控件基本上可以理解为两层结构：下层是一张固定工的图片，上层的图片则可以在水平方向上调整 大小。
html
```
  <div class="image-slider">
      <div>//调整图片大小会导致图片变形失真，所以调整容器
              <img src="pic-before.jpg" alt="">
      </div>
      <img src="pic-after.jpg" alt="">
  </div>
```
css代码
```
.image-slider {
        position: relative;
        display:inline-block;
}
.image-slider > div {
        position: absolute;
        top:0;
        left:0
        bottom:0;
        width:50%;
        overflow: hidden;
        resize: horizontal;
}
.image-slider > div::before {//调节手柄不容易辨认，使用伪元素美化
        content: "";
        position: absolute;
        right: 0;
        bottom: 0;
        width: 12px;
        height: 12px;
        background: linear-gradient(-45deg,white 50%,transparent 0);
        cursor: ew-resize;
}
```
缺点：键盘不可访问，用户体验不好
**范围输入控件方案**
利用`<input type="range" />`控制div的宽度可以直接套用前面的css，把不需要的删除，resize属性和调节手柄的CSS属性。为控件添加样式和事件
css
```
.image-slider input {
    position:absolute;
    left:0;
    bottom:10px;
    width:100%;
    margin:0;
}
```
事件
```
range.oninput = function(){
  div.style.width = this.value +"%";
}
```
<h1>chap7</h1>
<h3>36.自适应内部元素</h3>
希望元素width具有自适应内部元素的行为，可以使用width和height属性定义的 新关键字`min-content`,将解析为这个容器内部最大的不可断行元素的宽度。
<h3>37.精确控制表格列宽</h3>
表格，即使我们显示指定了width，对不固定的内容来说，也只能起到类似提示的作用。解决方案，使用`table-layout`,它的默认值是`auto`。使用`fixed`值，我们设置的宽度可以直接起作用而不仅是提示，而此时表格影响的仅是表格行的高度。
  **注意**，为了该技巧有效请为元素`<table>`或者设置`display:table`设置一个指定的宽度(哪怕是100%)。
<h3>38.根据兄弟元素的数量来设置样式</h3>
实现列表项总数为一个**固定值**(这里举例为4)时，选中每一个列表项。
```
li:first-child:nth-last-child(4),//这个选中的是一个正好有四个列表项的列表中的第一个列表项
li:first-child:nth-last-child(4)~li
```
实现列表项总数为一个**范围**时，选中每一个列表项。`:nth-child()`当它的参数为`n+b`的时候，表示从第b个元素开始的所有子元素。当它为为`-n+b`选中的是开头的b个元素。同样，用于`nth-last-child()`可以得到同样的效果。
选中列表项总数为2~6时命中的所有列表项
```
li:first-child:nth-last-child(n+2):nth-last-child(-n+6),
li:first-child:nth-last-child(n+2):nth-last-child(-n+6)~li
```
<h3>39.满幅的背景图，定宽的内容</h3>
最常见的两种方法是为每个区块准备两层元素：外层实现满幅背景，内层实现定宽的内容。我们之所以要增加一层容器，原因之一是为了给`margin`指定神奇的auto。`margin：auto`所产生的的左右边距实际是视口宽度的一半减去内容宽度的一半。我们可以使用`cal()`函数。
```
.wrapper1{
    max-width:900px;
    margin: 1em auto;
}
.wrapper2{
   padding: 1em cal(50%-450px);//这里省略width，计算的时候已经给内容留出了900px
}
```
<h3>40.垂直居中</h3>
**基于绝对定位的解决方案**
如果元素的宽高是固定的，我们可以借助calc()函数。
```
main{
    position:absolute;
    top:cal(50%-3em);
    left:(50%-9em);
    width:18em;
    height:6em;
}
```
宽高不确定时，可以借用translate()函数。
```
main{
    position:absolute;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%);
}
```
缺点： 绝对定位对布局影响太大了，居中的元素在高度上超过了视口，顶部会被视口裁掉。同时可能会导致元素显示模糊。因为 元素可能放置在半个像素上。可以使用`transform-style:preserver-3d`修复，这个秀姑手段很难保证它未来不会出问题。
**基于视口单位的解决方案**
使用视口单位
```
main {
  width:18em;
  padding:1em,1.5em;
  margin:50vh auto 0;
  transform:translateY(-50%);
}
```
注意：只适用于视口居中的场景
**基于Flexbox的解决方案**
先给带居中元素的父元素设置`display:flex`
```
.parent{
    display:flex;
    min-height:100vh;
    margin:0;
}
main{
    margin:auto;
}
```
可以先给<main>指定一个固定的尺寸，使用Flexbox规范引入相关属性，可以让内部的文字也居中。

```
main{
  display:flex;
  align-items:center;//侧轴对齐方式
  justify-content:center;//主轴对齐方式
  width:18em;
  height:10em;
}
```
<h3>41.紧贴底部的页脚</h3>
问题：页脚当页面较短时，页脚紧跟内容下方，而不是我们期望的紧贴视口底部。
**固定高度的解决方案**
方法一：给主体内容设置固定高度
```
main{
  min-height:calc(100vh-2.5em-7em);//减去页脚和页头的高度
  box-sizing:border-box;
}
```
方法二：把`<header>`和`<main>`包在一个容器
```
#wrapper{
  min-height:calc(100vh-7em);//减去页脚高度
}
```
**更灵活的解决方案**
借助Flexbox。对三个主要区块（页脚，页头，内容）的父元素body，设置样式。
```
body{
  display:flex;
  flex-flow:column;//决定主轴方向
  min-height: 100vh;
}
main{
  flex:1;//设置了大于0的值，就获得可伸缩的特性。flex值控制尺寸分配比例
}
```

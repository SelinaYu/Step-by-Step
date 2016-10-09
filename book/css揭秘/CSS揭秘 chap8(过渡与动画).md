# CSS揭秘 chap8(过渡与动画)

标签（空格分隔）： css

---

<h3>42.缓动效果</h3>
css动画使用关键帧，它的默认调速函数为`ease` ,还有其他关键字调速,较为灵活的是使用`cubic-bezier()`函数，允许我们指定自定义的调速函数。它同时接受两个锚点的坐标作为参数。**当我们把控制锚点的水平坐标和垂直坐标进行互换，可以得到任何调速函数的反向版本。**
**弹性过渡**
先弹到110%再弹回100%。
方法一：使用动画
```
@keyframes elastic-grow{
  from { transform: scale(0);}
  70% {
    transform: scale(1.1);
    animation-timing-function:cubic-bezier(.1,.25,1,.25);
  }
}
input:not(:focus) + .callout{ transform : scale(0);}
input:focus + .callout { animation: elastic-grow .5s}
.callout{ transform-origin: 1.4em -.4em;}
```
方法二：直接使用过渡
利用调速函数cubic-bezier()在垂直方向突破0~1，从而让过渡达到低于0或高于100%的程度。
```
input:not(:focus) + .callout{
  transform : scale(0);
  transtion:.25s transform;
  }
.callout{ 
  transform-origin: 1.4em -.4em;
  transition:.5s cubic-bezier(.25,.1,.3,1.5) transform}
```
上面` transtion:.25s transform;`这句声明需要特别注意。
需要设置时间的原因有两个：
1）收缩的时候会变形scale(-0.1);
2）由于放大的时候在50%，已经变大到100%，而收缩的时候将花费所有的时间，感觉会慢一半，所以要覆盖过渡时间。
默认过渡是只要可以过渡的属性都可以过渡。可以使用`transform`限制在某几种特定的属性上。
<h3>43.逐帧动画</h3>
利用`step()`调速函数。step()会根据你指定的步进数量，把把整个动画且为多帧，而整个动画会在帧与帧之间硬切。
```
@keyframes loader{
  to { background-position:-800px;}
}
.loader{
/*其他样式*/
animation: loader 1s infinite steps(8);//关键代码
}
```
<h3>44.闪烁效果</h3>
使用CSS动画进行闪烁，要实现平滑的闪烁效果，可以借助`animation-direction`属性。`animation-direction`的唯一作用就是反转每一个循环周期(reverse)或第偶数个循环周期(alternate),或第奇数个循环周期(alternate-reverse)。
```
@keyframes blink-smooth{ to { color: transparent } }
.highlight {
  animation: .5s blink-smooth 6 alternate;
}
```
**注意：**现在一次淡出淡入的的过程是由两个循环周期组成的。所以，我们要把循环时间减半，动画循环的次数翻倍。
**普通的闪烁效果**
```
    @keyframes blink-smooth {
      50%  {      //这里要设置为50%，否则没有任何变化。
                color: transparent;
        }
    }
    .callout{
        animation: 1s blink-smooth 3 steps(1);
    }
```
<h3>45.打字动画</h3>
希望一段文本中的字符逐个显现。
**解决方案**
核心思路就是让容器的宽度成为动画的主体：把所有文本包裹在这个容器中，然后让它的宽度从0开始以步进动画的方式，一个一个扩张到它应有的宽度。局限性：不适合多行文本。
```
@keyframes typing{
    from { width: 0}
}
@keyframes caret {//闪烁的光标和闪烁效果原理一样
    50% { border-color:transparent; }
}
h1{
    width: 15ch;/*文本宽度，表示15个字符的数量*/
    overflow:hidden;
    white-space:nowrap;//阻止文本折行
    border-right:.5em solid;
    animation: typing 6s steps(15),
               caret 1s steps(1) infinite;
    
    
}
```
ch单位，表示"0"字形的宽度，在等宽字体中，"0"字形的宽度和其他所有字形的宽度是一样的。
<h3>46.状态平滑的动画</h3>
动画没播完被打断，在默认情况下，动画只会立即停止播放，并生硬的跳回开始状态。使用`animation-play-state`属性。
```
@keyframes panoramic {
    to { background-position: 100% 0;}
}
.panoramic {
    width: 150px;height: 150px;
    background:url("pic.jpg");
    background-size: auto 100%;
    animation: panoramic 10s linear infinite  alternate;
    animation-play-state:paused;
}
.panoramic:hover,.panoramic:focus{
   animation-play-state: running;
}
```
<h3>47.沿环形路径平移的动画</h3>
实现简单的沿着环形移动，代码如下：
html
```
  <div class="path">
          <div class="avatar">
                  <img src="pic.jpg" alt="图片">
          </div>
  </div>
```
css
```
.path{
        background: orange;
        width: 300px;
        height: 300px;
        border-radius: 50%;
        padding:20px;
}

.avatar {
        width: 50px;
        border-radius: 50%;
        margin: 0 auto;
        overflow: hidden;
        animation: spin 5s  infinite linear;//关键代码
        transform-origin: 50% 150px;//关键代码
}
.avatar > img{
        display: block;
        width: inherit;
}
@keyframes spin{
        to { transform: rotate(1turn); }
}
```
可是，我们希望他沿着环形移动的时候，图片同时保持本来的朝向。
**需要两个元素的解决方案**
上面其实只需要html的结构代码其实只需要两个的，之所以写成三个主要为这个方案铺垫。这个方案的主要思路是用**内层的变形来抵消外层的变形效果**。上面只是对`.avatar`容器进行旋转，当我们对头像元素进行反向 旋转的时候，这两层旋转就会相互抵消。所以只要添加以下样式就可以
```
.avatar > img{
   animation:inherit;
   animation-direction:reverse;
}
```
**单个元素的解决方案**
此时html的结构如下：
```
<div class="path">
	<img src="pic.jpg" class="avatar" />
</div>
```
**上面之所以使用两个元素的原因是元素变形的原点不一样。**由于每个`transform-origin`都可以被两个`translate()`模拟出来。**每个变形函数并不是彼此独立的，而是把整个元素的坐标系统进行变形。**下面两断代码是等价的。
```
transform:rotate(360deg);
transform-origin: 200px 300px;


transform: translate(200px,300px)
           rotate(30deg)
           translate(-200px,-300px);
transform-orgin:0 0;
```
所以上面`spin`的动画可以写成
```
@keyframes spin {
  from {
    transform:translate(50%,150px)
              rotate(0turn)
              translate(-50%,-150px)
  }
  to{
    transform: translate(50%,150px)
               rotate( 1turn)
               translate(-50%,-150px);
  }
}
```
实际上这两个位移在本质上所做的就是把它放在圆心。如果把头像放在圆心并以此作为起点，就可以消除最开始的两个位移操作。所以两个动画合并之后的代码：
```
@keyframes spin{
  from {
    transform:rotate(0turn)
              translateY(-150px) translateY(50%)
              rotate(1turn);
  }
  to {
    transform:rotate(1turn)
              translateY(-150px) translateY(50%)
              rotate(0turn);
  }
}
.avatar { animation:spin 3s infinite linear ;}
```
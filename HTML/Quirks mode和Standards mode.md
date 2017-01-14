# Quirks mode和Standards mode

标签（空格分隔）： html

---
目前浏览器的渲染方式有三种模式：怪异模式（Quirks mode）、准标准模式（Almost standards mode）、以及标准模式（Standards mode）。

- 标准模式（Standard Mode）是完全符合标准的模式。

- 准标准模式（Almost Standard Mode）是几乎要符合标准的模式。

- 怪异模式（Quirks Mode）是不符合 WEB 规范的模式
<h3>DOCTYPE 的作用</h3>
在具体讨论这些模式之前，我们先要知道浏览器是如何知道我们写的CSS是什么标准的，这里引入DOCTYPE（简称DTD）的概念，即网页的头部声明，浏览器会通过识别DTD而采用对应的渲染模式，让浏览器正确的解析html和css。

<h3>XHTML</h3>
简单的来说，XHTML是升级版的html,对html进行了规范，编码更加严谨，是html和xml过渡的语言。
如果你的网页使用XHTML并在`Content-Type HTTP` 标头使用`application/xhtml+xml MIME` 类型，你不需要使用 `DOCTYPE` 启动标准模式，因为这种文件会永远使用标准模式。
如果你的`XHTML `网页使用 `text/html MIME` 类型，浏览器会将其视为 HTML，因此你就需要 DOCTYPE 启用标准模式。
**使用XHTML的局限**
xhtml语法要求严格，必须有`head`、`body` 每个dom 必须要闭合。空标签也必须闭合。例如`<img />`, `<br/>`,` <input />`等。另外要在属性值上使用双引号。一旦遇到错误，立刻停止解析，并显示错误信息。 如果页面使用`'application/xhtml+xml'`,一些老的浏览器会不兼容
<h3> Quirks mode和Standards mode的常见区别</h3>
**1）盒模型：**
盒模型（box mode）是浏览器 Quirks Mode 和 Standards Mode 的主要区别。
W3C标准盒模型的宽度和高度等于内容区的高度和宽度，不包含内边距和边框。
怪异模式的IE盒模型的宽高还包含内边距和内边框。

**2）行内元素的垂直对齐**
标准模式是以盒内文本的`baseline`对齐的；
怪异模式是以盒内文本的`bottom`对齐的；
具体可以看这篇文章[CSS深入理解vertical-align和line-height的基友关系][1]

**3）`<table>`元素中的字体**
标准模式：font的属性都是可以继承的。
怪异模式：对于table元素，字体的某些属性不会从body或其他封闭元素继承到table中，特别是font-size,font-weight。

**4）内联元素的尺寸**
CSS标准，inline元素可以分为`replaced`和`non-replaced`两类。
>`replaced`元素：有自己的宽高，如`img`，`input`
>`non-replaced`元素：如`span`元素

标准模式：`non-replaced inline`元素无法自定义大小
怪异模式：定义`non-replaced inline`元素的大小，影响该元素的大小尺寸。
**5）元素的百分比高度**
CSS 中对于元素的百分比高度规定如下，百分比为元素包含块的高度，不可为负值。如果包含块的高度没有显式给出，该值等同于“auto”（即取决于内容的高度）。所以百分比的高度必须在父元素有声明高度时使用。
标准模式： 高度取决于内容的变化
怪异模式：百分比高度则被正确应用。
在 IE Standard Mode 下，overflow取默认值 visible，即溢出可见，这种情况下，溢出内容不会被裁剪，呈现在元素框外。而在 Quirks Mode 下，该溢出被当做扩展box来对待，即元素的大小由其内容决定，溢出不会被裁剪，元素框自动调整，包含溢出内容。
6）元素溢出
标准模式：`overflow`取默认值`visible`，即溢出可见，不会被裁剪，呈现在元素框外。
怪异模式：被当作扩展box对待，即元素的大小由其内容决定，溢出不会被裁剪，元素框自动调整包含溢出内容。
更多可以看扩展阅读的链接。

<h3>DOCTYPE切换</h3>
浏览器根据不同的DOCTYPE选择不同的渲染方法就叫做DOCTYPE切换。 其实DOCTYPE切换就是用来识别和兼容旧网页的。
以下情况浏览器会采用标准模式渲染：


- 给出了完整的DOCTYPE声明
- DOCTYPE声明了Strict DTD
- DOCTYPE声明了Transitional DTD和URI
以下情况浏览器会采用混杂模式渲染：

DOCTYPE声明了Transitional DTD但未给出URI

- DOCTYPE声明不合法
- 未给出DOCTYPE声明
- 如果你是使用最新标准编写的页面但未给出DOCTYPE声明，这时就可能会出现一些怪异的行为。 例如盒模型不正确、窗口的size不正确等问题。所以，尽量为你网站的所有页面都给出合法的DOCTYPE声明。


**扩展阅读**
[怪异模式（Quirks Mode）对 HTML 页面的影响][2]
[What happens in Quirks Mode?][3]


  [1]: http://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/
  [2]: https://www.ibm.com/developerworks/cn/web/1310_shatao_quirks/#icomments
  [3]: https://www.cs.tut.fi/~jkorpela/quirks-mode.html
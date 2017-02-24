# DOM编程&&算法和流程控制

标签（空格分隔）： 高性能JavaScript

---
# DOM编程
浏览器中通常会把DOM和JavaScript独立实现。两个相互独立的功能只要通过接口彼此连接，就会产生消耗。所以DOM的访问和修改都是有代价的。

## DOM访问与修改
最小化DOM访问次数，尽可能在JavaScript端处理。修改DOM的时候，可以先局部变量存储修改的内容，最后 一次性写入。
```
function innerHTMLLoop(){
  for(var count = 0; count < 15000; count++){
    document.getElementById('here').innerHTML += 'a'
  }
}
```
这里每个元素都被访问两次，一次读取innerHTML,另一次重写它，优化后循环最后一次性写入。
```
function innerHTMLLoop2(){
  var content = '';
  for(var count = 0;count < 15000; count++){
    content +='a'
  }
  document.getElementById('here').innerHTML += content;
}
```
## innerHTML和DOM方法
修改页面区域使用 `innerHTML` 和类似 `document.createElement()`的原生DOM方法，性能相差无几。推荐使用`innerHTML`因为在绝大部分浏览器中都运行得更快，对日常操作而言，并没有太大区别。并且使用`element.cloneNode()`替代`document.createElement()`，更有效率，但是不是特别明显。
## HTML集合
HTML集合是包含DOM节点引用的类数组对象。因为它**实时**连系着底层文档，建议把集合的长度缓存到一个变量中，并在迭代中使用。如果需要经常操作集合，建议拷贝到一个数组中。
## 元素节点
DOM元素属性如`childNodes`,`firstChild`,`nextSibling`并不区分元素节点和其他类型节点，比如注释和文本节点(通常两个节点间的空格)。在某些情况下，只需访问元素节点，因此在循环中可能需要检查返回节点的类型并过滤非元素节点。这些类型检查和过滤其实是不必要的DOM操作。
现在大部分浏览器提供的API只返回元素节点，推荐使用这些API`querySelectorAll()`,返回NodeList(**不会返回HTML集合，因此不会对应实时的文档结构**)

## 重排何时发生
- 添加或删除可见的DOM元素
- 元素位置改变
- 元素尺寸改变
- 内容改变(文本改变)
- 页面渲染器初始化
- 浏览器窗口尺寸改变

## 最小化重绘和重排
- 合并最所有的改变然后一次处理。
比如添加元素的样式
```
var el = document.getElementById('mydiv');
el.style.cssText = 'border-left:1px;border-right:2px;padding:5px;'
```
- 修改CSS的class名称
```
var el = document.getElementById('mydiv');
el.className = 'active';
```
## 批量修改DOM
减少重绘和重排的次数，离线操作DOM树
三种基本方法使DOM脱离文档：

- 隐藏元素，设置属性`display:none`
- 在当前DOM外构建子树，再拷贝回文档(`document.createDocumentFragment`)
- 使用`cloneNode()`拷贝到一个脱离文档的节点，然后替换原始元素。

使用缓存，减少访问布局信息的次数。
## IE和:hover
有大量元素使用`:hover`会降低响应速度。所以元素很多时应该避免使用该效果。
## 事件委托
使用事件委托减少事件处理器的数量。
跨浏览器兼容的部分：
- 访问事件对象，并判断事件源
- 取消文档树中的冒泡
- 阻止默认行为


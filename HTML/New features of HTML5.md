# New features of HTML5

标签（空格分隔）： HTML

---

## 语义
使用语义化的标签，更恰当地描述页面的内容。HTML5新增的标签主要体现在如下：

- HTML结构
 - `<section>`,`<article>`,`<hgroup>`,`<header>`，`<footer>`
- 音频和视频元素
 - `<audio>`和`<video>`
- HTML表单
 - `<input>`属性type新增一些值，如`color`,`date`,`email`,`number`,`range`,`search`,`tel`,`url`等，更多可以[参考这里][1]
 - `<output>`定义一个用户的操作或者计算的结果。
- 新的语义元素
 - `<figure>`,`<figcaption>`,`<mark>`,`<progress>`,`<meter>`

## 通信
**1 Web Sockets**
允许在页面和服务器之间建立持久连接并通过方法来就换非HTML数据。
**客户端**
 1） 创建WebSocket连接到对应的服务器,赋值之后，ws.readyState会变成CONNECTING,建立并可传输数据，readyState会变成OPEN.
`var ws = new WebSocket("ws://www.example.com/socketserver")`
 2）发送数据到服务器，数据可以是字符串，也可以Blob和ArrayBuffer对象发送二进制数据。
`ws.send("Something you want to send to server!")`
3)四个事件

- onopen 发生在套接字建立连接
- onmessage 发生客户端收到服务器端的数据
- onerror 发生在通信出现错误的时候
- onclose  连接关闭的时候（关闭连接`ws.close()`）

**2 Server-sent events**
允许服务器向客户端推送事件，而不是仅在响应客户端请求时发送数据的传统模式。
**客户端**
1）初始化一个EventSource对象
`var evtSource = new EventSource("demo.php`);
2)监听消息

 - 监听从服务器发来却没有指定事件类型的消息（没有event字段）
  `evt.onmessage = function(e){}`
 - 监听其他类型的事件
  `evtSource.addEventListener("ping",function(e){});`
>注： 服务器端发送的响应内容应该使用值为"text/event-stream"的MIME类型.
##离线&存储
**1 离线检测**
 开发离线应用的第一步是要知道设备在线还是离线。HTML5为此定义了一个`navigator.onLine`属性，`true`表示设备能上网,`false`表示设备离线。两个事件：

- `online`:网络从离线变为在线
- `offline`:网络从在线变为离线

HTML5主要是`localStorage`和`sessionStorage`。说到缓存常常也会提到cookie，所以总结下三者。
 **2 `localStorage`和`sessionStorage`的区别**
 
 - 生命周期
`localStorage`只要不 clear或remove，数据会一直保存
`sessionStorage`生命周期为当前窗口或标签页，一旦窗口或标签页被永久关闭了，那么所有通过sessionStorage存储的数据也就被清空了。
 - 数据共享范围
 `localStorage`可以共享相同浏览器的不同页面(页面属于相同域名和端口)
`sessionStorage`无法共享不同页面和标签页间的sessionStorage信息。

>注：页面和标签页仅指顶级窗口，如果一个标签页包含多个iframe标签且他们属于同源页面，他们之间是可以共享sessionStorage的。
同源：协议，主机名和端口要一样

**3.关于Cookies**

- Session Cookies:暂时的Cookies文件,关闭后就会销毁
- Persistent  Cookies:会一直在浏览器中直至你删除它

**4.Web Storage和Cookies**
1) Cookies的容量限制在4KB,Web Storage可以去到5MB
2）Cookies 一旦设置了之后，每个 http 请求都会带上 Cookies，而 Web Storage 不会
3）在存储和读取信息的时候，Web Storage 比 Cookies 简单得多

##多媒体
1.`<audio>` 和 `<video>` 元素嵌入并支持新的多媒体内容的操作。
```
<audio controls="controls">
  Your browser does not support the <code>audio</code> element.
  <source src="foo.wav" type="audio/wav">
</audio>
```
2.使用Camera API
可以使用手机的摄像头拍照，将拍到的照片发送给当前网页。
[使用Camera API的demo，请戳这里][2]
## 图像
HTML5主要新添加了以下三种图像功能：
1）使用Canvas绘制图像
2）WebGL的3D图像功能
3）SVG，一个基于XML的可以直接嵌入到HTML中的矢量图像格式
## Web Workers API
一个worker是使用一个构造函数创建运行在另一个全局上下文中的JavaScript文件。**在主页面与 worker 之间传递的数据是通过拷贝，而不是共享来完成的。**
```
var worker = new Worker("my_task.js");
worker.postMessage("");
worker.onmessage = function (message) {
  var data = message.data;
    console.log(JSON.stringify(data));
	worker.terminate();
};
worker.onerror = function(error){
  console.log(error.filename,error.lineno,error.message);
}
```
上面主要有以下几个关键点：

- 新建一个worker对象，可以理解成新创建的工作线程在主线程的引用。
- `worker.postMessage()`与新建的工作线程通信，传入一个json对象。
- worker对象的onmessage事件。onmessage是在worker线程返回数据时回调函数执行.
- worker对象的onerror事件。当worker线程执行出错时，函数回调执行，error参数中封装了错误对象的文件名，出错行号，具体错误信息。

## [History API][3]
操纵浏览器的历史记录
```
//历史记录的后退
window.history.back();
//前进
window.history.forward();
//移动到指定的历史记录点(当前页面位置索引值为0，上一页-1，下一页1)
window.history.go(1)
//添加历史记录
var stateObj = { foo: "bar" };
history.pushState(stateObj, "page 2", "bar.html");
//修改历史记录，和添加历史记录类似replaceState();
//每当激活的历史记录发生变化时，都会触发popstate事件。
```
## DragDrop API
拖拽事件：

- 作用在被拖拽元素上
 - ondragstart事件：当拖拽元素被拖拽的时候触发 
 - ondragend事件： 当拖拽完成后触发的时间
- 作用在目标元素上
 -  ondragenter事件:当拖拽元素进入目标元素的时候触发
 -  ondragover事件：当拖拽元素在目标元素上移动的时候触发
 -  ondrop事件： 被拖拽的元素在目标元素上同时鼠标放开时触发

**DataTransfer对象**
拖拽事件周期会初始化一个DataTransfer对象，用于保存拖拽数据和交互信息。以下是常用的属性和方法：

- dropEffect: 拖拽交互类型,通常决定浏览器如何显示鼠标光标并控制拖放操作.常见的取值有copy,move,link和none
- effectAllowed: 指定允许的交互类型,可以取值:copy,move,link,copyLink,copyMove,limkMove, all, none默认为uninitialized(允许所有操作)
- setData(format, data): 以键值对设置数据,format通常为数据格式,如text,text/html
- getData(format): 获取设置的对应格式数据,format与setData()中一致
- clearData(format): 清除指定格式的数据
- setDragImage(imgElement, x, y): 设置自定义图标

一个完整的drag and drop流程通常包含以下几个步骤:

1.设置可拖拽目标.设置属性draggable="true"实现元素的可拖拽.
2.监听dragstart设置拖拽数据
3.为拖拽操作设置反馈图标(可选)
4.设置允许的拖放效果,如copy,move,link
5.设置拖放目标,默认情况下浏览器阻止所有的拖放操作,所以需要监听dragenter或者dragover取消浏览器默认行为使元素可拖放.
6.监听drop事件执行所需操作

> 注：Event.preventDefault()阻止默认的事件方法执行。在ondragover中一定要执行preventDefault(),否则ondrop事件不会被触发。另外，如果是从其他应用软件或文件中拖东西进来，尤其是图片的时候，默认动作是显示这个图片或是相关信息，并不是真的执行drop。此时需要用document的ondragover事件把它干掉

## 全屏API
- 激活全屏模式,调用下面这个脚本使视频全屏播放:
```
var elem = document.getElementById("myvideo");
if (elem.requestFullscreen) {
  elem.requestFullscreen();
} else if (elem.mozRequestFullScreen) {
  elem.mozRequestFullScreen();
} else if (elem.webkitRequestFullscreen) {
  elem.webkitRequestFullscreen();
}
```
- 退出全屏模式:  cancelFullscreen()
document提供额外一些关于全屏的信息：
- fullscreeenElement: 告诉你当前全屏显示的element。非空则进入全屏模式
- fullscreenEnabled: 告诉你是否进入了可以请求全屏模式的状态

## 设备方向
监听设备方向事件：
```
window.addEventListener('orientationChange', handleOrientation);
function handleOrientation(event) {
  var absolute = event.absolute;
  var alpha = event.alpha;//表示设备沿z轴上的旋转角度，范围0-360
  var beta = event.beta;//表示设备在X轴上的旋转角度，范围-180~180
  var gamma = event.gamma;//表示设备在y轴上的旋转角度，范围-90~90

}
```
## 地理位置API
地理位置 API 通过 navigator.geolocation 提供。
**获取当前定位** getCurrentPosition()函数，使用如下，第二个参数可选，当有错误时会执行的回调函数，第三个参数，你可以该对象参数设定最长可接受的定位返回时间、等待请求的时间和是否获取高精度定位。
```
navigator.geolocation.getCurrentPosition(success,[error],[PositionOptions ]);
```
**监视定位**可以通过`watchPosition()`实现响应定位数据发生的变更，和`getCurrentPosition()`接受相同的参数。
成功回调，回调函数会传入一个position参数，包含一个coords属性，coords的属性包含

- latitude 纬度
- longitude 经度
- altitude 海拔
- heading 度(顺时针，以正北为基准)
- speed 米/秒
`watchPosition()` 函数会返回一个 ID，唯一地标记该位置监视器。您可以将这个 ID 传给 clearWatch() 函数来停止监视用户位置。
```
navigator.geolocation.clearWatch(watchID);
```



  [1]: https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input
  [2]: https://developer.mozilla.org/zh-CN/docs/Web/Guide/API/Camera
  [3]: https://developer.mozilla.org/zh-CN/docs/DOM/Manipulating_the_browser_history
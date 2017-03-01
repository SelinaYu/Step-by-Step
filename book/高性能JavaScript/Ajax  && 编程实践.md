# Ajax  && 编程实践

标签（空格分隔）： 高性能JavaScript

---

# Ajax
## 请求数据
5种常用技术用于向服务器请求数据：

- XMLHttpRequest(XHR)
- Dynamic scripts tag insertion动态脚本注入
- iframes
- Comet
## Multipart XHR
客户端用一个HTTP请求就可以从服务端传递多个资源。它通过在服务端将资源（CSS文件，HTML片段，Javascript代码或者base64编码的图片）打包成一个由双方约定的字符串分割的长字符串，并发送到客户端。
下面是读取了三张图片，并把它们转换为base64编码的字符串，他们之间用一个简单的 Unicode 编码字符1连接，然后返回客户端。
```
function splitImages(imageString){
  var imageData = imageString.split('\u0001');
  var imageElement;

  for (var i =0, len = imageData.length; i<len; i++){
    imageElement = document.createElement('img');
    imageElement.src = 'data:image/jpeg;base64,' + imageData[i];
    document.getElementById('container').appendChild(imageElement);
  }
}
```
当MXHR响应消息的体积越大，有必要在每个资源收到时(监听readystate为3的状态)就立刻处理，而不是等到整个响应消息接收完成。MXHR不能缓存接收到的响应。

## 使用XHR时，POST和GET的对比
- GET请求会被缓存起来，往服务器只发送一个数据包
- POST至少发送两个数据包：一个装载头信息，一个POST正文，适合发大量数据到服务器。
注：IE对URL长度有限制，不可能使用过长的GET请求。

## 发送数据
当数据只需要发送到服务器时，有两种广泛的技术： XHR 和 Beacons.
Beacons 类似脚本注入
```
var url = "/status_tracker.php";
var params = [
  'step = 2',
  'time = 1248027314'
];
(new Image()).src = url + '?' params.join('&');
```
缺点：无法发送 POST 数据，URL的长度有限，接收到的响应类型有限。
优点： Beacons向服务器回传数据最快且最有效。服务器根本不需要发送任何响应正文。
# 编程实践
## 避免双重求值
在程序中提取一个包含代码的字符串，然后动态执行，这种实现有四种方法实现(避免使用)：`eval()`,`Function()`,`setTimeout()`和`setInterval()`
```
var num1 = 5,
    num2 = 6,
    //eval()执行代码字符串
    result = eval("num1 + num2"),
    //Function() 执行代码字符串
    sum = new Function("arg1","arg2","return arg1 + arg2")
    setTimeout("sum = sum1 + sum2",100);
    setInterval("sum = sum1 + sum2",100);
```
在JavaScript代码中执行另一端JavaScript代码，会导致双重求值的性能小号。
## 使用Object/Array直接量
使用直接量创建对象和数组，直接量会运行得更快，更节省代码。
```
//直接量
var myObject = {
  name: "Nicholas",
  count: 50,
  flag: true,
  pointer: null
}
//其他
var myObject = new Object();
myObject.name = "Nichaolas";
```
## 小结
- 避免重复的工作。当需要检测浏览器时，可使用延迟加载或条件预加载。
- JavaScript的原生方法总会比你写的任何代码要快。
- 进行数学计算时，考虑直接操作数字的二进制形式的位运算。

## 构建部署过程
- 合并JavaScript文件减少HTTP请求数
- 使用YUI Compressor压缩JavaScript文件
- 在服务端压缩文件(Gzip编码)
浏览器发送请求时，通常会发送一个 Accept-Encoding HTTP头告诉服务器支持哪种编码转换类型 ，其可取值： gzip、compress、deflate 和 identity。Gzip是目前最流行的编码方式，通常能减少70%的下载。Gzip压缩主要适用于文本。
- 正确设置HTTP响应头缓存JavaScript文件，避免向文件增加时间戳来避免缓存问题
- 使用CDN提供JavaScript文件，CDN不仅可以提升性能，也管理文件的压缩与缓存。
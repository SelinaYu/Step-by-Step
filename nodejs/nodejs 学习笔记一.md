# nodejs 学习笔记一

标签（空格分隔）： nodejs


---

**Node.js 有以下三部分组成**
1. 引入 required 模块
2. 创建服务器
3. 接收请求与响应请求
**Node.js REPL(Read Eval Print Loop: 交互式解释器)**
可以执行以下任务：

- 读取-读取用户输入，解析输入了Javascript数据结果并存储在内存中
- 执行-执行输入的内容
- 打印-输出结果
- 循环-循环操作以上步骤直到用户两次按下 ctrl-c 按钮退出。
我们可以进行一些的运算，并且可以使用下划线**_**变量获取表达式的**运算结果**
![1](http://7xq2ky.com1.z0.glb.clouddn.com/REPL_.png)

**REPL命令**

- ctrl + c :退出当前终端
- ctrl + c按下两次： 退出Node REPL
- ctrl + d: 退出Node REPL
- 向上/向下键：查看输入的历史命令行
- tab键： 列出当前命令
- .help： 列出使用命令
- .break: 退出多行表达式
- .clear: 退出多行表达式
- .save filename:保存当前的Node REPL会话到指定文件
- .load filename: 载入当前Node REPL会话的文件内容
**事件驱动程序**

Node.js 使用事件驱动模型，当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。

当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户。

这个模型非常高效可扩展性非常强，因为webserver一直接受请求而不等待任何读写操作。（这也被称之为非阻塞式IO或者事件驱动IO）

在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。
在 Node 应用程序中，执行异步操作的函数将回调函数作为最后一个参数， 回调函数接收错误对象作为第一个参数。

**Node.js EventEmitter**
events 模块只提供了一个对象： events.EventEmitter。EventEmitter 的核心就是事件触发与事件监听器功能的封装。

     EventEmitter会触发的事件：
     1. error事件：对象实例化时发生错误
     2. newListener:添加新的监听器时
     3. removeListener:监听器被移除时
大多数时候我们不会直接使用 EventEmitter，而是在对象中继承它。包括 fs、net、 http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的子类。

为什么要这样做呢？原因有两点：
 
首先，具有某个实体功能的对象实现事件符合语义， 事件的监听和发射应该是一个对象的方法。

其次 JavaScript 的对象机制是基于原型的，支持 部分多重继承，继承 EventEmitter 不会打乱对象原有的继承关系。
 **模块**
 在编写每个模块时，都有`require`、`exports`、`module`三个预先定义好的变量可供使用。
**require**
`require`函数用于在当前模块中加载和使用别的模块，传入一个模块名，返回一个模块导出对象。模块名可使用相对路径（以`./`开头），或者是绝对路径（以/或C:之类的盘符开头）。另外，模块名中的`.js`扩展名可以省略。模块名中.json扩展名不可省略。
**exports**
`exports`对象是当前模块的导出对象，用于导出模块公有方法和属性。别的模块通过require函数使用当前模块时得到的就是当前模块的exports对象。
**module**
通过`module`对象可以访问到当前模块的一些相关信息，但最多的用途是替换当前模块的导出对象。
**模块初始化**
一个模块中的JS代码仅在模块第一次被使用时执行一次，并在执行过程中初始化模块的导出对象。之后，缓存起来的导出对象被重复利用。
**package.json**
如果想自定义入口模块的文件名和存放位置，就需要在包目录下包含一个`package.json`文件，并在其中指定入口模块的路径。package.json里必要的字段如下。
```
{
    "name": "file-explorer",           # 包名，在NPM服务器上须要保持唯一
    "version": "1.0.0",            # 当前版本号
    "dependencies": {              # 三方包依赖，需要指定包名和版本号
        "argv": "0.0.2"
      },
    "main": "./lib/index.js",       # 入口模块位置
    "bin" : {
        "node-echo": "./bin/node-echo"      # 命令行程序名和主模块位置
  
```
**工程目录**
一个标准的工程目录应如下：

    - /home/user/workspace/node-echo/   # 工程目录
    + bin/                          # 存放命令行相关代码
    + doc/                          # 存放文档
    +lib/                          # 存放API相关代码
    - node_modules/                 # 存放三方包
        + argv/
    + tests/                        # 存放测试用例
    package.json                    # 元数据文件
    README.md                       # 说明文件
**版本号**
使用NPM下载和发布代码时都会接触到版本号。语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新：

    如果只是修复bug，需要更新Z位。
    如果是新增了功能，但是向下兼容，需要更新Y位。
    如果有大变动，向下不兼容，需要更新X位。
 **process全局对象**
 process全局对象中包含了三个流对象，分别对应三个UNIX标准流：
 - **stdin** : 标准输入
 - **stdout** : 标准输出
 - **stderr** : 标准错误
 第一个是可读流，`stdout`和`stderr`是可写流。
当涉及持续不断地对数据进行读写时，流就出现了。

     1. fs模块是唯一一个同时提供同步和异步API的模块
     2. __dirname来获取执行文件时该文件在文件系统中所在的目录
     3. process.cwd()获取当前工作目录

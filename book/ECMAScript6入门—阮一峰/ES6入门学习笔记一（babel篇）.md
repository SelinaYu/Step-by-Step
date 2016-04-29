# ES6入门学习笔记一（babel篇）

标签（空格分隔）： ES6

---

<h3>Babel转码器</h3>
Babel转码器可以将ES6代码转为ES5代码，从而支持现有环境执行。使用Babel需要配置它的配置文件`.babelrc`。基本格式如下：
```
{
"presets":[],
"plugins":[]
}
```
`presets`字段设定转码规则，官方提供如下的规则集，可以根据需要安装：
ES2015转码规则
> npm install --save-dev babel-preset-es2015
react转码规则
>npm install --save-dev babel-preset-react

然后将这些加入`.babelrc`
```
{
"presets":[
"es2015",
"react"
],
"plugins":[]}
```
**直接运行ES6脚本**
> babel-node es6.js

**把ES6转码成ES5,须先安装插件,在执行babel命令**
> npm install babel-preset-es2015
> babel es6.js --presets es2015

**babel-register**

使用`babel-register`模块改写`require`。此后每当使用`require`加载`.js`,`.jsx`,`.es`和`.es6`后缀名的文件，就会先用Babel转码。因为是实时转码的，只是用在开发环境中使用。

**babel-core**
如果某些代码需要调用那个Babel的API进行转码，就要使用`babel-core`模块。如下则需要通过输入`npm install babel-core --save`安装并且引入`babel-core`
```
var es6Code = 'let x = n => n + 1';
var es5Code = require('babel-core')
  .transform(es6Code, {
    presets: ['es2015']
  })
  .code;
```
**babel-polyfill**
Babel只转换新的JavaScript句法，而不转换新的API或全局对象或定义在全局对象的方法（`Array.from`）则需要使用`babel-polyfill`。用法同上，安装并引入。
**浏览器环境**I
Babel从6.0开始，不在提供浏览器版本，而是通过构建工具构建出来。如果没有或者不想使用构建工具，可以安装5.x版本的`babel-core`模块获取。
> npm install babel-core@5

运行上面的命令以后，就可以在当前目录的 `node_modules/babel-core/`子目录里面，找到`babel`的浏览器版本`browser.js`（未精简）和`browser.min.js`（已精简）。然后，将代码插入网页。`browser.js`是`Babel`提供的转换器脚本，用户的ES6脚本放在注明`type="text/babel"`的`script`标签中。
下面是将代码打包成浏览器可以使用的脚本，以`babel`配合`Browserify`为例。
1)安装babelify模块
> npm install --save-dev babelify babel-preset-es2015

2)用命令行转换ES6脚本
> browserify script.js -o bundle.js \
  -t [ babelify --presets [ es2015 react ] ]
  
  上面代码将ES6脚本script.js，转为bundle.js，浏览器直接加载后者就可以了。
  在package.json设置下面的代码，就不用每次命令行都输入参数了。
  
```
    {
       "browserify": {
            "transform": [["babelify", { "presets": ["es2015"] }]]
    }
}
```
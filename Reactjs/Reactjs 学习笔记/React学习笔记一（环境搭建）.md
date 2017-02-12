# React学习笔记一（环境搭建）

标签（空格分隔）： reactjs

---
一开始使用sublime写jsx语法，里面的标签高亮不正确，解决办法如下：
sublime配置react开发环境，使用babel插件
在菜单栏中，选择
>view>syntax>open all with current extension>babel>javascript

    
<h3>搭建JSX使用环境</h3>
1. 把jsx代码单独成一个文件
2. 在html里面引入jsx代码,注意`type="text/babel"`
`<script type="text/babel" src="XX.js"></script>`
请注意，某些浏览器（如，Chrome浏览器）将无法加载该文件，除非它通过HTTP服务
**离线转换**
<h3>利用bower使用react</h3>
1.安装`babel`命令行
    
>npm install --global babel-cli
>npm install babel-preset-react
注意：如果你使用ES2015,还需要`babel-preset-es2015`这个包

2.把你的jsx代码转换成标准的js,而且当你修改jsx代码时会自动更新你的js代码,src为放置jsx的文件夹，build为转换后文件放置的位置。
>babel --presets react src --watch --out-dir build


<h3>利用npm使用react</h3>

1.  新建一个js文件如main.js，通过`require`引入`react`和`react-dom`
2.  写babel配置文件
基本格式如下(presets字段设定转码规则)：

        {
         "presets":["react","es2015"],
         "plugins":[]
        }

3. 第三步有两种方法
**方法1 使用`browserify`安装`ReactDOM`**
   
        npm install --save react react-dom babelify babel-preset-react
        browserify -t [ babelify ] main.js -o bundle.js
**方法2 使用`webpack`安装`ReactDOM`**

        npm install --save react react-dom babel-preset-react babel-loader babel-core
        webpack main.js bundle.js --module-bind 'js=babel-loader'
        
默认情况下，React将在开发模式，但是这很慢不建议用在上线的产品，所以要添加环境变量`NODE_ENV`生产，代码如下
```
new webpack.DefinePlugin({
  "process.env": {
    NODE_ENV: JSON.stringify("production")
  }
})
```
总结：
1. React可以渲染HTML标签和React组件，HTML标签，只需使用小写字母开头的标签名；React组件，只需创建一个大写字母开头的本地变量。
2. 由于JSX就是JavaScript,一些标识符像`class`和`for`不建议作为XML属性名。作为替代，ReactDOM使用`className`和`htmlFor`来做对应的属性。
3. 要使用JavaScript表达式作为属性值，只需要把这个表达式用`{}`,不要使用引号`""`
4. 修改`props`是不好的，因为react不能帮你检查属性类型，这样子即使你的属性类型有错误也不能得到清晰的错误提示。`props`应当是被当作禁止修改的。






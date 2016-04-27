# es6-webpack-reactjs

标签（空格分隔）： reactjs

---

<h1>使用webpack配置reactjs</h1>
1） 在`MyApp`文件夹里,执行`npm init`创建`package.json`。
2） 输入`npm install --save-dev webpack`在项目中安装`webpack`。
3） 在`MyApp`里新建webpack的配置文件`webpack.config.js`,添加如下代码
```
var config = {
	entry: "app/main.js",
	output:{
		filename:"bundle.js",
		path:__dirname+"dist"
	}
}
module.exports = config;
```
4）建立如下文件目录：

 - MyApp
   - app
     - main.js
   - dist
     - index.html
   - webpack.config.json
   - package.json
   
5） 在`main.js`写你的入口文件。在`index.html`添加如下：
```
<!Doctype html>
<html>
	<head>
		<title>Hello World</title>
		<meta charset="utf-8">
	</head>
	<body>
		<div id="content"></div>
		<script src="bundle.js"></script>
	</body>
</html>
```
6） 输入`npm install --save react react-dom`安装React
7)  要支持 jsx和ES6语法还需安装`npm intall --save-dev babel-preset-react babel-preset-es2015 babel-loader`
8)在`webpack.config.js`添加如下`loader`
```
    module:{
    	loaders:[
	{
		test:/\.js$/,
		exclude:/node_modules/,
		loader:'babel-loader',
		query:{
			presets:['react','es2015']
		}
	}
	],
    }
```
9)执行`webpack`,打开`dist/index.html`








# ES6入门学习笔记一

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





# Reactjs学习笔记四

标签（空格分隔）： reactjs

---
<h3>Reactjs处理三种不同情况的两个节点的差异：</h3>
情况一：不同的节点类型
React会把它们当作不同的子树，甚至不会判断他们渲染来了什么就直接移除原来的DOM树然后创建并插入新的DOM
情况二：DOM节点
比较两个DOM节点的时候，我们查看两者的属性。对style使用键值对象改变样式属性。
情况三：自定义组件
我们定义两个自定义组件是相同的，因为组件是有状态的，我们不可能每次状态改变状态都要创建一个新的组件实例。React利用新组件的属性并调用`componentWillReciveProps()`和`componentWillUpdate()`更新组件。
<h3>List-wise diff(子级优化差异算法)</h3>
React更新子级是通过同时更新遍历两个子级列表，当发生差异的时候就进行修改。
1)给每个子级添加一个可选属性key，React就可以检出节点插入，删除，修改，并借助哈希表使节点复杂度为O（n）。记住，键值仅需要在兄弟节点中唯一，而不是全局唯一。
2)当前能检测某个子级树从它的兄弟节点移除，但不能检出他是否移到了其他的地方，当前算法将会重新渲染整个子树。Key值应该是稳定的，不能稳定的Key会在每次数据更新中重新渲染。
<h3>得到一个ReactComponent实例</h3>
```
var MyComponent = React.createClass({
    render:function(){
    }
})
```
注意：不要自己使用new调用该构造函数。
方法一： 
    
    var element = React.createElement(MyComponent);

方法二：使用JSX,当实例传给`React.render`,React会调用构造函数，然后创建并返回一个ReactComponent。

    var element = <MyComponent />
    var component = React.render(element,document.body);
<h3>关于flux</h3>
Flux是Facebook用来构建用户端的Web应用的应用程序体系架构。
Flux应用主要包括三部分：**dispactcher,store和views。**

**dispactcher**：本质就是store callback的注册表。它就是一个用来向stores分发actions的机器。 当dispatcher发出一个action是，应用中所有的store都会通过注册的callback收到action。

**Stores**的典型特征就是models的集合和所属业务域下的model实例。store响应了dispactcher发出的action后，会向应用中广播一个change事件，views可以选择响应事件获取新的数据并更新。

**views**当它接收到`store`的广播后，它首先会通过`store`的公共`getter`方法来获取所需的数据，然后调用自身的`setState()`或`forceUpdate()`方法来促进自身和后代的重新渲染。
<h3>React Tip</h3>
1)行内样式
在React中，行内样式并不是以字符串的形式出现，而是通过一个特定的样式对象来指定。key值是用驼峰形式表示的样式名（浏览器前缀除了ms以外 首字母应该大写。），而其对应的值则是样式值，通常来说是字符串。
2)不能在JSX语法中使用`if~else`语句（但是你可以先判断赋值给变量），或者使用三元操作式
3）所有的标签都必须闭合，可以是自闭和的形式，也可以是常规的闭合。
4）render只能返回一个节点
5）在React设置input的value为null时，input就会未经允许就改变。
6）节点初次被放入的时候`componentWillReceiveProps`并不会触发。
    


    





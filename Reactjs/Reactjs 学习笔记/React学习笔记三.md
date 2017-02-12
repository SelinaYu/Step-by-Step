# React学习笔记三

标签（空格分隔）： reactjs

---
**<h3> 要点一：props的传递,使用解构赋值中的剩余属性特性来把未知属性批量提取出来，并传递下去。</h3>**

 我们大部分情况下是显式传递props,但是并没有把所有属性都传下去。所以，我们可以利用**解构赋值**。
 列出所有要当前使用的属性，后面跟着`...other`。
 
      var {checked,...other} = this.props;
其中`checked`为已知属性，在组件中添加`{...other}`可以把那些未知属性传递下去。`{...this.props}`会把所有属性都传递下去。
**<h3>要点二：传递未知属性和已经使用过的属性</h3>**

要点一提到的只是传递那些未使用的，如果要把使用过的属性继续往下传，可以直接在组件中添加该属性。如上面的`checked`属性可以`checked={checked}`。
**<h3>要点三：select组件选中多项</h3>**

在`<select>`组件中，给value属性传递一个数组，可以选中多个选项。

    <select multiple={true} value={['A','B']}>。
**<h3>要点四：虚拟事件对象</h3>**

事件处理器将会传入虚拟事件对象的实例。一个浏览器本地事件的跨浏览器封装。**一般情况下是在事件冒泡阶段触发，如要在捕获阶段触发，需要在事件名字后追加`Capture`,如`onClickCapture`。**
**<h3>要点五：使用ES6创建class</h3>**

```
class HelloMessage extends React.Component{
    constructor(props){
        super(props);
        this.state = {count:props.initialCount};
        this.tick = this.tick.bind(this);
    }
    tick(){}
    render(){
        return <div>Hello {this.props.name}</div>
    }
}
```

原本通过`React.createClass`创建的`Component/Element`中的成员函数都是有自动绑定的能力，但是在使用ES6 Class则需要自己绑定，或者使用`=>`的声明方式。`=>`会将当前`this`绑定到当前类实例，但是不要对`render`使用`=>`。例如：
```
     <div onClick={this.tick.bind(this)}>
     //或者
     <div onClick={() => this.tick()}>
```
但是我们通常在`constructor`中绑定你的事件，这样他们就能在每一个实例上绑定一次。但是，ES6不支持`mixins`。
**要点六：Stateless Functions Components(无状态的函数式组件)**
对于简单的无状态的组件（只有render函数）,提供了更简单的语法声明。
方法一：直接使用普通的js函数。
```
function HelloMessage(props) {
  return <div>Hello {props.name}</div>;
}
ReactDOM.render(<HelloMessage name="Sebastian" />, mountNode);
```
方法二：使用ES6语法
```
const HelloMessage = (props) => <div>Hello {props.name}</div>
React.render(<HelloMessage name="Sebastian")/>,mountNode);
```
注意：
1. 这些组件必须不保留内部状态，不能含有实例，不能声明生命周期。但是，你们可以在ES6中指定`.proptypes`和`.defaultprops`。
2. 如果用户想在`stateless function component`查找DOM节点，必须要包一层含有`ref`的`stateful component`(就是使用**要点五**的创建的组件)

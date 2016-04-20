# React学习笔记二

标签（空格分隔）： reactjs

---

**<h3>JSX的新特性延展属性</h3>**

这里涉及到操作符`...`，表示将某些数据结构转为数组，已经被ES6支持.

     var props = {};
     props.foo = x;
     props.bar = y;
     var component = <Component {...props} />
     
 **Tip:**  后面的属性会覆盖前面的
 **<h3>JSX陷阱</h3>**
 
 HTML实体插入到JSX的文本中，因为React默认会转义所有字符串，为了防止各种XSS（跨站脚本攻击）攻击。
 方法如下：
 1. 直接使用Unicode字符，同时确保文件UTF-8编码且网页也指定为UTF-8编码
 2. 找到[实体的Unicode编号][1]，然后再JavaScript字符串里使用。
 
    <div>{'First \u00b7 Second'}</div>
    <div>{'First ' + String.fromCharCode(183) + ' Second'}</div>
3. 在数组里面混合使用字符串和JSX元素
    
    <div>{['First',<span>&middot;</span>,'Second']}</div>
4. 实在没办法，就直接使用原始HTML
   
     <div dangerouslySetInnerHTML={{__html:'First &middot; Second'}} />

[1]: http://www.fileformat.info/info/unicode/char/b7/index.htm
  **<h3>关于State</h3>**
  
 1. State应该包括那些可能被组件的事件处理器改变并触发用户界面更新的数据
 2. `this.state`应该仅包括用户界面状态所需的最少数据。所以它不应该包括**计算所得数据，React组件，基于props的重复数据**。
 
**<h3>动态子级</h3>**

如果子组件位置会改变或有新组件添加到列表开头，子级要在多个渲染阶段保持自己的特征和状态，可以通过以下两种方式给子级设置唯一标识区分。
方式一：使用`key`
当React校正带有`key`的子级时，它会确保它们被重新排序或删除而不是破坏和重用。**务必把key添加到子级数组里组件身上，而不是每个子级内部最外层HTML上**
```
  render: function() {
    var results = this.props.results;
    return (
      <ol>
        {results.map(function(result) {
          return <li key={result.id}>{result.text}</li>;
        })}
      </ol>
    );
  }
```
方式二：使用`object`来做`key`的子级。`object`的`key`会被当作每个组件的`key`。牢记JavaScript并不总是保证属性的顺序会被保留。数字型属性会按大小排序并排在其它属性前面，一旦发生这种情况，React渲染组件的顺序就会混乱，可以在key前面加一个字符串前缀来避免：
```
render: function() {
    var items = {};

    this.props.results.forEach(function(result) {
      // 如果 result.id 看起来是一个数字（比如短哈希），那么
      // 对象字面量的顺序就得不到保证。这种情况下，需要添加前缀
      // 来确保 key 是字符串。
      items['result-' + result.id] = <li>{result.text}</li>;
    });

    return (
      <ol>
        {items}
      </ol>
    );
  }
```
  **Tip:**  
  1. 如果往原生HTML元素传入HTML规范里不存在的元素，React不会显示它们。如果需要自定义属性，要添加`data-`前缀，同时，`aria-`开头的[网络无障碍]可以正常使用。
  2. 如果在手机或平板等触摸设备上使用React,需要调用`React.initializeTouchEvents(true);`启动触摸事件处理
  3. 我们可以重写`shouldComponentUpdate()`返回false来让React跳过子树的处理。如果数据变化时让`shouldComponentUpdate()`返回false,React就不能保证用户界面同步。当使用它的时候一定确保你清楚到底做了什么，并且只在遇到明显性能问题的时候才使用它。不要低估 JavaScript 的速度，DOM 操作通常才是慢的原因。

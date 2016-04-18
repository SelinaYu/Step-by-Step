# React学习笔记二

标签（空格分隔）： reactjs

---

<h3>JSX的新特性延展属性</h3>
这里涉及到操作符`...`，表示将某些数据结构转为数组，已经被ES6支持.

     var props = {};
     props.foo = x;
     props.bar = y;
     var component = <Component {...props} />
     
 **Tip:**  后面的属性会覆盖前面的
 <h3>JSX陷阱</h3>
 
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
  
  **Tip:**  
  1. 如果往原生HTML元素传入HTML规范里不存在的元素，React不会显示它们。如果需要自定义属性，要添加`data-`前缀，同时，`aria-`开头的[网络无障碍]可以正常使用。

未完待续。。。。

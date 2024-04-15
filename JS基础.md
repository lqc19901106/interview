[## ES6基础
### 1. var、let和const的区别

](https://github.com/axuebin/articles/issues/39

-----
## JS 基础
1. [js基础] 数据类型
    - number（基本数据类型）
    - boolean（基本数据类型）
    - string（基本数据类型）
    - null（基本数据类型）
    - undefined（基本数据类型）
    - symbol（es6新增）
    - Date（引用类型）
    - Array（引用类型）
    - Object（引用类型）
    - Function（引用类型）
  ```
  - 1. 封装 classof() 方法获取数据类型
    function classof(o){
      return Object.prototype.toString.call(o).slice(8,-1);       
    }
    /*
    Object.prototype.toString.call(window).slice(8,-1)
    'Window'
    Object.prototype.toString.call(null).slice(8,-1)
    'Null'
    Object.prototype.toString.call(undefined).slice(8,-1)
    'Undefined'
    Object.prototype.toString.call(function() {}).slice(8,-1)
    'Function'
    Object.prototype.toString.call(new Date()).slice(8,-1)
    'Date'
    Object.prototype.toString.call(/^abc/ig).slice(8,-1)
    'RegExp'
    */
  ```
  
2. [js基础] 变量提升
> JavaScript 代码执行过程中，JavaScript 引擎把变量的声明部分和函数的声明部分提升到代码开头的“行为”。变量被提升后，会给变量设置默认值，这个默认值就是我们熟悉的 undefined。

```

```
6. [js基础] 执行上下文
   
7. [js基础] 原型链和作用域链
   
8. [js基础] 闭包
   
9. [js基础] this指向、call/apply/bind的手动实现

10. [js基础] 防抖和节流
  /*
  * 防抖函数
  * 按钮提交场景：防止多次提交按钮，只执行最后提交的一次
  * 服务端验证场景：表单验证需要服务端配合，只执行一段连续的输入事件的最后一次，还有搜索联想词功能类似
  * 也可以直接只用lodash.debounce代替
  */ 

  const debounce = (fn, delay) => {
    let timer = null;
    return (...args) => {
      clearTimeout(timer);
      timer = setTimeout(() => {
        fn.apply(this, args);
      }, delay);
    };
  };

  /**
  * 节流函数
  * 拖拽场景：固定时间内只执行一次，防止超高频次触发位置变动
  * 缩放场景：监控浏览器resize
  * 动画场景：避免短时间内多次触发动画引起性能问题
  * 也可以直接只用lodash.throttle代替
  */ 
  const throttle = (fn, delay = 500) => {
    let flag = true;
    return (...args) => {
      if (!flag) return;
      flag = false;
      setTimeout(() => {
        fn.apply(this, args);
        flag = true;
      }, delay);
    };
  };

    
11. [js基础] 深拷贝、浅拷贝
```
// 浅拷贝
1. object.assign
2. {...obj}

// 深拷贝
1. JSON.parse(JSON.stringify(obj))

```

12. JSON.parse JSON.stringify
```
1、 利用 eval('')


```


)

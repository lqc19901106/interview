> https://juejin.cn/post/6940945178899251230

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
     <b><details><summary>1. 面试题：封装 classof() 方法获取数据类型</summary></b>
     
     ```
     function classof(o){
     return Object.prototype.toString.call(o).slice(8,-1);       
     }
     /*
     classof(window)      // 'Window'
     classof(null)        // 'Null'
     classof(Undefined)   // 'Undefined'
     classof(new Date())   // 'Date'
     classof(function() {})   // 'Function'
     classof(/^abc/ig)   // 'RegExp'
     ```
     
     </details>

<b><details><summary>2. null和undefined的区别</summary></b>

- null 表示空对象，表示不应该有值的存在

- undefined 表示不存在
  
  NULL：
  ①用作函数的参数，表示该函数的参数不是对象。
  ②用作对象原型链的终点
  
  undefined：
  ①函数没有返回值时，默认返回undefined。
  ②变量已声明，没有赋值时，为undefined。
  ③对象中没有赋值的属性，该属性的值为undefined。
  ④调用函数时，应该提供的参数没有提供，该参数等于undefined。
  
  </details>

<b><details><summary>3. `==` 和 `===`的区别</summary></b>
`==` 在执行比较之前将变量值转换为相同的类型。这称为类型强制。
`===` 不进行任何类型转换（强制），并且仅当被比较的两个变量的值和类型都相同时才返回true。

</details>

1. [js基础] 闭包
* 有权访问另一个函数作用域中变量的函数 *
  作用：
  
  - 函数外部访问函数内部的变量
  
  - 保留对函数变量的引用，不被内存回收
    <b><details><summary>1. 输出结果</summary></b>
    
    ```
    for(var i = 0; i < 5; i++) {
    setTimeout(function() {
    console.log(i);
    }, 1);
    }
    // 5、5、5、5、5
    ```
    
    </details>
    <b><details><summary>2. 下面代码输出结果</summary></b>
    ```
    var data = [];

for (var i = 0; i < 3; i++) {
  data[i] = (function (i) {
    return function(){
      console.log(i);
    }
  })(i);
}

data[0](); // 3
data[1](); // 3
data[2](); //3

```
</details>
4. [js基础] 作用域和作用域链
5. [js基础] 执行上下文
6. [js基础] 继承


1. [js基础] 变量提升和函数提升
> 变量提升，JavaScript 引擎把变量的声明部分和函数的声明部分提升到代码开头的“行为”。变量提升只能提升声明，不能提升赋值

> 函数提升：在JavaScript中，函数声明会被提升（hoisted）到其作用域的顶部，这意味着你可以在声明之前调用函数。这种行为适用于函数声明，但不适用于函数表达式。

> 函数提升高于变量提升: 进入执行上下文后，先处理函数提升在处理，变量提升
```

greet(); // 正常工作，因为函数声明被提升了

function greet() {
  console.log('Hello, World!');
}

```
4. 防抖和节流
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


5. [js基础] 深拷贝、浅拷贝
```

// 浅拷贝

1. object.assign
2. {...obj}

// 深拷贝

1. JSON.parse(JSON.stringify(obj))
2. 

```
6. JSON.parse JSON.stringify
```

1、 利用 eval('')

```
)
```

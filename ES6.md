> https://es6.ruanyifeng.com/

```ES6
1. ES6引入来严格模式
    变量必须声明后在使用
    函数的参数不能有同名属性, 否则报错
    不能使用with语句
    不能对只读属性赋值, 否则报错
    不能使用前缀0表示八进制数,否则报错
    不能删除不可删除的数据, 否则报错
    不能删除变量delete prop, 会报错, 只能删除属性delete global[prop]
    eval不会在它的外层作用域引入变量
    eval和arguments不能被重新赋值
    arguments不会自动反映函数参数的变化
    不能使用arguments.caller
    不能使用arguments.callee
    禁止this指向全局对象
    不能使用fn.caller和fn.arguments获取函数调用的堆栈 
    增加了保留字（比如protected、static和interface）

2. 关于let和const新增的变量声明

3. 变量的解构赋值

4. 字符串的扩展
    includes()：返回布尔值，表示是否找到了参数字符串。
    startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
    endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
5. 数值的扩展
    Number.isFinite()用来检查一个数值是否为有限的（finite）。
    Number.isNaN()用来检查一个值是否为NaN。
6. 函数的扩展
    函数参数指定默认值
7. 数组的扩展
    扩展运算符
8. 对象的扩展
    对象的解构
9. 新增symbol数据类型

10. Set 和 Map 数据结构 
    ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。 Set 本身是一个构造函数，用来生成 Set 数据结构。

    Map它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。
11. Proxy
    Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问
    都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。
    Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
    Vue3.0使用了proxy
12. Promise
    Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。
    特点是：
        对象的状态不受外界影响。
        一旦状态改变，就不会再变，任何时候都可以得到这个结果。
13. async 函数 
    async函数对 Generator 函数的区别：
    （1）内置执行器。
    Generator 函数的执行必须靠执行器，而async函数自带执行器。也就是说，async函数的执行，与普通函数一模一样，只要一行。
    （2）更好的语义。
    async和await，比起星号和yield，语义更清楚了。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。
    （3）正常情况下，await命令后面是一个 Promise 对象。如果不是，会被转成一个立即resolve的 Promise 对象。
    （4）返回值是 Promise。
    async函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用then方法指定下一步的操作。
14. Class 
    class跟let、const一样：不存在变量提升、不能重复声明...
    ES6 的class可以看作只是一个语法糖，它的绝大部分功能
    ES5 都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。
15. Module
    ES6 的模块自动采用严格模式，不管你有没有在模块头部加上"use strict";。
    import和export命令以及export和export default的区别
```

#### 面试题 1. var、let和const的区别?

参考答案：

    - 变量提升：暂时性死区
        - var 存在变量提升 可以在声明前调用 值为undefined
        - let const 不存在变量提升 所声明的变量都要在声明后使用
        ```
        // var
        console.log(a)  // undefined
        var a = 10
    
        // let 
        console.log(b)  // Cannot access 'b' before initialization
        let b = 10
    
        // const
        console.log(c)  // Cannot access 'c' before initialization
        const c = 10
        ```

- 重复声明
  - var 可以重复声明不会报错
  - let、const重复声明会报错
- 块级作用域
  - var 没有
  - let、const 存在块级作用域
- 修改变量
  - var、let可以修改变量
  - const 声明之后不能修改，const不能声明为null的变量、引用类型的属性值可以修改
    
    </details>

#### 面试题 2. 箭头函数和普通函数的区别

参考：

- 1、函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。
- 2、不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
- 3、不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。
- 4、不可以使用 new 命令，因为：
   没有自己的 this，无法调用 call，apply。
   没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 __proto__
  
  </details>

#### 面试题 3. [ES6] Map、Set、WeakMap、WeakSet的使用区别

```
> https://www.cnblogs.com/jaetyn/p/16410225.html
### Map和WeakMap
- 键类型的限制：

Map： 键可以是任意类型的值，包括基本类型和对象引用。
WeakMap： 键只能是对象引用。这是因为WeakMap的键是弱引用，不会阻止对象被垃圾回收，这使得WeakMap适合用于存储对象之间的关联信息。
垃圾回收机制：

Map： 如果某个键不再被引用，但它仍然会被Map引用，因此不会自动被垃圾回收。
WeakMap： 如果某个键不再被引用，它会被自动从WeakMap中删除，这有助于避免内存泄漏。

- 迭代能力：

Map： 可以使用Map.prototype.keys()、Map.prototype.values()和Map.prototype.entries()等方法来迭代Map中的键、值或键值对。
WeakMap： 由于其键是弱引用，不可直接迭代键或值，因此WeakMap对象是不可枚举的，无法获取大小。

- 功能差异：
Map： 提供了更多的功能，例如可以获取Map的大小（使用Map.prototype.size属性），可以通过键获取值（使用Map.prototype.get()方法），可以遍历Map中的键值对等。

Set和WeakSet
Set 和 WeakSet 的区别主要体现在以下几个方面：

- 成员类型：

Set 可以包含任何类型的值，包括基本类型和对象。
WeakSet 只能包含对象，且成员必须是弱引用。
-弱引用特性：

WeakSet 中的对象是弱引用的，这意味着如果没有其他强引用指向这些对象，它们将被垃圾回收机制回收。这有助于防止内存泄漏，尤其是当存储的对象是DOM节点或其他可能在页面卸载时不再需要的资源时。
不可遍历和没有size属性：

WeakSet 不支持使用for...of循环进行遍历，也没有size属性来获取集合中成员的数量。这是因为其成员是弱引用，状态可能随时变化，因此无法保证遍历结果的准确性。
- 使用场景：

由于WeakSet的弱引用特性和不可遍历的特性，它非常适合用于存储不需要强制保持活动状态的对象，如DOM节点，同时避免内存泄漏的风险。
总结来说，Set和WeakSet的主要区别在于成员的类型、弱引用处理以及是否可遍历等方面。WeakSet设计用于存储不需要强制保持活动状态的对象，有助于管理内存使用，避免不必要的内存消耗。
```
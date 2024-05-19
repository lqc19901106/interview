>[requestIdleCallback]([docs/[Vue] VirtualDOM原理.md](https://blog.csdn.net/qq_41370833/article/details/134038243))
Vue的虚拟DOM（Virtual DOM）是基于snabbdom的修改版。它的核心是通过JavaScript对象描述DOM树，并与实际的DOM进行对比更新。

初始化：Vue实例开始时，会将模板编译成渲染函数，并在内存中生成一个代表DOM树的JavaScript对象（VNode）。

渲染：使用渲染函数将VNode渲染到实际DOM。

更新：当数据变化时，Vue会重新调用渲染函数生成新的VNode，然后Vue会 diff 这两个VNode，找出最小的差异进行DOM更新。

下面是一个简化的例子，演示Vue中的虚拟DOM更新过程：


```
// 假设有一个简单的Vue模板
<template>
  <div>{{ message }}</div>
</template>
 
// JavaScript部分
new Vue({
  el: '#app',
  data: {
    message: 'Hello, Vue!'
  }
});
 
// 编译和渲染过程
// 1. Vue模板被编译成渲染函数。
// 2. 渲染函数被调用，生成初始的VNode。
// 3. VNode被渲染到实际DOM中。
 
// 数据更新过程
// 1. 用户更新data.message。
// 2. Vue重新调用渲染函数生成新的VNode。
// 3. Vue比较新旧VNode树的差异，并最小化更新实际DOM。
```
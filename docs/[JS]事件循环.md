> https://github.com/Jacky-Summer/personal-blog/blob/master/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3JavaScript%E7%B3%BB%E5%88%97/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%20JavaScript%20%E4%B9%8B%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF(Event%20Loop).md

----- 

### js 如何处理异步任务

JS 是单线程，那么非阻塞怎么体现呢？如果 JS 是阻塞的，那么 JS 发起一个异步 IO 请求，在等待结果返回的这个时间段，后面的代码就无法执行了，而 JS 主线程和渲染进程是互斥的，因此可能造成浏览器假死的状态。事实 JS 是非阻塞的，那它要怎么实现异步任务呢，靠的就是事件循环。

### 事件循环

事件循环就是通过异步执行任务的方法来解决单线程的弊端的。

1. 一开始整个脚本作为一个宏任务执行
2. 执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列
3. 当前宏任务执行完出队，读取微任务列表，有则依次执行，直到全部执行完
4. 执行浏览器 UI 线程的渲染工作
5. 检查是否有 Web Worker 任务，有则执行
6. 执行完本轮的宏任务，回到第 2 步，继续依此循环，直到宏任务和微任务队列都为空

### 宏任务和微任务

JS 引擎把所有任务分成两类，一类叫宏任务(macroTask)，一类叫微任务(microTask)

#### 宏任务

    - script(整体代码)
    - setTimeout/setInterval
    - I/O
    - UI 渲染
    - postMessage
    - MessageChannel
    - requestAnimationFrame
    - setImmediate(Node.js 环境)

#### 微任务

    - new Promise().then()
    - MutaionObserver
    - process.nextTick(Node.js 环境）

#### 1. 经典面试题 （字节跳动）

```
    async function foo() {
        console.log('fool')
    }
    async function bar(){
        console.log('bar start')
        await foo()
        console.log('bar end')
    }
    console.log('script start')
    setTimeout(function(){
        console.log('setTimeout')
    },0)
    bar()
    new Promise(function(resolve,reject){
        console.log('promise executor')
        resolve()
    }).then(function(){
        console.log('promise then')
    })
    console.log('script end')

// 执行结果
script start
bar start
fool
promise executor
script end
bar end
promise then
setTimeout
```

</details>

#### 2. 经典面试题

```
setTimeout(function () {
  console.log('timer1')
}, 0)

requestAnimationFrame(function () {
  console.log('requestAnimationFrame')
})

setTimeout(function () {
  console.log('timer2')
}, 0)

new Promise(function executor(resolve) {
  console.log('promise 1')
  resolve()
  console.log('promise 2')
}).then(function () {
  console.log('promise then')
})

console.log('end')

// 执行结果
promise 1
promise 2
end

promise then

timer1
timer2

requestAnimationFrame
```

谷歌浏览器中的结果 requestAnimationFrame()是在一次事件循环后执行，火狐浏览器中的结果是在三次事件循环结束后执行。
可以知道，浏览器只保证 requestAnimationFrame 的回调在重绘之前执行，但没有确定的时间，何时重绘由浏览器决定。

</details>

#### 3. 经典面试题 （哔哩哔哩）

```
var date = new Date() 

console.log(1, new Date() - date) 

setTimeout(() => {
    console.log(2, new Date() - date)
}, 500) 

Promise.resolve().then(console.log(3, new Date() - date)) 

while(new Date() - date < 1000) {} 

console.log(4, new Date() - date)

// 执行结果
1 0
3 1
4 1000
2 1000
```

- 首先执行同步代码，输出 1 0
- 遇到 setTimeout ，定时 500ms 后执行，此时，将 setTimeout 交给异步线程，主线程继续执行下一步，异步线程执行 setTimeout
- 主线程执行 Promise.resolve().then , .then 的参数不是函数，发生值透传（ value => value ） ，输出 3 1
- 主线程继续执行同步任务 whlie ，等待 1000ms ，在此期间，setTimeout 定时 500ms 完成，异步线程将 setTimeout 回调事件放入宏任务队列中
- 继续执行下一步，输出 4 1000
- 检查微任务队列，为空
- 检查宏任务队列，执行 setTimeout 宏任务，输入 2 1000

</details>
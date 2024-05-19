
``` vue3 和 vue2响应式区别
递归方式：Vue2使用Object.defineProperty()实现响应式原理，对对象的所有属性进行递归，如果对象属性被改变，将无法实现响应式。而Vue3使用Proxy()实现响应式原理，可以按需递归，只有当使用到内部对象的属性时，才会进行递归，性能更好。
数组操作：Vue2对数组的操作需要重写数组的方法进行重写，而Vue3则可以轻松实现。
响应式原理：Vue2使用Object.defineProperty()实现响应式原理，而Vue3使用Proxy()实现。
对象属性：Vue2中对象不存在的属性不能被拦截，而Vue3可以。
数组实现：Vue2中改变数组的长度是无效的，无法做到响应式，而Vue3可以。
```

Vue的响应式原理基于以下几个核心概念：

- Observer: Vue使用Observer来监控数据的变化，一旦数据变化，就触发视图更新。

- Dep: 一个依赖收集器，每个Observer对象都维护一个Dep实例，用于存储依赖于这个Observer的所有Watcher。

- Watcher: 观察者，实例化时会向Dep添加依赖，当Dep触发notify时，Watcher会执行相应的更新函数。

- Virtual DOM: Vue使用Virtual DOM来高效地计算出真实DOM的最小变更量。

- Directives: Vue指令是声明式的设置数据绑定的方法，例如v-bind，v-model等。

```
class Observable {
  constructor(data) {
    this.data = data;
    this.observe(data);
  }
 
  observe(data) {
    if (typeof data !== 'object' || data === null) {
      return;
    }
 
    Object.keys(data).forEach(key => {
      this.defineReactive(data, key, data[key]);
    });
  }
 
  defineReactive(obj, key, value) {
    const that = this;
    let dep = new Dep();
 
    Object.defineProperty(obj, key, {
      enumerable: true,
      configurable: true,
      get: function reactiveGetter() {
        Dep.target && dep.addSub(Dep.target);
        return value;
      },
      set: function reactiveSetter(newVal) {
        if (newVal === value) return;
        value = newVal;
        that.observe(newVal);
        dep.notify();
      }
    });
  }
}
 
class Dep {
  constructor() {
    this.subs = [];
  }
 
  addSub(sub) {
    this.subs.push(sub);
  }
 
  notify() {
    this.subs.forEach(sub => sub.update());
  }
}
 
// 使用Observable
const data = {
  message: 'Hello Vue!'
};
 
const observable = new Observable(data);
 
// 观察者
const watcher = {
  update() {
    console.log('Data changed!');
  }
};
 
Dep.target = watcher; // 设置当前观察者
data.message = 'Hello World!'; // 触发更新
Dep.target = null; // 清除当前观察者
```

### Vue3响应式原理
```
function reactive(target) {
  // 如果是数组，则处理成WeakMap的key
  if (Array.isArray(target)) {
    target.__v_isReactive = true;
    // 为数组的各个索引创建代理
    target.forEach((item, index) => {
      target[index] = reactive(item);
    });
  } else if (typeof target === 'object') {
    // 使用WeakMap来保存响应式的数据
    const weakMap = new WeakMap();
    const handler = {
      get(target, key) {
        // 当获取属性时，进行依赖收集
        track(target, key);
        return weakMap.get(key);
      },
      set(target, key, value) {
        // 当设置属性时，触发依赖更新
        trigger(target, key);
        const oldValue = weakMap.get(key);
        if (oldValue !== value) {
          weakMap.set(key, reactive(value));
        }
        return true;
      }
    };
    return new Proxy(target, handler);
  }
  return target;
}

// 存储依赖
const targetMap = new WeakMap();
 
// 依赖收集的简化版本
function track(target, key) {
  if (activeEffect) {
    let depsMap = targetMap.get(target);
    if (!depsMap) {
      targetMap.set(target, (depsMap = new Map()));
    }
    let dep = depsMap.get(key);
    if (!dep) {
      depsMap.set(key, (dep = new Set()));
    }
    dep.add(activeEffect);
  }
}
 
// 触发依赖更新的简化版本
function trigger(target, key) {
  // 获取并执行依赖，这里省略了具体的依赖触发逻辑
  const depsMap = targetMap.get(target);
  if (depsMap) {
    const effects = new Set();
    const dep = depsMap.get(key);
    if (dep) {
      dep.forEach(effect => effects.add(effect));
    }
    targetMap.get(target)?.forEach((dep, key) => {
      if (key !== '__v_isReactive') {
        dep.forEach(effect => effects.add(effect));
      }
    });
    effects.forEach(effect => effect());
  }
}
 
// 假设的activeEffect和depsMap，用于依赖收集和触发
let activeEffect = null;
function effect(callback) {
  const effect = () => {
    activeEffect = effect;
    callback();
  };
  effect();
}
 
// 使用响应式系统的例子
const state = reactive({ count: 0 });
 
// 创建一个effect，它依赖于state.count
effect(() => {
  console.log(state.count); // 跟踪依赖
});
 
// 改变state.count，触发effect执行
state.count++;
```
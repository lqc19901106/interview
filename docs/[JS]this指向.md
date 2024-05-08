## this 指向问题
> - this 在全局中指向 `window`, node 环境指向 `global`
> - 构造函数中的this指向实例化对象
> - 构造函数的静态方法中的this，指向构造函数
> - 普通函数中this，指向调用者
> - 事件处理中的this，指向事件源
> - 箭头函数中没有this值，默认捕获外层上下文的this。
```
console.log(this)  // window

function Dog(name) {
   this.name = name;
   this.say = function() {
      console.log(this);
   }
}
Dog.eat = function() {
   console.log(this);
}
const dog = new Dog(1111);
dog.say();  // Dog { name: 1111, say: f}
Dog.eat();  // f Dog() { } 

function fn() { console.log( this); }
fn();  // window  fn() === window.fn() 调用对象 为window
const obj = {
   name: '123',
   fn: function () { console.log( this); }
}
obj.fn(); // obj  调用对象 为obj


document.body.addEventLister('click', function() { console.log(this); });
// document.dody 事件处理中的this，指向事件源

const obj = {
   test() {
      return () => {
         console.log(this === obj);
      }
   }
}
obj.test()(); // true
```

<b><details><summary>1. 面试题 -</summary></b>
```
   var name = "window";
   var person = {
      name: "person",
      sayName: function () {
         console.log(this.name);
      },
   };
   function sayName() {
      var sss = person.sayName;
      sss(); // window: 独立函数调用
      person.sayName(); // person: 隐式调用
      person.sayName(); // person: 隐式调用  (person.sayName)()其实相当于就是person.sayName();
      (b = person.sayName)(); // window: 赋值表达式(独立函数调用)只要是赋值表达式--就是独立函数的调用---开发中不这种代码
   }
   sayName();
```
</details>

<b><details><summary>2. 面试题二</summary></b>
```
var name = "window";
var person1 = {
   name: "person1",
   foo1: function () {
      console.log(this.name);
   },
   foo2: () => console.log(this.name),
   foo3: function () {
      return function () {
         console.log(this.name);
      };
   },
   foo4: function () {
      return () => {
         console.log(this.name);
      };
   },
};
var person2 = { name: "person2" };
// person1.foo1(); // person1(隐式绑定)
// person1.foo1.call(person2); // person2(显示绑定优先级大于隐式绑定)
// person1.foo2(); // window(箭头函数没有this，不绑定作用域,上层作用域是全局)
// person1.foo2.call(person2); // window
// person1.foo3()(); // window(独立函数调用)
// person1.foo3.call(person2)(); // window(独立函数调用)--重点是只看最终的this绑定是谁，这里是独立函数的调用
// person1.foo3().call(person2); // person2(最终调用返回函数式, 使用的是显示绑定)
// person1.foo4()(); // person1(箭头函数不绑定this, 上层作用域this是person1)
// person1.foo4.call(person2)(); // person2(上层作用域被显示的绑定了一个person2)
// person1.foo4().call(person2); // person1(上层找到person1)person1.foo4()使上层作用域变成person1，又因为箭头函数没有this，不绑定call，而是从上层作用域中
```
</details>

<b><details><summary>3. 面试题三</summary></b>
```
var name = "window";
function Person(name) {
   this.name = name;
   (this.foo1 = function () {
         console.log(this.name);
   }),
   (this.foo2 = () => console.log(this.name)),
   (this.foo3 = function () {
      return function () {
         console.log(this.name);
      };
   }),
   (this.foo4 = function () {
      return () => {
         console.log(this.name);
      };
   });
}
// 函数每次new的时候都会创建一个新对象 每次的传入的this也是根据自己传入的参数自定义的this
var person1 = new Person("person1");
var person2 = new Person("person2");
person1.foo1(); // person1
person1.foo1.call(person2); // person2(显示高于隐式绑定)
person1.foo2(); // person1 (上层作用域中的this是person1)
person1.foo2.call(person2); // person1 (上层作用域中的this是person1)
person1.foo3()(); // window(独立函数调用)
person1.foo3.call(person2)(); // window
person1.foo3().call(person2); // person2
person1.foo4()(); // person1
person1.foo4.call(person2)(); // person2
person1.foo4().call(person2); // person1
var obj = {
  name: "obj",
  foo: function () {},
};
```
</details>

<b><details><summary>4. 面试题四</summary></b>
```
'var name = "window";
function Person(name) {
   this.name = name;
   this.obj = {
      name: "obj",
      foo1: function () {
            return function () {
            console.log(this.name);
         };
      },
      foo2: function () {
         return () => {
           console.log(this.name);
         };
      },
  };
}
var person1 = new Person("person1");
var person2 = new Person("person2");
person1.obj.foo1()(); // window 独立函数的调用
person1.obj.foo1.call(person2)(); // window  person1.obj.foo1.call(person2)是改变foo1的this指向为person2 但是foo1返回值为一个独立函数 独立函数的调用为window
person1.obj.foo1().call(person2); // person2  隐式绑定person2
person1.obj.foo2()(); // obj 上层作用域   主要看的返回值这个函数的上层作用域foo2是被谁调取的  这里是obj 调取的
person1.obj.foo2.call(person2)(); // person2  上层作用域   主要看的返回值这个函数的上层作用域foo2通过call改变this成为person2 所以是person2
person1.obj.foo2().call(person2); // obj 上层作用域是foo2通过隐式绑定变成obj， 箭头函数没有this 看上层作用域的指向
//
// 上层作用域的理解
// var obj = {
//   name: "obj",
//   foo: function() {
//      上层作用域是全局
//   }
// }
// function Student() {
//   this.foo = function() {
// 上层作用域是function Student
//   }
// }
```
</details>

<b><details><summary>5. 手写bind</summary></b>
```
Function.prototype.myBind = function (context, ...args) {
   const _this = this;
   context = Object(context) || window;
   context.fn = this;
   return function F(...otherArgs) {
      let result = null;
      if (new.target) {
         return new _this(...args.concat(otherArgs));
      } else {
         result = context.fn(...args.concat(otherArgs));
      }
      delete context.fn;
      return result;
   };
}
```
</details>

<b><details><summary>6. 手写call</summary></b>
```
Function.prototype.myCall = function (context, ...args) {
   let result = null;
   context = Object(context) || window;
   context.fn = this;
   result = context.fn(...args);
   delete context.fn;
   return result;
};
function a (a, b) {
}
a._call({}, 1, 2, 3);
```
</details>

<b><details><summary>7. 手写apply</summary></b>
```
Function.prototype.myApply = function (context, args = []) {
   let result = null;
   context = Object(context) || window;
   context.fn = this;
   result = context.fn(...args);
   delete context.fn;
   return result;
};

function a (a, b) {
}

a._call({}, 1, 2, 3);
```
</details>

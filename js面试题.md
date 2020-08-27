## this 指向问题

1、 在全局作用域下,`this`指向`windows`

2、 在函数作用域，`this`一般指向函数调用时所在对象（this 指向取决于函数调用时，而不是函数声明时）

- 作为普通函数调用

```javascript
//非严格模式下
function aa() {
  console.log(this);
}
aa(); //window
//严格模式下
("use strict");
aa(); //undefined
```

- 作为对象的方法调用,指向`window`

```javascript
var obj = {
  fun: function () {
    console.log(this);
  },
};
obj.fun(); //obj
```

- 使用`new`构造函数调用，指向实例对象

```javascript
function Fn() {
  this.author = "lry";
}
var o = new Fn();
console.log(o.author); //lry
```

- 使用 call，apply，bind，指向第一个参数绑定的对象

## new 操作符做了什么

- 创建新对象
- 绑定原型关系
- 执行构造函数，绑定 this
- 返回新对象

## 数据类型判断

- `typeof` 只能判断基本数据类型（除了`null`，返回`object`）
- `instanceof`用来判断引用类型

```javascript
var arr = [];
arr instanceof Array; //true
```

- `Object.protype.toString.call()`

```javascript
var arr = [];
Object.protype.toString.call(); //[object Array]
//但是自定义类型无法准确判断
function Person() {}
let person = new Person();
Object.protype.toString.call(); //[object Object]
//明显无法判断出person是Perosn类型
```

## promise 原理

承诺确保状态改变才执行回调

- promise 有三种状态，`pending`，`Fulfilled`，`Rejected`,并且状态只能从`pendding->resolve`,或者`pending->reject`
- 构造一个 promise 实例需要传入一个函数，函数又两个形参，分别为`resolve`，和`reject`，用来改变状态，执行异步回调
- promise 原型上 then 方法绑定回调函数，当状态已经改变时，直接执行回调函数，状态没改变，回调函数存入一个数组，异步结果返回，当状态改变时，再执行回调

```javascript
new Promise(function (resolve, reject) {
  // 一段异步操作
  resolve(); // 数据处理完成
  reject(); // 数据处理出错
}).then(//绑定回调
  function A() {
    // 成功
  },
  function B() {
    // 失败
  }
);
```

## async 和 await

异步的终极解决方案，将异步强行转换为同步处理，async/await的实现是基于 Promise的，简单而言就是async 函数就是返回Promise对象，是generator的语法糖
```javascript
//generaotr函数
function* gen(x){//加星号，于普通函数区分
  var y = yield x + 2;//yield语句标注异步操作需要暂停的地方
  return y;
}
var g = gen(1);  //next方法分步执行
g.next() // { value: 3, done: false }
g.next() // { value: undefined, done: true }
```

- async 和 await 是配对使用的，await 存在于 async 的内部。否则会报错。
- await 表示在这里等待一个 promise 返回，再接下来执行
```javascript
async function fun1(){
    let res = await fun2();//promise结果

    // 取得res以后doSomething 
}

```
## document.ready和window.onload
- 执行时机不同：document.ready在DOM结构绘制完毕后就执行，不必等到加载完毕，document.ready是在window.onload是页面所有元素都加载完毕，包括图片等所有元素
- 执行次数不同：一个页面可以有多个document.ready,但是只能有一个window.onload,写多个只执行最后一个

## 作用域问题
```javascript
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}//一秒立即输出5个5
```
> 首先setTimeout在延迟之后把事件添加到队列，而不是立即执行，主线程for循环依次执行5次添加队列操作，中间间隔忽略不计，所以会在一秒后立刻打印五次，至于为什么是5个5，而不是12345，因为js没有块级作用域，i是全局变量，5次for循环之后，i已经变成5

## 事件循环Event Loop

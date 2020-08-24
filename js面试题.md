## this指向问题
1、 在全局作用域下,`this`指向`windows`

2、 在函数作用域，`this`一般指向函数调用时所在对象（this指向取决于函数调用时，而不是函数声明时）
- 作为普通函数调用
```javascript
//非严格模式下
function aa(){
 console.log(this)
}
aa();//window
//严格模式下
'use strict'
aa()//undefined
```
- 作为对象的方法调用,指向`window`
```javascript
var obj={
    fun:function(){
        console.log(this);
}
    }
    obj.fun();//obj

```
- 使用`new`构造函数调用，指向实例对象
```javascript
function Fn() {
    this.author = "lry"
}
var o = new Fn();
console.log(o.author); //lry
```
- 使用call，apply，bind，指向第一个参数绑定的对象

## new 操作符做了什么
- 创建新对象
- 绑定原型关系
- 执行构造函数，绑定this
- 返回新对象

## 数据类型判断
- `typeof` 只能判断基本数据类型（除了`null`，返回`object`）
- `instanceof`用来判断引用类型
```javascript
    var arr = [];
    arr instanceof Array;//true

```
- `Object.protype.toString.call()`
```javascript
var arr = [];
Object.protype.toString.call()//[object Array]
//但是自定义类型无法准确判断
function Person(){

}
let person = new Person();
Object.protype.toString.call()//[object Object]
//明显无法判断出person是Perosn类型
```



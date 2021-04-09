---
title: 查收一篇js基础文章
date: 2019-10-30 17:57:03
tags:
- Javascript
categories:
- web前端
---

js基本数据类型：**string，boolean，number，null，undefined，Object**

判断js数据类型的方法：
**typeOf：**
除了null都能返回正确的数据类型

``` javascript
typeof null // Object
typeof [] // Object
typeof new Function() // function
let message
typeof message // 对于未声明变量 返回 undefined```

**instanceof**：用于测试构造函数的prototype属性是否出现在对象的原型链中的任何位置

### js的浅拷贝与深拷贝： ###
首层深拷贝：Object.assign，concat ，slice，...运算符
深拷贝：JSON.parse(JSON.stringify())
递归深拷贝：
```javascript
function deepClone(obj = {}) {
  if (obj !== 'object' || obj == null) {
    return obj
  }
  let result = obj instanceof Array ? [] : {}
  for (key in obj) {
    if (obj.hasOwnProtety(key)) {
      result[key] = deepClone(obj[key])
    }
  }
  return result
}
```

### 数组的对象方法 ###
<table>
    <tr>
        <td>push()</td>
        <td>在数组末尾添加，返回数组长度</td>
    </tr>
    <tr>
        <td>  pop()</td>
        <td>  删除数组末尾项，并返回被删除的值</td>
      </tr>
      <tr>
        <td>  shift()</td>
        <td>  删除数组头部元素，并返回删除的值</td>
      </tr>
 <tr>
        <td>  unshift()</td>
        <td>  向数组头部添加元素，并返回length</td>
      </tr> <tr>
        <td>    slice()</td>
        <td>    返回数组指定元素</td>
      </tr><tr>
        <td>      sort()</td>
        <td>      排序</td>
      </tr><tr>
        <td> join()</td>
        <td>将数组放入字符串，并用指定分隔符分隔</td>
      </tr>
<tr>
        <td>   concat()</td>
        <td>  合并多个数组</td>
      </tr>
<tr>
        <td>     splice()</td>
        <td>    添加或删除数组中的项，返回被删除的项，会改变原数组</td>
      </tr>
      <tr>
        <td>       indexOf()</td>
        <td>      查找项在数组的索引</td>
      </tr>
<tr>
        <td>         lastIndexOf()</td>
        <td>        从后往前查找项的位置</td>
      </tr><tr>
        <td>           reverse()</td>
        <td>          反转数组中元素的顺序</td>
      </tr>

</table>

**Array.prototype中的方法**
``` javascript
concat: ƒ concat()
constructor: ƒ Array()
copyWithin: ƒ copyWithin()
entries: ƒ entries()
every: ƒ every()
fill: ƒ fill()
filter: ƒ filter()
find: ƒ find()
findIndex: ƒ findIndex()
flat: ƒ flat()
flatMap: ƒ flatMap()
forEach: ƒ forEach()
includes: ƒ includes()
indexOf: ƒ indexOf()
join: ƒ join()
keys: ƒ keys()
lastIndexOf: ƒ lastIndexOf()
length: 0
map: ƒ map()
pop: ƒ pop()
push: ƒ push()
reduce: ƒ reduce()
reduceRight: ƒ reduceRight()
reverse: ƒ reverse()
shift: ƒ shift()
slice: ƒ slice()
some: ƒ some()
sort: ƒ sort()
splice: ƒ splice()
toLocaleString: ƒ toLocaleString()
toString: ƒ toString()
unshift: ƒ unshift()
values: ƒ values()
```
### this的指向 ###
this 的值并不是由函数定义放在哪个对象里面决定，而是函数执行时由谁来唤起决定。

```javascript
// 1.函数名和匿名函数，this指向window
function getName () {
    console.log(this) // window
}
(function () {
    console.log(this) // window
})()
var getName = function() {
    let name= 'blue'
    console.log(this.name) // undefined
}
```
``` javascript
// 2.对象.方法名()  this指向对象
var ObjList = {
    name: 'zlp',
    getName: function () {
        console.log(this) // 指向ObjList对象
    }
}
```

``` javascript
// 3.new 构造函数名(), this指向构造函数
function Person () {
    console.log(this) // this 指向构造函数Person
}
var personOne = new Person()
```


4.间接调用apply，call
>this就是call和apply对应的第一个参数,如果不传值或者第一个值为null,undefined时this指向window
>apply()方法接收两个参数：一个 是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是 Array 的实例，也可以是 arguments 对象
>call()方法与 apply()方法的作用相同，它们的区别仅在于接收参数的方式不同。对于 call() 方法而言，第一个参数是 this 值没有变化，变化的是其余参数都直接传递给函数。换句话说，在使用 call()方法时，传递给函数的参数必须逐个列举出来
>扩充函数赖以运行的作用域
```javascript
function foo() {
    console.log(this);
}
foo.apply('我是apply改变的this值');//我是apply改变的this值
foo.call('我是call改变的this值');//我是call改变的this值 ```

节流
```javascript
let throttle = function(func, delay) {
    let timer = null
    return () => {
     if (!timer) {
        timer = setTimeout( () => {
            func.apply(this, argments)
            timer = null
        }, delay)
        }
    }
}```

闭包：就是在一个函数a返回了一个函数b，函数b使用了函数a的变量，函数b就是闭包
优点：能在函数中访问另一个函数的变量
缺点：该变量存储在内存中，不会被销毁，所以会影响性能，不能滥用闭包，避免造成性能问题
应用场景：
```javascript
let obj = {}
function fn() {
  return {
    set: function (name, value) {
      obj[name] = value
    }
    get: function (name) {
      return obj[name]
    }
  }
  
}
```

---
title: js原型到原型链
date: 2020-05-07 18:47:08
tags:
- Javascript
categories:
- web前端
---

每个函数都有prototype
```javascript
function Person {}
Person.prototype.name = 'zzz'
let p1 =  new Person()
p1.name // zzz
```

```javascript
function Person (name) {
    this.name = name,
    this.eat = function() {
        console.log('吃饭')
    }
}
var p1 = new Person('小明')
var p2 = new Person('小红')

p1.eat === p1.eat // false
```

通过new构造出来的函数，都会生成新的堆区，会造成内存浪费，影响性能
要实现公共库的对象，就要用到prototype(作用：共享方法，不开辟新内存空间)

```javascript
function Person(name) {
    this.name = name
}
Person.prototype.eat = function () {
    console.log('吃饭')
}
let p1 = new Person('小明')
let p2 = new Person('小红')
p1.eat === p2.eat // true (两个实例对象指向相同的位置了)
p1.__proto__ = Person.prototype
```

js本源是空的 即null
对象都有__proto__属性，Function有prototype
Function是个特殊对象，它有prototype属性，指向这个对象共享的属性和方法
Funtion 有__proto__ 和 prototype
Person.prototyoe指向了原型对象，原型对象有共有的方法, Person.__proto__ = Function.prototype
p1和p2是Person这个对象的两个实例，指向构造函数的原型对象(prototype)  p1.__proto__ =  Person.prototype

__proto__指向的是一条指针链，先查找自身属性，找不到就查找原型对象

```javascript
function Foo () {
    this.sing = function () {
        console.log(1)
    }
} 
Foo.prototype.sing = function() {
    console.log(2)
}
Object.prototype.sing = function () {
    console.log(3)
}
let p1 = new Foo()
p1.sing() 

p1.__proto__ === Foo.prototype // true 指向了共享的属性和方法
Foo.__proto__ === Function.prototype // true
Function.prototype.__proto__ === Object.prototype // true
Object.prototype.__proto__ === null
```
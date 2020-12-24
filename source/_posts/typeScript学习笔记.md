---
title: TypeScript学习笔记
date: 2020-12-14 21:23:43
tags:
---
## 1、ts的主要数据类型
布尔类型
数字类型
字符串类型
数组类型
```javascript // 在数组类型后面加上[]
let arr:number[] = [1, 2]
// 使用数组泛型
let arr:Array<number> = [1, 2]
```

元祖：用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同（数组的一种）
```javascript
let x: [string, number]
x = ['Runoob', 1]
x = [1, 'Runoob']
console.log(x[0])
```

enum：枚举
```javascript
enum Color {Red, Green = 2, Blue}
let c: Color = Color.Blue // 2
Color.Red // 0 没有赋值时 为索引值
Color.Blue // 3  
```

any：任意类型
```javascript
let x:any = 1
x = 'string'
```

null 和 undefined
```javascript
let num:number
console.log(num) // 报错
let num:number | undefined // 指定类型为number或者undefined
```

void: 用于表示方法返回值的类型，一般用于定义方法的时候方法没有返回值
声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null
```javascript
let unusable:void = undefined;
function hello():void {
    alert('hello, runoob')
}
function hello():number {
    return 123
}
```

never类型：是其他类型（包括null和undefined） 从来不会出现的值，不太用到，一般可用any代替
```javascript
var a:never
a = (() => {
    throw new Error('错误')
})()
```


## 2、ts中的函数

### 2.1定义函数的方法
函数声明方法
```javascript
function run():string {
  return 'run'
}
```

匿名函数
```javascript
var fun2 = function():number {
    return 123
}
```

定义方法传参
```javascript
function fun(name:string, age:number):string {
    return ${name}${age}
}
```

可选参数，在参数后加个?(可选参数必选配置到参数的最后)
```javascript
function fun2(name:string, age?:number):string {}
```

默认参数
```javascript
function fun3(name:string, age:number=20):string {}
```

剩余参数 ...
```javascript
function sum(a:number, b:number, ...result:number[]):number {
  let s = a+b
  for (let i = 0; i < result.length; i++) {
    s += result[i]
  }
  return s
}
sum(1,2,3,4,6)
```

函数重载：同名的函数，传入不同的参数，实现不同的结果（es5没有重载，会直接替换相同函数）
```javascript
function getInfo(name:string):void;
function getInfo(age:number):void;
function getInfo(str:any):void{
  if(typeof str == "string"){
    console.log("名字:",str)
  }
  if(typeof str == "number"){
    console.log("年龄",str)
  }
}
getInfo("zhangsan")
```


es5 类和静态方法  继承  （p5  要多看几遍）

### 3、ts中的类  

## 3.1 ts中定义类
```javascript
class Person {
  name:string;
  constructor(n:string) {
      this.name = n
  }
  getName():string {
      return this.name
  }
  setName(n:string):void {
      this.name = n
  }
}
let p = new Person('zhangsan')
p.setName('李四')
console.log(p.getName())
```

## 3.2、ts中实现继承 关键字 extends super
```javascript
class Work extends Person {
  constructor(name:string) {
    super(name) /** 初始化父类的构造函数 **/
  }
}
let w = new Work('zhangsan')
console.log(w.getName())
```

** *当子类和父类有相同方法时，调通该子类的方法，会先去找子类中的方法，再去找父类里找**


## 3.3 类里的修饰符
ts在定义属性时，提供了三种修饰符
public: 公有，在当前类里面、子类、类外面都可访问
protected：保护类型；在当前类里面、子类可访问，类外面不可访问
private：私有，在当前类里面可以访问，子类、类外面都不可访问
属性不加修饰符，默认public
```javascript
// 定义类
class Person {
  public/private/private name:string;
  constructor(name:string) {
      this.name = name
  }
  run ():string {
    return `${this.name}在运动` // 在类里访问
  }
}

/** 继承 */
class Work extends Person {
  constructor(name:string) {
    super(name)
  }
  work ():string {
    return `${this.name}在工作` // 在子类里访问属性
  }
}
let p = new Person('张三')
console.log(p.name) // 在外部访问
```

### 3.4 类中的静态属性 静态方法
es5中的静态方法
```javascript
function Person () {
    this.run1=function () {} // 实例方法
}
Person.run2 = function () {} // 静态方法
Person.name = 'hhhh' // 静态属性
let p = new Person()
p.run1() // 调用实例方法
Person.run2() // 静态方法的调用
```


接口
ts中自定义方法传入参数对json的约束
```javascript
function printLabel(labelInfo:{label:string}):void {
}
printLable({label: 'aaa'}) // 必须要传入有label的对象，且label为string类型
```

属性类接口
对传入的批量方法进行约束
接口，对行为和动作的规范，对批量方法进行约束
```javascript
interface FullName { // 定义接口
  firstName:string; // 分号
  secondName:string
}
function printName (name:FullName) {
    console.log(name.firstName + name.secondName)
}
printName({firstName: '23', secondName: '323'}) // 必须传入只包含firstName 和 secondName字段的对象

let obj = {
    age: 10,
    firstName: '23',
    secondName: '323'
}
printName(obj) // 这样写 可以传入别的值 （用字面量  绕开检查机制？）

interface FullName {
  firstName:string; 
  secondName?:string // 可选属性  方法里可不传这个参数
}
```



函数类接口：对传入的参数和返回的参数进行约束，也可以批量约束
```javascript
interface a{
    (key:string, value:string):string
}
var md5:a = function(key:string,value:string):string {
    return
}
md('name', 'zhangsan')
```

可索引接口：数组\对象的约束（不常用）
第一种写法:对数组的约束
```javascript
interface userArr{
    [index:number]:string
}
var arr:userArr = ['aaa', 'bbb']
```

对对象的约束（更不常用）

类类型接口：对类的约束（抽象类）常用
```javascript
interface Animal {
    name:string;
    eat(str:string):void;
}
class Dog imlement Animal {
    name:sring;
    constructor(name:string) {
        this.name = name
    };
    eat(food:string) {
        console.log(this.name+this.food)
    }
}
let d = new Dog('小黑')
d.eat('吃骨头')
```

对批量方法进行约束


泛型
可以支持不特定的数据类型，要求传入参数和返回参数一致
通俗理解：泛型是解决类 接口 方法的复用性，以及对不特定数据类型的支持
T 表示泛型

*传入什么类型返回什么类型
不用泛型：需要写多个函数
```javascript
function identity(arg:number):number {
    return ary
}
```

可以使用any实现，使用any就放弃了类型检查，any可以接受任何类型的参数，丢失了一些信息
应该传入什么类型返回什么类型
```javascript
function identity(arg:any):any {
    return arg
}
```

使用泛型：给identityt添加了类型变量T，T帮我们捕获了传入的类型，之后我们可以使用这个类型
```javascript
function identity<T>(arg:T): T {
    return arg
}
// 两种方法使用
identity<number>(122) // 复杂情况下需要明确传入T的类型
identity(111) // 更普遍 ；类型推论--编译器帮助我们确定T的类型
```

泛型类：
```javascript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

ts中的模块
 “内部模块”现在称做“命名空间”。 “外部模块”现在则简称为“模块”
与es6差不多
export  导出声明
export 导出语句
export default
import 导入模块

命名空间
在代码量较大的情况下，为了避免命名冲突，可将相似功能的函数、接口、类等放置到命名空间内
Typescript的命名空间，可将代码包裹起来，只对外暴露需要在外部访问的对象，命名空间内的对象可用export暴露，命名空间是唯一的
```javascript
namespace A {
    interface Animal {
    	name: string;
     	eat(): void;
    }
    export class Dog implements Animal {
     	name: string;
      	constructor(theName: string) {
           	this.name = theName       
      	}
       	eat () {
                    
        }
    }
}
```

命名空间和模块的区别：
命名空间：内部模块，主要用于组织代码，避免命名冲突
模块：ts的外部模块简称，侧重代码的复用，一个模块里可能会有多个命名空间

Typescript 装饰器
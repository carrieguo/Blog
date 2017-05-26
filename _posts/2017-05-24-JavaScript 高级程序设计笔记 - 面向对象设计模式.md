---
layout:            post
title:             "面向对象设计模式"
date:              2017-05-17 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---

## 工厂模式
```javascript
function creatPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name); 
    };
    return o;
}
var person1 = creatPerson('carrie',29,'coder');
```
缺点：无法识别对象类型

## 构造函数模式
```javascript
//在开发中，如果使用到了构造函数，通常将函数名首字母大写
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    }
}
var person1 = new Person('Carrie',29,'coder');
var person2 = new Person('xiao',29,'coder');
```
#### 构造模式中 new 操作符的执行过程
1. 创建一个新对象
2. 将构造函数的作用域赋给新对象（因此this就指向了这个新对象）
3. 执行构造函数中的代码（为这个新对象添加属性）`创建多个对象时，其中的sayName方法要运行多次`
4. 返回新对象

缺点：构造函数创建的对象上面的每一个方法和属性都要在创建的对象上重新实例化一遍。
```javascript
alert(person1.sayName == person2.sayName);//false
```

#### 构造函数模式与工厂模式的区别
1. 没有显示的常见对象
2. 直接将属性和方法赋给了this对象
3. 没有return语句

#### constructor
    对象上的一个属性，每个对象都有一个constructor属性。实际上constructor是一个指针，指向当前对象的构造函数。
    工厂模式下创建出的对象，当调用此对象的constructor时，只能得到一个Object。也就是说，创建的对象的构造函数就是之前的Object。
    构造函数模式创建出的对象可以检测由哪个构造函数创建的，能识别对象类型。
   
#### instanceof   
判断一个对象是否是某个构造函数创建的可以使用  instanceof  
```javascript
alert(person1 instanceof Object); //true ,所有对象均继承自Object
alert(person1 instanceof Person); //true
```
## 原型模式
    在JS中，每创造的函数都有一个特殊的属性prototype`原型`，指向当前函数的一个原型对象。默认情况下，所有源性对象都会自动获得一个constructor(构造函数)属性，这个属性包括一个指向prototype属性所在函数的指针。
    调用构造函数创建一个对象后，对象的内部都有一个特殊的指针`[[Prototype]]`指向它的构造函数的原型对象，正常情况下访问不到这个指针，但是现代浏览器在每个对象上都支持一个属性__proto__，可以通过其访问到。
    ES5 中也提供了一个方法叫 Object.getPrototypeOf(),来访问这个指针。
```javascript
function Person(){}
Person.prototype.name = 'carrie';
Person.prototype.age = 29;
Person.prototype.job = 'coder';
Person.prototype.sayName = function(){
    alert(this.name);
};
var person1 = new Person();
var person2 = new Person();
alert(person1.sayName == person2.sayName);  //true
```
<figure>
   <img src="{{ "/media/img/prototype_process.jpg" | absolute_url }}" />
   <figcaption>prototype</figcaption>
</figure>

#### 原型模式的另一种写法，以及带来的问题 `原型的动态性`
```javascript
function Person() {}
Person.prototype = {
    name : 'carrie',
    age : 29,
    job : 'engineer',
    sayName : function() {
      alert(this.name);
    }
}
//这种更简单的写法会带来一个问题。我们将Person.prototype设置为等于一个以对象字面量形式创建的新对象。前面介绍过，每创建一个函数，就会同时创建它的prototype对象，这个对象也会自动获得constructor属性。而我们在上面的写法里，本质上完全重写了默认的prototype对象，因此constructor属性也就变成了新对象的constructor属性（指向Object构造函数），不再指向Person函数。
//此时instanceof操作符还能返回正确的结果，但是通过constructor已经无法确定对象的类型了
var friend = new Person();
alert(friend instanceof Object);    //true
alert(friend instanceof Person);    //true
alert(friend.constructor == Person);    //false
alert(friend.constructor == Object);    //true

//解决constructor指针指向，可以像下面一样特意将它设置
function Person() {}
Person.prototype = {
    constructor : Person,
    name : 'carrie',
    age : 29,
    job : 'engineer',
    sayName : function() {
      alert(this.name);
    }
}
//但是这样又有一个问题，默认情况下原生的constructor属性是不可枚举的，我们还需要使用ES5中的Object.defineProperty()
//重设构造函数
Object.defineProperties(Person.prototype, 'constructor',{
    enumerable: false,
    value:Person
});
```


#### 查找对象属性时 `原型链查找`
    当我们访问person1上的某个属性时，解释器首先在person1对象上查找有没有这个属性，没有找到会顺着prototype找到对应的原型对象，直至Object.如果还未找到，就会返回undefined。
    例如，所有的对象我们都可以调用它的toSting()和valueof()方法，因为这两个方法存在于Object的原型对象上。
    object的原型对象还有三个，分别为：
    1. isPrototypeOf() 确定是否是当前对象的原型对象    
```javascript
alert(Person.prototype.isPrototypeOf(person1)); //true
```
    2. hasOwnProperty() 判断参数是否是当前对象上的自由属性

```javascript
alert(Person1.hasOwnProperty('name')); //false name来自于原型对象
```
    3. for-in 循环访问对象属性时 除了获取对象本身的属性，还会顺着原型链获取上一层的所有可枚举的属性。in 本身是一个操作符，可以判断属性是否能够访问到。
```javascript
//for-in
for (x in person1)
{
 console.log(x);   
}
//in
alert('name' in person1); //true 来自原型
```
    4.Objext.getOwnPropertyNames() 得到当前对象下所有的自由属性
    3.Objext.keys() 得到所有属性，得到的结果和for-in循环相同

#### 可枚举属性和不可枚举属性
    在实际开发中我们创建的属性方法都可枚举
    在IE8下创建新对象设置属性toString,是不可枚举的
  000000000000000000
    
#### 设置对象属性或方法时
    解释器看当前对象是否存在这个属性或方法，如果有，直接赋值；若不存在，直接在当前对象创建属性或方法。
    会引起同名屏蔽问题,有可能导致原型对象和当前对象上存在一个同名的变量和函数，访问时只能访问到当前对象上的属性和方法。
    1. 尽量不要创建同名属性和方法
    2. 使用delete操作符删除

#### this 指向四种分类
1. 函数的调用
```javascript
function a(){
    this.name=2;
}
//作为纯函数的调用，指向全局作用域，在浏览器环境下指向windows对象
```
2. 作为对象的方法或属性调用时，this指向此对象
    例如工厂模式中的this
    
3. 构造函数中指向创建出的新对象
4. 强制指向一个对象，apply call,强制转换this指向

#### 在实际开发中，如果不确定this的指向，
1.可以console.log一下
2. 设置一个断点，在控制台打印this，

## 组合模式 `组合使用构造函数模式和原型模式`
    一般我们在开发中，构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。每个实例都会有自己的一份实力属性的副本，但同时又共享着对方法的引用，极大限度的节省了内存。这种混合模式还支持传递参数。

```javascript
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ['carrie','bill'];
}

Person.prototype = {
    constructor : Person,
    sayName : function() {
      alert(this.name);
    }
}

var person1 = new  Person('carrie',29,'coder');
var person2 = new  Person('bill',29,'coder');

person1.friends.push('van');

alert(person1.sayName === person2.sayName); //true
```
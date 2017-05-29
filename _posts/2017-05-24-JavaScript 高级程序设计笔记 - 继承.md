---
layout:            post
title:             "继承"
date:              2017-05-26 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
继承的目的是让一些对象拥有另一些对象的属性和方法
JS语言中虽然不存在继承，但是我们可以通过一些手段来模拟继承。

### 原型链 `实际开发中很少单独使用`
    核心思想：让子类的原型对象指向父类的实例
    我们有两个构造函数a和b,我们的目的是让b的实例可以使用a的属性和方法。
    当使用new b创建一个b对应的实例时，如果想访问实例的属性和方法，会先看实例上有没有这个属性和方法，如果没有就会到实例对应的原型对象上接着去找。
```javascript
b.prototype = new a();
    //让子类的原型对象指向父类的实例
```
```javascript
function SuperType() {
  this.property = true;
}
SuperType.prototype.getSuperValue = function(){
    return this.property;
};
function SubType(){
    this.subpropery = false;
}
//继承了SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function(){
    return this.subpropery;
};
var instance = new SubType();
alert(instance.getSuperValue()); //true
```
<figure>
   <img src="{{ "/media/img/prototype.jpg" | absolute_url }}" />
   <figcaption>原型链</figcaption>
</figure>
事实上，上面展示的原型链还少一环。所有引用类型默认都继承了Object,而这个继承也是通过原型链实现的。所以函数的默认原型都是Object的实例，因此默认原型都会包含一个内部指针，指向Object.prototype。这也正是所有自定义类型都会继承toString()、valueOf()等默认方法的根本原因。如下图
<figure>
   <img src="{{ "/media/img/prototype_all.jpg" | absolute_url }}" />
   <figcaption>原型链</figcaption>
</figure>

优点
1. 使用简单
2. 在b.prototype上增加属性和方法，子类都可以访问到，在父类的原型中，可以动态增加属性和方法，不会影响父类。
 b.prototype=a.prototype ,增加到b.prototype上的属性方法，会直接增加到a上，会破坏a的原型对象。

缺点
1. 当为子类增加属性和方法时，需要写在 b.prototype = new a();之后，否则会被覆盖掉
2. 原型链式的继承，无法实现多继承。
3. 所有属性都是共享的`类似原型`。
```javascript
function SuperType(){
    this.colors = ['red','blue','green'];
}
function SubType() {
}
//继承了SuperType
SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push('black');
alert(instance1.colors);    //'red,blue,green,black'

var instance2 = new SubType();
alert(instance2.colors);    //'red,blue,green,black'
/*当SunType通过原型链继承了SuperType之后，SubType.Prototype就变成了SuperType的一个实例，因此它也拥有了一个它自己的color属性，就跟专门创建了一个SubType.prototype.colors属性一样。结果SubType所有的实例都会共享这一个colors属性，我们对instance1.colors的修改能够通过instance2.colors反映出来。*/
```
4. 在创建子类型的实例时，不能向超类型的构造函数中传递参数。实际上，应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。


#### 借用构造函数
    核心思想：让父类的构造函数在子类中重新运行一遍。
    在子类型的构造函数内部调用超类型构造函数
```javascript
function SuperType(){
    this.colors = ['red','blue','green'];
}
function SubType(){
    //继承了SuperType
    SuperType.call(this);
}

var instance1 = new SubType();
instance1.colors.push('black');
alert(instance1.colors);    //'red,blue,green,black'

var instance2 = new  SubType();
alert(instance2.colors);    //'red,blue,green'
```
优点
1. 可传参
```javascript
function SuperType() {
  this.name = name;
}
function SubType(){
    //继承了SuperType,同时还传递了参数
    SuperType.call(this,'Carrie');
    //实例属性
    this.age = 29;
}

var instance = new SuperType();
alert(instance.name);   //Carrie
alert(instance.age);    //29
```
2. 可实现多继承
3. 所有属性不共享

缺点
1. 创建的实例只是子类的实例，只是重新运行了一遍父类的方法
2. 只能继承父类构造函数中的属性和方法，无法继承源性对象中的属性和方法
3. 构造函数创建的对象上面的每一个方法和属性都要在创建的对象上重新实例化一遍。

#### 组合继承 `伪经典继承，多使用此方法`
1. 子类的构造函数中使用call或apply,重新运行一遍父类的构造函数。`SuperType.call(this,name);`
2. 让子类的原型指向父类的实例。`SubType.prototype = new SuperType();`

缺点：
1. 父类的构造函数被调用了两次。占资源。call，apply;b.prototype=new a()
2. 会引起同名屏蔽问题,有可能导致原型对象和当前对象上存在一个同名的变量和函数，访问时只能访问到当前对象上的属性和方法。属性和方法在实例和原型中各存在一份，

####原型式继承`Object.creat()`
类似原型模式，包含引用类型值的属性始终都会共享相应的值。
接收两个参数：一个用做新对象原型的对象(要继承的对象)和（可选的）一个为新对象定义额外属性的对象。
````javascript
var person={
    name:'carrie',
    friends:['bill']
};

var anotherPerson = Object.create(person);
anotherPerson.name = 'Greg';
anotherPerson.friends.push('Rob');

var yetAnotherPerson = Object.create(person);
yetAnotherPerson.name = 'Linda';
yetAnotherPerson.friends.push('Bar');

anotherPerson.name; //"Greg"
yetAnotherPerson.name; //"Linda"
alert(person.friends); //"bill", "Rob", "Bar"
````
创建对象的三种方式
1. 字符字面量
2. new操作符
3. Object.create()

#### 寄生式继承
与原型式继承紧密相关。寄生式继承的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，然后再像真的是它做了所有工作一样返回对象。
```javascript
function creteAnother(original){
    var clone = object(original);   //通过调用函数创建一个新对象
        clone.sayHi= function() {   //以某种方式来增强这个对象
          alert('hi');
        };
        return clone;
}

var person = {
    name:'carrie',
    friends:['carrie','bill']
};
var another = creteAnother(person);
```
#### 组合寄生式继承 `完美继承`
```javascript
b.prototype = new a(); //旧写法
b.prototype.__proto__=a.prototype;  //新写法，但是只支持ES5，浏览器兼容性不好

//寄生组合式集成的基本模式
function inheritPrototype(subType,superType) {
  var p = object(superType.prototype); //创建对象
  p.constructor= subType;                //增强对象
  subType.prototype=p;                  //指定对象
}

function SuperType(name) {
    this.name = name;
    this.colors = ['red','blue']
}

SuperType.prototype.sayName = function() {
  alert(this.name);
}
function SubType(name,age) {
  SuperType.call(this,name);
  this.age = age;
}
inheritPrototype(SubType,SuperType);
SubType.prototype.sayAge = function() {
  alert(this.age);
};
```


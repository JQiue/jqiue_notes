---
title: 面向对象编程
category: 编程语言
tag: JavaScript
author: JQiue
article: false
---

## 创建类

JavaScript 并不是像其它语言一样通过`class`关键字来定义类，而是通过构造器函数的方式来创建类的实例：

```js
function Person() {}
var foo = new Person();
```

## 实例化对象

通过`new`关键字配合函数的调用产生了对象的实例，JavaScript 虽然使用的类来实现面向对象编程，但是类却由构造器来定义，而构造器使用函数来实现，比如`Person()`。按照定义，一个类有自己的属性和方法，而 JavaScript 是动态的，因此能够在类被实例化后动态的为对象添加属性和方法：

```js
foo.age = 18;
foo.say = functioin (){
  console.log('我是 foo');
}
```

## 声明属性和方法

也可以通过`this`关键字在构造器中定义属性和方法，这样初始化的时候就会自带一些属性和方法：

```js
function Person() {
  this.name = 'foo';
  this.age = '18';
  this.showInfo(){
    console.log('我叫' + this.name，'我的年龄是' + this.age);
  }
}
```

::: danger this 的指向
如果一个函数被当作构造器使用，那么`this`永远指向当前实例对象，否则指向全局对象`window`
:::

类的属性可以通过构造器的参数传递

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.showInfo(){
    console.log('我叫' + this.name，'我的年龄是' + this.age);
  }
}
var foo = new Person('foo', 18);
```

所有的实例对象都有一个`constructor`属性指向构造器，如果对实例使用`constructor`，将会返回构造器的引用，于是就有了下面的写法：

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.showInfo = function (){
    console.log('我叫' + this.name + '我的年龄是' + this.age);
  }
}
var foo = new Person('foo', 18);
var bar = new foo.constructor();
```

### prototype 定义属性和方法

除了在构造器中定义属性和方法以外，还可以通过`prototype`定义，每一个构造器都有`prototype`属性，这个属性是静态的，无需实例化，一旦通过`prototype`定义了属性或方法，所有的实例对象都可以使用

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.showInfo = function () {
    console.log('我叫' + this.name + '我的年龄是' + this.age);
  }
}
Person.prototype.gender = null;
Person.prototype.sayHello = function () {
  console.log('hello');
}
var foo = new Person('foo', 18);
foo.sayHello();
```

### \_\_proto\_\_ 定义属性和方法

每个实例都有个`__proto__`属性指向创建它的构造器的`prototype`属性，于是产生了这种写法：

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.showInfo = function () {
    console.log('我叫' + this.name + '我的年龄是' + this.age);
  }
}
var foo = new Person('foo', 18);
foo.__proto__.run = function () {
  console.log('我可以动');
}
var bar = new Person('bar', 22);
bar.run();
```

::: tip
最好将新功能添加到构造器的`prototype`属性，因为它会被所有的实例对象共享
:::

## Object

并非所有的类都必须通过`new`运算符来实例化，也可以使用`Object`来实例化一个对象

```js
var foo = new Object({
    name: 'foo',
    age: 22,
    show: function () {
      console.log('我叫' + this.name + '我的年龄是' + this.age);
  }
});

var bar = {
  name: 'bar',
  age: 18,
  show: function () {
    console.log('我叫' + this.name + '我的年龄是' + this.age);
  }
}
```

这两种写法都是等效的，无论是`new Object`，还是通过`{}`创建的实例都是属于`Object`，这是一种键值对写法，通过这种方式创建出来的叫做**字面量对象**

## 构造器配合 Object 定义属性和方法

为构造器的`prototype`定义属性或方法，实例都可获得，那么说明`prototype`指向的也是一个对象，因此可以让`prototype`重新指向一个新对象就可以实现定义属性和方法：

```js
function Person() {}
Person.prototype = {
  name: 'foo';
  age: 18;
  show: function () {
    console.log('我叫' + this.name + '我的年龄是' + this.age);
  }
}
var foo = new Person();
```

## 只读和只写属性

在面向对象编程中，属性往往是无法直接读写的，如果想要实现单独的控制就应该使用一些办法来屏蔽外部直接访问属性的接口，通过`this`和`prototype`定义的属性都是可以被读写的，所以应该使用`var`来定义属性，因为`var`定义的属性是有局部作用域的，变相的实现了私有属性，然后通过方法来实现属性的读写

```js
function Person() { 
  var name = null;
  this.getName = function(){
    return name;
  }
  this.setName = function(value){
    name = value;
  }
}
var foo = new Person();
foo.setName('foo');
foo.getName();
foo.name // Error
```

## this

在类中，`this`一般指向自己的实例，`this`的行为和当前类的实例变量一样。但是`this`会在方法内的嵌套函数中形成一个新的环境，在这里的`this`会指向全局对象`window`，而不是对象的实例

```js
function Foo() { 
  this.method = function(){
    function test(){
      console.log(this);  // window
    }
    test();
  }
}
```

因此想要在嵌套函数中使用实例的`this`，最好使用中间变量引用一下：

```js
function Foo() { 
  this.method = function(){
    var that = this;
    function test(){
      console.log(that);  // Foo
    }
    test();
  }
}
```

::: tip
如果构造器不是通过`new`来创建类的实例，而是当作函数一样调用，那么`this`也不会指向
:::

## 静态方法

通过`prototype`定义的方法叫实例方法，实例方法要通过一个变量指向构造函数的新对象，而静态方法则可以直接在构造器上定义，因为函数本身就是一个对象：

```js
function Foo() {}
Foo.a = function name() {
  console.log('static method a');
}
Foo.a();
```

静态方法是属于类的方法，不能通过实例来调用，而是直接使用类来调用

## 成员访问

对于一个类来说，应该限制对类成员的直接访问，这样一个程序才更加可靠，在一些情况下需要将一些成员指定为私有的，使用`this`和`prototype`定义的属性和方法都是公共成员，使用普通的定义方式就可以声明私有成员，定义私有成员有助于封装数据

## 继承

JavaScript 是通过`prototype`来实现继承的，要想继承一个类，可以使用`prototype`指向该类的实例：

```js
function Person(name , age){
  this.name = name;
  this.age = age;
}
function Child(){}
Child.prototype = new Person();
var zs = new Child();
zs.name = 'zs'; // zs
```

如果想在创建子类实例的时候赋予初始值，则应该手动在子类中写调用父类的构造器方法来实现

```js
function Person(name , age){
  this.name = name;
  this.age = age;
}
function Child(name, age){
  this.$super = Person;
  this.$super(name, age);
}
Child.prototype = new Person();
var zs = new Child('zs', 23);
zs.name; // zs
```

`super`是 JavaScript 中的保留字，不能作为标识符使用。很多语言都定义了`super()`方法来实现调用父类的构造器，但是`JavaScript`没有定义这种方法，所以要手动实现`super()`方法。由于 JavaScript 所有的类都是由`Object`派生的，只要为`Object`定义一个`$super()`，就可以作用于所有的类

```js
Object.prototype.$super = function () {
  var result;
  try {
    result = eval(this.constructor).prototype.constructor;
    // 调用基类的构造器方法
    result.apply(this, arguments);
  } catch (error) {
    // 如果不是内建类或者自定义类，或者不在构造器中调用该方法，那么抛出错误
    throw new Error("only can be used in constructor!");
  }
}
function Person(name , age){
  this.name = name;
  this.age = age;
}
function Child(name, age){
  // this.$super = Person;
  this.$super(name, age);
}
Child.prototype = new Person();
var zs = new Child('zs', 23);
zs.name; // zs
```

## 组合类

当一个类无法形成继承关系，则无法使用继承来重用类，但是一个类的属性可以是另一个类，也就是说，一个类的成员含有一个或多个类的对象，这种结构的类就叫组合类

```js
function Window() {
  this.open = function () { }
  this.close = function () { }
}
function Door() {
  this.open = function () { }
  this.close = function () { }
}

function House() {
  this.openWindow = function () {
    var window = new Window();
    window.open();
  }
  this.closeWindow = function () {
    var window = new Window();
    window.close();
  }
  this.openDoor = function () {
    var door = new Door();
    door.open();
  }
  this.openDoor = function () {
    var door = new Door();
    door.close();
  }
}
```

类的组合符合真实世界的开发方式，在真实世界中，一个对象由更多的小对象组成，`Window`类和`Door`不仅仅可以组合成`House`类，也可以组成`Car`类

## 静态成员

静态成员指不需要实例化就可以访问到成员，也被称为类成员，而非静态成员需要实例化才能访问，所以被称为实例成员，如果一个类所有的成员都是静态的，那么这个类就是静态类

之所以定义静态成员，是因为某些对象的一些特定行为是不会改变的，比如太阳和月亮都是世间唯一的，谈不上个体，因此无需实例化。如果在更广泛的特定下，太阳和月亮都属于行星，它们都属于行星类的一个个体。

有一些类虽然可以实例化，但是其中的某一些属性和方法也具有一定的唯一性，这些都应该被定义为静态成员。如果想要将成员声明为静态的，则直接将静态成语赋值给类本身

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}
Person.gender = '男';
Person.showGender = function () {
  console.log('我的性别是' + this.gender);
}
Person.showGender();
```

静态成员是一般是公共成员，但是静态成员不能够被继承，因为它属于类，而不属于实例对象

::: danger
静态方法只能访问静态属性，不能访问实例属性
:::

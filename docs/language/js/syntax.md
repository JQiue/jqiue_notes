---
title: 基本语法
category: 编程语言
tag: JavaScript
author: JQiue
article: false
---

这里是 JavaScript 基本语法规则

## 注释

```js
// 单行注释，只能注释单行

/*
多行注释
可以注释多行内容
*/
```

::: tip
不支持注释嵌套，不要在多行注释中嵌套另一个多行注释
:::

## 代码块和语句

JavaScript 采用`{}`来区分代码之间的层次，当存在换行符时，大多数情况下可以省略语句的分号

::: tip
JavaScript 会将换行符理解成“分号”，但有部分例外，比如在`[]`前不会被添加一个隐式的分号，运行时可能造成错误，所以对新手来说最好不要省略
:::

## 标识符

+ 由英文字母、数字、符号`$`和`_`组成，但第一个字符不能是数字
+ 不能是 JavaScript 中的关键字和保留字
+ 区分大小写

::: tip
甚至允许非英文字母，从技术上来讲，这样是没问题的，但是使用英文字母是惯例
:::

## 变量和常量

通过关键字`var`声明，由于 JavaScript 是动态语言，不需要显式声明变量的数据类型且初始化，数据类型可以随意改变，通过`=`运算符赋值

```js
var foo;
var bar = 3;
```

::: danger
即使不使用`var`也可以直接使用变量名，当解释程序遇到未声明过的变量时，会用该变量名创建一个全局变量，并将其初始化为指定的值，不过这样做非常危险
:::

为了避免上述问题，在 ES6 中新增了`let`关键字，它和`var`大致相同，但`let`是最推荐的变量声明方式

```js
let foo;
let bar = 3;
```

let 和 var 的区别是：

1. `var`没有块级作用域，只有函数作用域以及全局作用域，所以它会穿透一些代码块，而`let`是具有局部作用域的
2. `var`允许同一个作用于下重新声明，而`let`是不允许的
3. `var`声明的变量会被提前，可以在声明前使用变量，而`let`是不会的

::: danger 变量提升
如果使用了一个未声明的变量，且在后面进行了声明，解释器会将该变量的声明进行提前。如果在声明的同时赋值，赋值操作不会被提前
:::

ES5 前不支持常量定义，但可以通过`Object.defineProperty()`间接实现定义常量，后在 ES6 中支持以`const`关键字定义常量（只读变量），它和`let`成为了声明变量的主要方式

```js
const PI = 3.14;
```

## 数据类型

JavaScript 有八种基本的数据类型（七种原始类型，一种复杂类型）

+ Number：代表整数和浮点数，除了常规的数字，还包括一些“特殊数值”也属于这种类型：`Infinity`,`-Infinity`,`NaN`，在 JavaScript 中做数学运算是足够安全的，不会因为任何错误而停止，最坏的情况下会得到`NaN`
+ Bigint：任意长的整数
+ String：由`'`、`"`、<code>``</code>括起来的字符序列，单引号和双引号是没什么区别的，但反引号是扩展功能，允许将变量或表达式通过`${...}`嵌入字符串中
+ Boolean：只有两个取值`true`和`false`，在适当的时候会转换为`1`和`0`
+ Null：特殊值`null`不属于任何类型，它是一个独立的类型，只有一个`null`值，JavaScript 中的`null`仅代表“无”、“空”或“未知值”的特殊值
+ Undefined：`undefined`和`null`一样是独立的类型，当一个变量未赋值时，值就是`undefined`
+ Symbol：用于唯一的标识符
+ Object：更复杂的数据结构

::: tip 不同进制的数值表示
二进制用`0b`表示  
八进制用`0`表示  
十进制不需要添加任何额外符号  
十六进制用`0x`表示
:::

### 类型检测

+ typeof：返回该一个说明数据类型的字符串，支持`typeof x`和`typeof(x)`两种形式
+ instanceof：检测一个对象是否为另一个对象的实例

::: tip
如果用 typeof 检测 `null`会得到`object`，这是一个设计上的错误，实际上它并不是`object`
:::

### 类型转换

大多数情况下 JavaScript 会将值转换为正确的类型，但某些时候需要将值显示的转换

+ 字符串转换：String(value) 将一个值转换为字符串类型
+ 数字转换：Number(value) 将一个值转换为数字类型，这个值应该是一个有效数字，比如将字符串数字转化为数值数字，否则就会返回`NaN`
+ 布尔转换：Boolean(value) 将一个值转换为布尔类型，直观上是“空”（0、空串、null、undefined、NaN）的值会变成`false`，剩下的都是`null`

::: danger
包含`0`的字符串是`true`，JavaScript 中的非空字符串总是`true`
:::

## 运算符

### 数学运算

符号|作用
---|---
+|加法
-|减法
*|乘法
/|除法
%|取余
**|求幂

通常情况下加法用于求和，但是如果用于字符串，它将连接字符串，在表达式中如果有一个字符串，那么都会被转换为字符串，但运算符是按照顺序工作的

```js
1 + 1 + '1' // '21' 而不是 '111'
```

`+`有时候也可作为一元运算符，这会将值合法的转换数字类型，它的效果和`Number()`相同，但更加简短

```js
+true // 1
+"" // 0
```

### 赋值

`=`也是一个运算符，只不过优先级比较低，所以总是等到其他表达式运算完成才轮到`=`，`=`不仅仅可以赋值，也会返回一个值，这很有趣，但是不要使用这样的代码

```js
let foo = 1;
let bar = 2;
let qux = 3 - (foo = bar + 1);
console.log(foo); // 3
console.log(bar); // 0
```

当然也支持链式赋值，尽量不使用这种方式，因为可读性变差了

```js
let foo, bar;
foo = bar = 5;
console.log(foo); // 5
console.log(bar); // 5
```

不仅如此，还支持原地修改，因为经常需要对某个变量进行允许，且存储在该变量中，类似于一种“修改并赋值”的操作

```js
let foo = 2;
foo = foo + 2;
foo = foo * 2;
```

可以使用`+=`和`*=`来表示

```js
foo += 2; // 等同于 foo = foo + 2
foo *= 2; // 等同于 foo = foo * 2
```

也支持自增自减，优先级比大多数运算符高

### 比较

所有的比较运算符返回结果都为布尔值

在进行字符串的大小比较时，会按字符逐个进行比较（字符编码）

```js
'z' > 'a' // true
'abd' > 'abc' // true
'aaa' > 'aa' // true
```

规则如下：

1. 先比较首字符的大小，如果相等进入下一步
2. 比较后面的字符，如果相等则继续向后比较，直至完成
3. 如果两个字符串的字符都用完了，则代表相等，否则未结束的字符串更大

对于不同类型的比较，会将值转换为数值再判定大小

```js
'3' > 1 // true，'3' 被转换为数字 2
'03' > 1 // true，'03' 被转换为数字 1
```

对布尔类型而言，`true`会被转换为`1`,`false`则被转换为`0`

```js
true == 1 // true
false == 0 // true
```

普通的`==`会出现一个问题，它不能区分`0`和`false`和`''`和`false`，这是因为`==`再比较时会先转换类型，如果要严格区分类型再比较就可以使用`===`

`===`不会做任何类型的转换，同样的`!==`也表示“严格不等于”

在非严格相等`==`下，`null`和`undefined`相等，但各不等于其他值

::: tip 特殊值的比较
`NaN`和`NaN`比较会返回`false`，无论是否全等，但是`null`和`undefined`进行`==`比较时会返回`true`，而进行`===`比较时会返回`false`
:::

### 逻辑运算

JavaScript 有三个逻辑运算符：`||`，`&&`，`!`，它们可以用于任何类型

`||`不仅有传统编程上的作用，在 JavaScript 中还有着更为特殊的用法

```js
let result = value1 || value2 || value3;
```

在上述示例中`||`做了这样的事：

1. 从左到右计算操作数
2. 处理操作数时，将其转换为布尔值，如果为`true`则停止计算，并返回当前操作数
3. 如果所有的操作数都是`false`，那么就返回最后一个操作数

返回的是操作数的原值，不会做任何类型转换，这个特性产生了很多有趣的用法

`&&`在传统的编程中，只有操作数都是`true`时才会返回`true`，像`||`一样，操作数可以是任意类型的值，当然它也有着特殊的用法

```js
let result = value1 && value2 && value3;
```

`&&`做了这样的事：

1. 从左到右计算操作数
2. 处理操作数时，将其转换为布尔值，如果为`false`则停止计算，并返回当前操作数
3. 如果所有的操作数都是`true`，那么就返回最后一个操作数

与`||`不同的是，`&&`会返回第一个计算为`false`的操作数

::: danger
不要用`||`或`&&`取代`if`
:::

`!`运算就更加简单了，它只接收一个参数，然后将参数转换为布尔值，并进行取反

```js
!true // false
```

使用`!!`可以将某个值转换为布尔值，也就是取反再取反

```js
!!1 // true 
```

`!`比`&&`和`||`优先级高，`||`是优先级最低的

### 空值合并

空值合并的写法为`a ?? b`，如果`a`是已定义的，则结果为`a`，如果`a`不是已定义的，则结果为`b`

通常`??`的使用场景，是为可能未定义的变量提供一个默认值

```js
let user = 'foo';
console.log(user ?? "bar");
```

`??`似乎和`||`用法相同，，但`||`是长期存在，并被长期的用于这种目的，另一方面，`??`是最近才被添加到 JavaScript 中的，他们最重要的区别是：

1. `||`返回第一个为真的值
2. `??`返回第一个已经定义的值

`||`无法区分`false`，`0`，空串，以及`null/undefined`，它无法考虑下面这种情况

```js
let height = 0;
console.log(height || 100); // 100
console.log(height ?? 100); // 0
```

这种情况使用`??`才能得出正确的结果

出于安全，JavaScript 禁止`??`和`||`以及`&&`连用，除非明确的使用括号来指定优先级

```js
let x = 1 && 2 ?? 3; // Syntax error
let y = (1 && 2) ?? 3; // ok
```

## 流程控制

### 条件分支

`if`语句和条件运算符`?`均可实现根据条件来执行不同的语句

```js
if (2 == 2) console.log('2 == 2');
```

在上面这个例子中，括号中的表达式会被转换为布尔值，为`true`则执行，`false`则不执行，这仅执行一条语句，如果想执行多条，必须将执行的语句放在`{}`中

```js
if (2 == 2) {
  console.log('1');
  console.log('2');
}
```

::: tip
当省略`{}`时，`if`只会决定紧跟后面的一条语句是否执行
:::

`if`也可以包含一个可选的`case`块，如果条件不成立，就会执行`case`代码块中的语句

```js
let age = 17;
if (age > 18) {
  console.log('成年了');
} else {
  console.log('还未成年呢');
}
```

在这个基础上还能使用`else if`产生更多的条件分支

```js
let score = 59;
if (score >= 90) {
  console.log('你太优秀了');
} else if (score >= 60) {
  console.log('刚刚及格');
} else {
  console.log('继续努力');
}
```

::: tip
最后一个`else`是可选的
:::

有时候需要更加简单的方式达到目的，而条件运算符可以帮忙做到这一点

```js
let ageFlag;
let age;
if (age > 18) {
  ageFlag = true;
} else {
  ageFlag = false;
}
```

使用`?`

```js
let age;
let ageFlag = age > 18 ? true : false;
```

条件运算符的语法是`let result = condition ? value1 : value2`，当`condition`为`true`则返回`value1`，否则返回`value2`

虽然`?`可替代`if`，但是可读性较差，当需要根据条件返回值时就使用`?`，当需要执行不同的代码结构时应该使用`if`

### 循环

JavaScript 有`while`，`do...while`，`for`三种循环结构

这是`while`的循环结构，当`condition`为真时，就会执行循环体中的语句，循环体的每一次执行叫做**迭代**

```javascipt
while (condition) {
  // 代码
}
```

这是`do...while`的循环结构，它将检查条件移动到下方，这导致无论如何循环体都会执行一次，然后再判断条件

```js
do {
  // 代码
} while
```

`for`循环是最为复杂的，但也是最常用的

```js
for (begin; condition; step){
  // code
}
```

它的工作步骤为：

1. 进入循环，`begin`先执行一次
2. 检查`condition`，为真则执行循环体
3. 然后执行`step`
4. 一直重复 2，3 步骤，直到`condition`不满足

在`begin`处声明的变量只对循环体可见，`for`语句中的任何语句段都可以省略，比如当`(;;)`时就是一个死循环

`break`语句用于终止整个循环，`continue`是`break`的轻量版本，不会终止整个循环，而是终止当前的迭代，并强制执行新的一轮循环

### switch

switch 可以代替多个`if`，它比`if`描述的更加形象

switch 至少有一个`case`代码块和一个可选的`default`代码块

```js
switch (x) {
  case 'value1':
    // code
    break;
  case 'value2':
    // code
    break;
  default:
    // code
}
```

1. `x`会先和第一个`case`的值进行比较是否严格相等，然后比较第二个，以此类推
2. 如果相等，就会执行`case`对应的代码块，直到遇到最近的`break`
3. 如果都没有符合`case`，则执行`default`代码块

`switch`和`case`允许任意表达式

::: danger
如果没有`break`，将不会检查，就会执行下一个`case`中的代码块
:::

`case`也有分组的能力

```js
let a = 3;

switch (a) {
  case 4:
    alert('Right!');
    break;

  case 3: // (*) 下面这两个 case 被分在一组
  case 5:
    alert('Wrong!');
    alert("Why don't you take a math class?");
    break;

  default:
    alert('The result is strange. Really.');
}
```

这种方式导致无论`3`还是`5`都执行的相同的代码块，且没有`break`的副作用

## 函数

使用`function`关键字声明函数，然后通过函数名调用

```js
function foo() {}
foo(); 
```

由于定义的函数是被看作全局对象`window`的方法，所以也支持这种调用方式：

```js
function foo() {}
window.foo();
window['foo'];
```

::: danger
至少在浏览器环境中是这样的，但是在其他环境中不一定
:::

函数能够嵌套定义，但是内部函数必须在嵌套它的函数作用域中调用

```js
function foo() {
  function bar() {}
  bar(); // 正确
}
bar(); // ReferenceError
```

### 局部变量和外部变量

在函数中声明的变量只在该函数内部可见

```js
function foo() {
  let a = 'a';
  console.log(a);
}
console.log(a); // undefined;
```

函数对定义在外部的变量拥有全部的访问权限

```js
let a = 'a';
function foo() {
  console.log(a);
}
console.log(a);
```

::: tip
任何声明在函数外部的变量都被称为全局变量，可被任意函数所访问，除非被局部变量覆盖
:::

当一个函数内部中的变量和外部变量重名，那么会优先使用局部变量

```js
let a = 'a';
function foo() {
  let a = 'aa';
  console.log(a); // aa
}
console.log(a); // a
```

### 参数

可定义参数将任意数据传递给函数

```js
function foo(a) {
  console.log(a); // a
}
foo('a'); 
```

如果没有提供参数，那么定义的参数默认值为`undefined`

```js
function foo(a) {
  console.log(a); // undefined
}
foo();
```

也可以在定义参数的同时指定默认值，当没有传入数据时，就会为该参数赋予默认值

```js
function foo(a = 'a') {
  console.log(a); // 'a'
}
foo();
```

最新的 JavaScript 语法还支持指定参数传值，不过为了兼容性，应该少使用

```js
function foo(a, b = 'b') {
  console.log(a); // 'a'
  console.log(b); // 'b'
}
foo(a = 'a');
```

### 返回值

`return`可以出现在函数中任意位置，后面跟上返回值，甚至不带返回值也是可以的

```js
function foo() {
  return;
}
```

::: tip
空值`return`或没有`return`语句的函数返回值均为`undefined`
:::

另外，不要在`return`与返回值之间添加新的一行，这导致 JavaScript 在`return`后面直接加了`;`，不会返回后面的表达式结果

```js
function foo() {
  return
    1 + 2 + 3
}
console.log(foo()); // undefined
```

### 函数表达式

函数在 JavaScript 中不是“语言结构”，而是一种特殊的值，还有另外一种创建函数的语法叫做**函数表达式**

```js
let foo = function() {};
foo();
```

函数像值一样被赋值给变量，为什么函数后面会有一个分号，因为这里它被看作一个表达式，只是一个特殊值而已，如果只写`foo`是不会调用函数的，只会返回函数的引用，所以需要`foo()`调用

下面这种写法也是 OK 的，看起来就像变量存储了函数，函数可以被当作值一样传递

```js
function foo () {}
let fc = foo;
fc();
```

另外这种写法也没什么区别，任然是一个函数表达式，即使增加了`func`，但它也会成为表达式的一部分，但是它允许函数内部引用自己

```js
let foo = func (){};
```

::: tip 声明函数和函数表达式的区别
声明函数可以无视定义的顺序调用，因为它会被解释器提前，而函数表达式只能在定义后调用，这不难理解，因为函数表达式只有赋值给变量后才能被引用。如果不是特殊用途不推荐使用函数表达式，因为阅读性较差，且定义更为繁琐
:::

::: tip 作为值的函数
既然函数也是引用类型，它的引用也可以被传递，访问不带`()`的函数名即可
:::

### 箭头函数

创建函数还有另一种非常简单的语法，且这种方式比函数表达式更好，它被称为**箭头函数**

```js
let foo = () => {};
```

它是下面的简单版本

```js
let foo = function (){};
```

箭头函数可以更简洁，当参数只有一个时，`()`可以省略，但没有参数的时候必须保留

```js
let double = n => { n * 2 };
console.log(double(2)); // 4
```

如果`{}`只有一行语句，也可以省略掉

```js
let double = n => n * 2
console.log(double(2)); // 4
```

除此之外还有其他有趣的特性：

1. 没有`this`，访问到的`this`来自外部的普通函数
2. 没有`arguments`，访问到的`arguments`来自外部的普通函数
3. 不能作为构造器使用`new`调用，这是不具有`this`

### 回调函数

既然函数可以传递，就产生了下面的写法

```js
function foo(callback) {
  callback();
}
function bar(){}
foo(bar);
```

从这里看来，`bar`被当作参数传递给`foo`，然后在`foo`中调用，因此`bar`被称为**回调函数**

回调函数的主要思想就是通过传递一个函数，且期望在稍后时将其进行回调

### 立即调用的函数表达式

如果在定义一个函数的同时调用这个函数，就会实现立即自执行函数的方式，为什么使用匿名函数而不是具名函数呢？因为在这种调用方式下函数有无标识都没有关系，不影响程序的执行，于是出现了下面的写法

```js
var foo = function () {}();
(function (){})();
(function (){}());
+function (){}();
-function (){}();
void function (){}();
new function (){}();

function (){}() // 抛出语法错误
```

为什么`function (){}()`抛出错误，因为解释器会将它视为一个缺标识的函数声明。而上面的匿名函数都会被看作一个表达式执行，仔细观察发现都是通过加一些额外的操作符让解释器将函数视为一个表达式，而不是函数声明，因此绕过了语法检查，这就是匿名自执行函数的本质，更准确的说法是“**立即调用的函数表达式**”

这种函数的定义方式有一个封闭的作用域范围，因此可以封装一些变量和函数，由于外部无法引用内部的变量，就能避免和全局对象的冲突。如果想要扩大作用域，可以为函数定义一个参数，将外部的定义的对象作为参数传入，并将内部的变量和函数绑定到对象上，即可实现全局变量和函数，jQuery 就是这么做的：

```js
(function (window, undefined) {
  // jQuery 逻辑实现
})(window);
```

### arguments

`arguments`是每个函数都有的属性，它是一个类数组的对象，包含传入函数中的所有参数，这是过去的一种获取所有参数的唯一办法，至今仍然有效

### 闭包函数

将一个函数作为返回值的函数就是闭包函数，它的目的是变相的扩大了局部变量的作用域，这导致在任何地方调用该函数都可以访问该作用域中的变量，下面这个例子中`temp`本质是一个局部变量，而`bar`被`foo`当作返回值在外部调用，却仍能够访问`temp`，但实际上不应该再访问到`temp`，这就是闭包函数的作用。闭包的核心就是无论在何处调用该函数，仍然能够访问它所处于环境中的变量，而这个变量是无法被其他程序访问到的

```js
function foo (){
  var temp = 'abc';
    function bar (){
      console.log(temp);
    }
    return bar;
  }
foo()();
```

### 函数也是对象

在 JavaScript 中函数本身也是一个对象，因为函数也可以作为一个值传递，必然属于某种类型，函数是一个可被调用的对象，不仅可以调用，也可以当作对象来处理，进行属性操作，以及引用传递等

那么作为对象就应该有自己的一些属性：

1. `name`：函数的名字，如果没有声明名字，则会根据上下文推测一个
2. `length`：返回函数入参的个数，但是 rest 参数不参与计数

::: tip
如果直接在一个数组中声明函数，那么该函数的`name`是无法推断的
:::

### Function 类

JavaScript 也提供了另一种创建函数的方法，只不过很少使用，所以提供了`Function`类来创建一个函数：`let func_name = new Function(arg1, arg2, ..., func_body)`，在这个形式中，每个参数都是字符串，而参数列表是可以省略的

```js
let func = new Function('a', 'b', 'return a + b');
console.log(func(1, 2)); // 3
```

与其他的方法相比，它是通过字符串创建的，允许将字符串变为函数，这种应用场景只有比较复杂的地方可以用到：从服务器种获得代码并编译成函数允许

::: tip 闭包
通常闭包指向创建函数时自身所处于的环境，但是使用`new Function()`创建的函数却指向全局环境
:::

### 内置函数

JavaScript 也内置了一些与定义的函数，用于处理一些常见的操作，预定义函数可以看作为全局对象的方法，而且常量`NaN`和`Infinity`看作它的属性，该类无需使用`new`创建，而是会在引擎初始化时被创建，方法和属性可以立即使用，且无需引用对象

`eval()`是一个很强大的函数，它的作用是计算一个字符串，并转换为对应的表达式或语句执行，它本身并不返回什么数据，目的就是执行字符串中的表达式或语句

```js
var x3 = 2;
eval('x' + 3); // 会变成 x3 ，并返回访问 x3 变量的值
eval('')
```

::: demo eval()

```html
<p id="eg1"></p>
<p id="eg2"></p>
```

```js
var x3 = 2;
document.querySelector('#eg1').innerText = eval('x' + 3);
document.querySelector('#eg2').innerText = eval(x3 + 3);
```

:::

`encodeURI()`用于将一个字符串编码成一个有效的 URI，而`decodeURI()`则是对已经编码的字符串进行解码。`encodeURIComponent()`和`decodeURIComponent()`也是如此，它们区别在于编码和解码的特殊字符

::: demo encodeURI()/encodeURIComponent() 和 decodeURI()/decodeURIComponent()

```html
<p id="eg3"></p>
<p id="eg4"></p>
<p id="eg5"></p>
<p id="eg6"></p>
```

```js
var str = 'https://wjqis.me/index?foo=张三&bar=33';
document.querySelector('#eg3').innerText = encodeURI(str);
document.querySelector('#eg4').innerText = decodeURI(encodeURI(str));
document.querySelector('#eg5').innerText = encodeURIComponent(str);
document.querySelector('#eg6').innerText = decodeURIComponent(encodeURIComponent(str));
```

:::

::: tip
这对于向服务器发送数据有用，因为用户输入的数据可能包含一些非法的数据导致服务器无法解析，所以要进行编码
:::

parseFloat()：将一个字符串转换为浮点数，而且会解析字符串中的数字，直到不是数字部分的字符，如果字符串不是以一个有效的数字开头，则返回`NaN`，有效数字前的空格会被忽略

::: demo parseFloat()

```html
<p id="eg7"></p>
<p id="eg8"></p>
<p id="eg9"></p>
```

```js
var foo = '2.3';
var bar = '174cm';
var qux = 'hello';
document.querySelector('#eg7').innerText = parseFloat(foo)
document.querySelector('#eg8').innerText = parseFloat(bar)
document.querySelector('#eg9').innerText = parseFloat(qux)
```

:::

+ parseInt()：将一个字符串转换为整数，如果不能转换将会返回`NaN`
+ String()：将一个对象转换为字符串，如果是`undefined`则返回`undefined`
+ Number()：将一个对象转换为数值，如果是`undefined`则返回`NaN`
+ Boolean()：将一个对象转换为逻辑值
+ isFinite()：判断一个数值是否为有限数，是就返回`true`
+ isNaN()：判断一个数值是否不是数字，是就是返回`true`

## 严格模式

JavaScript 是不断发展的，但没有带来任何兼容性的问题，即使新的特性被加入，旧的特性也不会改变，这么做有利于兼容旧代码，但 JavaScript 中设计的不合理的地方也被保留了下来，这种情况一直持续到 ES5 出现

严格模式是 ES5 新增的功能，虽然 ES5 可以向后兼容，如果使用严格模式，那么将会被禁止一些不再建议使用的语法，这样消除了 JavaScript 语法的一些不合理，不严谨的地方。如果在第一行声明了`"use strict"`字符串，则代表全局范围使用严格模式，也可以在函数内的第一行中使用，这样就是局部的严格模式。严格模式的兼容性非常好，在一些不被支持的浏览器中，它只会被看作一个字符串，可以大胆使用

这是一些在严格模式下的要求：

1. 变量使用前必须提前声明，且必须使用`var`关键字，否则就会抛出错误
2. 对象的属性不能够重复，且不能对只读属性赋值
3. 函数的`arguments`是只读的，参数列表不能存在同名的
4. 不能够使用`with`语句
5. `this`不再指向全局对象
6. 不再支持八进制数值

---
layout:     post
title:      javascript进阶
subtitle:   javascript进阶
date:       2024-01-15
author:     soushuuu
header-img: img/the-first.png
catalog: false
tags:
    - 前端
---


### 客户端 JavaScript 框架

[客户端 JavaScript 框架](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks)



### 客户端 Web API

[客户端 Web API](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs)



### 语言概述(重新介绍 JavaScript)

[语言概述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Language_overview)

[世界上被人误解最深的编程语言](https://crockford.com/javascript/javascript.html)


在 1995 年 Netscape 一位名为 Brendan Eich 的工程师创造了 JavaScript，随后在 1996 年初，JavaScript 首先被应用于 Netscape 2 浏览器上。最初的 JavaScript 名为 LiveScript，但是因为一个糟糕的营销策略而被重新命名，该策略企图利用 Sun Microsystem 的 Java 语言的流行性，将它的名字从最初的 LiveScript 更改为 JavaScript——尽管两者之间并没有什么共同点。这便是之后混淆产生的根源。

几个月后，Microsoft 随 IE 3 发布推出了一个与之基本兼容的语言 JScript。又过了几个月，Netscape 将 JavaScript 提交至 [Ecma International](https://www.ecma-international.org/)（一个欧洲标准化组织），[ECMAScript](https://developer.mozilla.org/zh-CN/docs/Glossary/ECMAScript) 标准第一版便在 1997 年诞生了，随后在 1999 年以 [ECMAScript 第三版](https://www.ecma-international.org/publications/standards/Ecma-262.htm)的形式进行了更新，从那之后这个标准没有发生过大的改动。由于委员会在语言特性的讨论上发生分歧，ECMAScript 第四版尚未推出便被废除，但随后于 2009 年 12 月发布的 ECMAScript 第五版引入了第四版草案加入的许多特性。第六版标准已经于 2015 年 6 月发布。

与大多数编程语言不同，JavaScript 没有输入或输出的概念。它是一个在宿主环境（host environment）下运行的脚本语言，任何与外界沟通的机制都是由宿主环境提供的。浏览器是最常见的宿主环境，但在非常多的其他程序中也包含 JavaScript 解释器，如 Adobe Acrobat、Adobe Photoshop、SVG 图像、Yahoo！的 Widget 引擎，Node.js 之类的服务器端环境，NoSQL 数据库（如开源的 Apache CouchDB）、嵌入式计算机，以及包括 GNOME（注：GNU/Linux 上最流行的 GUI 之一）在内的桌面环境等等。


#### 概览

JavaScript 是一种多范式的动态语言，它包含类型、运算符、标准内置（built-in）对象和方法。它的语法来源于 Java 和 C，所以这两种语言的许多语法特性同样适用于 JavaScript。JavaScript 通过原型链而不是类来支持面向对象编程（有关 ES6 类的内容参考这里[Classes](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)，有关对象原型参考见此[继承与原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)）。JavaScript 同样支持函数式编程——因为它们也是对象，函数也可以被保存在变量中，并且像其他对象一样被传递。


#### 类型

先从任何编程语言都不可缺少的组成部分——“类型”开始。JavaScript 程序可以修改值（value），这些值都有各自的类型。JavaScript 中的类型包括：

- Number（数字）
- String（字符串）
- Boolean（布尔）
    JavaScript 按照如下规则将变量转换成布尔类型：
    - false、0、空字符串（""）、NaN、null 和 undefined 被转换为 false
    - 所有其他值被转换为 true
- Symbol（符号）（ES2015 新增）
- Object（对象）
    - Function（函数）
    - Array（数组）
    - Date（日期）
    - RegExp（正则表达式）
- null（空）
- undefined（未定义）
    undefined 实际上是一个不允许修改的常量。

JavaScript 还有一种内置的 Error（错误）类型。


#### 变量

在 JavaScript 中声明一个新变量的方法是使用关键字 let 、const 和 var：
let 语句声明一个`块级作用域`的本地变量，并且可选的将其初始化为一个值。

const 允许声明一个不可变的常量。这个常量在`定义域`内总是可见的。

var 是最常见的声明变量的关键字。它没有其他两个关键字的种种限制。这是因为它是传统上在 JavaScript 声明变量的唯一方法。使用 var 声明的变量`在它所声明的整个函数`都是可见的。

如果声明了一个变量却没有对其赋值，那么这个变量的类型就是 undefined。

JavaScript 与其他语言的（如 Java）的重要区别是在 JavaScript 中语句块（blocks）是没有作用域的，只有函数有作用域。因此如果在一个复合语句中（如 if 控制结构中）使用 var 声明一个变量，那么它的作用域是整个函数（复合语句在函数中）。但是从 ECMAScript Edition 6 开始将有所不同的， let 和 const 关键字允许你创建块作用域的变量。



#### 运算符

JavaScript 的算术操作符包括 +、-、*、/ 和 %——求余（与模运算相同）。赋值使用 = 运算符，此外还有一些复合运算符，如 += 和 -=，它们等价于 x = x operator y。

可以使用 ++ 和 -- 分别实现变量的自增和自减。两者都可以作为前缀或后缀操作符使用。

`+` 操作符 (en-US) 还可以用来连接字符串

JavaScript 中的比较操作 (en-US)使用 <、>、<= 和 >=，这些运算符对于数字和字符串都通用。

由两个“=（等号）”组成的相等运算符有类型自适应的功能，具体例子如下：

```js
123 == "123"; // true
1 == true; // true
```

如果在比较前不需要自动类型转换，应该使用由三个“=（等号）”组成的相等运算符：

```js
1 === true; //false
123 === "123"; // false
```

JavaScript 还支持 != 和 !== 两种不等运算符，具体区别与两种相等运算符的区别类似。

JavaScript 还提供了 [位操作符 (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)。



#### 控制结构

JavaScript 的控制结构与其他类 C 语言类似。可以使用 if 和 else 来定义条件语句
```js
var name = "kittens";
if (name == "puppies") {
  name += "!";
} else if (name == "kittens") {
  name += "!!";
} else {
  name = "!" + name;
}
name == "kittens!!"; // true
```


JavaScript 支持 while 循环和 do-while 循环。前者适合常见的基本循环操作，如果需要循环体至少被执行一次则可以使用 do-while
```js
while (true) {
  // 一个无限循环！
}

var input;
do {
  input = get_input();
} while (inputIsNotValid(input));
```


JavaScript 的 for 循环与 C 和 Java 中的相同：使用时可以在一行代码中提供控制信息。
```js
for (var i = 0; i < 5; i++) {
  // 将会执行五次
}
```


JavaScript 也还包括其他两种重要的 for 循环： for...of 和 for...in 
```js
for (let value of array) {
  // do something with value
}


for (let property in object) {
  // do something with object property
}
```

&& 和 || 运算符使用短路逻辑（short-circuit logic），是否会执行第二个语句（操作数）取决于第一个操作数的结果。在需要访问某个对象的属性时，使用这个特性可以事先检测该对象是否为空
```js
var name = o && o.getName();
```
或用于缓存值（当错误值无效时）：
```js
var name = cachedName || (cachedName = getName());
```

类似地，JavaScript 也有一个用于条件表达式的三元操作符：
```js
var allowed = age > 18 ? "yes" : "no";
```

在需要多重分支时可以使用基于一个数字或字符串的 switch 语句
在 switch 的表达式和 case 的表达式是使用 === 严格相等运算符进行比较的
```js
switch (action) {
  case "draw":
    drawIt();
    break;
  case "eat":
    eatIt();
    break;
  default:
    doNothing();
}

switch (a) {
  case 1: // 继续向下
  case 2:
    eatIt();
    break;
  default:
    doNothing();
}

switch (1 + 3) {
  case 2 + 2:
    yay();
    break;
  default:
    neverhappens();
}
```


#### 对象

JavaScript 中的对象，Object，可以简单理解成“名称 - 值”对（而不是键值对：现在，ES 2015 的映射表（Map），比对象更接近键值对），不难联想 JavaScript 中的对象与下面这些概念类似：

- Python 中的字典（Dictionary）
- Perl 和 Ruby 中的散列/哈希（Hash）
- C/C++ 中的散列表（Hash table）
- Java 中的散列映射表（HashMap）
- PHP 中的关联数组（Associative array）

“名称”部分是一个 JavaScript 字符串，“值”部分可以是任何 JavaScript 的数据类型——包括对象。这使用户可以根据具体需求，创建出相当复杂的数据结构。

有两种简单方法可以创建一个空对象：
```js
var obj = new Object();

var obj = {};
```

对象的属性可以通过链式（chain）表示方法进行访问：
```js
obj.details.color; // orange
obj["details"]["size"]; // 12
```

对象属性可以通过如下两种方式进行赋值和访问：
```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// 定义一个对象
var You = new Person("You", 24);
// 我们创建了一个新的 Person，名称是 "You"
// ("You" 是第一个参数，24 是第二个参数..)



// 点表示法 (dot notation)
obj.name = "Simon";
var name = obj.name;


// 括号表示法 (bracket notation)
obj["name"] = "Simon";
var name = obj["name"];
// can use a variable to define a key
var user = prompt("what is your key?");
obj[user] = prompt("what is its value?");
```


#### 数组


JavaScript 中的数组是一种特殊的对象。它的工作原理与普通对象类似（以数字为属性名，但只能通过[] 来访问），但数组还有一个特殊的属性——length（长度）属性。这个属性的值通常比数组 __最大索引__ 大 1。


创建数组的方法
```js
var a = new Array();
a[0] = "dog";
a[1] = "cat";
a[2] = "hen";
a.length; // 3


var a = ["dog", "cat", "hen"];
a.length; // 3

var a = ["dog", "cat", "hen"];
a[100] = "fox";
a.length; // 101
```

遍历一个数组：
```js
for (var i = 0; i < a.length; i++) {
  // Do something with a[i]
}

for (const currentValue of a) {
  // Do something with currentValue
}

// 并不是遍历数组元素而是数组的索引。不推荐
for (var i in a) {
  // 操作 a[i]
}


["dog", "cat", "hen"].forEach(function (currentValue, index, array) {
  // 操作 currentValue 或者 array[index]
});

```

[Array（数组）类还自带方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)



#### 函数

学习 JavaScript 最重要的就是要理解对象和函数两个部分。

```js
function avg(...args) {
  var sum = 0;
  for (let value of args) {
    sum += value;
  }
  return sum / args.length;
}

avg(2, 3, 4, 5); // 3.5
```

JavaScript 允许你通过任意函数对象的 apply() 方法来传递给它一个数组作为参数列表。
apply() 的第一个参数应该是一个被当作 this 来看待的对象。

```js
avg.apply(null, [2, 3, 4, 5]); // 3.5
```

JavaScript 允许你创建匿名函数：

```js
var avg = function () {
  var sum = 0;
  for (var i = 0, j = arguments.length; i < j; i++) {
    sum += arguments[i];
  }
  return sum / arguments.length;
};
```

命名立即调用的函数表达式（IIFE——Immediately Invoked Function Expression），如下所示：

```js
var charsInBody = (function counter(elm) {
  if (elm.nodeType == 3) {
    // 文本节点
    return elm.nodeValue.length;
  }
  var count = 0;
  for (var i = 0, child; (child = elm.childNodes[i]); i++) {
    count += counter(child);
  }
  return count;
})(document.body);
```



#### 自定义对象


在经典的面向对象语言中，对象是指数据和在这些数据上进行的操作的集合。与 C++ 和 Java 不同，JavaScript 是一种基于原型的编程语言，并没有 class 语句，而是把函数用作类。那么让我们来定义一个人名对象，这个对象包括人的姓和名两个域（field）。名字的表示有两种方法：“名 姓（First Last）”或“姓，名（Last, First）”。使用我们前面讨论过的函数和对象概念，可以像这样完成定义：


```js
function makePerson(first, last) {
  return {
    first: first,
    last: last,
  };
}
function personFullName(person) {
  return person.first + " " + person.last;
}
function personFullNameReversed(person) {
  return person.last + ", " + person.first;
}

var s = makePerson("Simon", "Willison");
personFullName(s); // "Simon Willison"
personFullNameReversed(s); // "Willison, Simon"

function makePerson(first, last) {
  return {
    first: first,
    last: last,
    fullName: function () {
      return this.first + " " + this.last;
    },
    fullNameReversed: function () {
      return this.last + ", " + this.first;
    },
  };
}
s = makePerson("Simon", "Willison");
s.fullName(); // "Simon Willison"
s.fullNameReversed(); // Willison, Simon
```

Person.prototype 是一个可以被 Person 的所有实例共享的对象。它是一个名叫原型链（prototype chain）的查询链的一部分：当你试图访问 Person 某个实例（例如上个例子中的 s）一个没有定义的属性时，解释器会首先检查这个 Person.prototype 来判断是否存在这样一个属性。所以，任何分配给 Person.prototype 的东西对通过 this 对象构造的实例都是可用的。

这个特性功能十分强大，JavaScript 允许你在程序中的任何时候修改原型（prototype）中的一些东西，也就是说你可以在运行时 (runtime) 给已存在的对象添加额外的方法：

```js
s = new Person("Simon", "Willison");
s.firstNameCaps(); // TypeError on line 1: s.firstNameCaps is not a function

Person.prototype.firstNameCaps = function () {
  return this.first.toUpperCase();
};
s.firstNameCaps(); // SIMON
```

apply() 的第一个参数应该是一个被当作 this 来看待的对象。

```js
function trivialNew(constructor, ...args) {
  var o = {}; // 创建一个对象
  constructor.apply(o, args);
  return o;
}
```

apply() 有一个姐妹函数，名叫 call，它也可以允许你设置 this，但它带有一个扩展的参数列表而不是一个数组。

```js
function lastNameCaps() {
  return this.last.toUpperCase();
}
var s = new Person("Simon", "Willison");
lastNameCaps.call(s);
// 和以下方式等价
s.lastNameCaps = lastNameCaps;
s.lastNameCaps();
```

##### 内部函数

JavaScript 允许在一个函数内部定义函数，关于 JavaScript 中的嵌套函数，一个很重要的细节是，它们可以访问父函数作用域中的变量：

```js
function parentFunc() {
  var a = 1;

  function nestedFunc() {
    var b = 4; // parentFunc 无法访问 b
    return a + b;
  }
  return nestedFunc(); // 5
}
```

如果某个函数依赖于其他的一两个函数，而这一两个函数对你其余的代码没有用处，你可以将它们嵌套在会被调用的那个函数内部，这样做可以减少全局作用域下的函数的数量，这有利于编写易于维护的代码。

这也是一个减少使用全局变量的好方法。当编写复杂代码时，程序员往往试图使用全局变量，将值共享给多个函数，但这样做会使代码很难维护。内部函数可以共享父函数的变量，所以你可以使用这个特性把一些函数捆绑在一起，这样可以有效地防止“污染”你的全局命名空间——你可以称它为“局部全局（local global）”。




#### 闭包

闭包是 JavaScript 中最强大的抽象概念之一——但它也是最容易造成困惑的。

```js
function makeAdder(a) {
  return function (b) {
    return a + b;
  };
}
var add5 = makeAdder(5);
var add20 = makeAdder(20);
add5(6); // ?
add20(7); // ?
```

这里发生的事情和前面介绍过的内嵌函数十分相似：一个函数被定义在了另外一个函数的内部，内部函数可以访问外部函数的变量。唯一的不同是，外部函数已经返回了，那么常识告诉我们局部变量“应该”不再存在。但是它们却仍然存在——否则 adder 函数将不能工作。也就是说，这里存在 makeAdder 的局部变量的两个不同的“副本”——一个是 a 等于 5，另一个是 a 等于 20。那些函数的运行结果就如下所示：
```js
add5(6); // 返回 11
add20(7); // 返回 27
```


下面来说说，到底发生了什么了不得的事情。每当 JavaScript 执行一个函数时，都会创建一个作用域对象（scope object），用来保存在这个函数中创建的局部变量。它使用一切被传入函数的变量进行初始化（初始化后，它包含一切被传入函数的变量）。这与那些保存的所有全局变量和函数的全局对象（global object）相类似，但仍有一些很重要的区别：第一，每次函数被执行的时候，就会创建一个新的，特定的作用域对象；第二，与全局对象（如浏览器的 window 对象）不同的是，你不能从 JavaScript 代码中直接访问作用域对象，也没有 可以遍历当前作用域对象中的属性 的方法。

所以，当调用 makeAdder 时，解释器创建了一个作用域对象，它带有一个属性：a，这个属性被当作参数传入 makeAdder 函数。然后 makeAdder 返回一个新创建的函数（暂记为 adder）。通常，JavaScript 的垃圾回收器会在这时回收 makeAdder 创建的作用域对象（暂记为 b），但是，makeAdder 的返回值，新函数 adder，拥有一个指向作用域对象 b 的引用。最终，作用域对象 b 不会被垃圾回收器回收，直到没有任何引用指向新函数 adder。

作用域对象组成了一个名为作用域链（scope chain）的（调用）链。它和 JavaScript 的对象系统使用的原型（prototype）链相类似。

一个闭包，就是 一个函数 与其 被创建时所带有的作用域对象 的组合。闭包允许你保存状态——所以，它们可以用来代替对象。[关于闭包的详细介绍](https://stackoverflow.com/questions/111102/how-do-javascript-closures-work)。



### JavaScript 数据类型和数据结构


#### 动态和弱类型

JavaScript 是一种有着动态类型的动态语言。JavaScript 中的变量与任何特定值类型没有任何关联，任何变量都可以被赋予（和重新赋予）各种类型的值：
```js
let foo = 42; // foo 现在是一个数值
foo = "bar"; // foo 现在是一个字符串
foo = true; // foo 现在是一个布尔值
```

JavaScript 也是一个弱类型语言，这意味着当操作涉及不匹配的类型时，它允许隐式类型转换，而不是抛出类型错误。
```js
const foo = 42; // foo 现在是一个数值
const result = foo + "1"; // JavaScript 将 foo 强制转换为字符串，因此可以将其与另一个操作数连接起来
console.log(result); // 421
```

对于 symbol 和 BigInt，JavaScript 有意禁止了某些隐式类型转换。



#### 原始值

除了 Object 以外，所有类型都定义了表示在语言最低层面的不可变值。我们将这些值称为原始值。

除了 null，所有原始类型都可以使用 typeof 运算符进行测试。typeof null 返回 "object"，因此必须使用 === null 来测试 null。

除了 null 和 undefined，所有原始类型都有它们相应的对象包装类型，这为处理原始值提供可用的方法。

```
类型	    typeof 返回值	对象包装器
Null	    "object"	    不适用
Undefined	"undefined"	    不适用
Boolean	    "boolean"	    Boolean
Number	    "number"	    Number
BigInt	    "bigint"	    BigInt
String	    "string"	    String
Symbol	    "symbol"	    Symbol
```

##### Null 类型

Null 类型只有一个值：null。


##### undefined 类型

Undefined 类型只有一个值：undefined。

从概念上讲，undefined 表示值的缺失，null 表示对象的缺失（这也可以说明 typeof null === "object" 的原因）。当某些东西没有值时，该语言通常默认为 undefined：

- 没有值（return;）的 return 语句，隐式返回 undefined。
- 访问不存在的对象属性（obj.iDontExist），返回 undefined。
- 变量声明时没有初始化（let x;），隐式初始化为 undefined。
- 许多像 Array.prototype.find() 和 Map.prototype.get() 的方法，当没有找到元素时，返回 undefined。


null 是一个关键字，但是 undefined 是一个普通的标识符，恰好是一个全局属性。在实践中，这两个差异很小，因为 undefined 不应该被重新定义或者遮蔽。


##### Boolean 类型

Boolean 类型表示一个逻辑实体并且包括两个值：true 和 false。

布尔值通常用于条件运算，包括三元运算符、if...else、while 等。


##### Number 类型

Number 类型是一种[基于 IEEE 754 标准的双精度 64 位二进制格式的值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number#number_%E7%BC%96%E7%A0%81)。

±(2^-1074 ~ 2^1024) 范围之外的值会自动转换：

- 大于 Number.MAX_VALUE 的正值被转换为 +Infinity。
- 小于 Number.MIN_VALUE 的正值被转换为 +0。
- 小于 -Number.MAX_VALUE 的负值被转换为 -Infinity。
- 大于 -Number.MIN_VALUE 的负值被转换为 -0。

NaN（“Not a Number”）是一个特殊种类的数值，当算术运算的结果不表示数值时，通常会遇到它。它也是 JavaScript 中唯一 __不等于自身__ 的值。


##### BigInt 类型

BigInt 类型在 Javascript 中是一个数字的原始值，它可以表示任意大小的整数。使用 BigInt，你可以安全地存储和操作巨大的整数，甚至超过 Number 的安全整数限制（Number.MAX_SAFE_INTEGER）。

BigInt 是通过将 n 附加到整数末尾或调用 BigInt() 函数来创建的。

你可以使用大多数运算符处理 BigInt，包括 +、*、-、** 和 %。——唯一被禁止的是 >>>。BigInt 并不是严格等于有着相同数学值的 Number，而是宽松的相等。

BigInt 值并不总是更精确的，也不总是比 number 精确，因为 BigInt 不能表示小数，但可以更精确地表示大整数。这两种类型都不能相互替代。如果 BigInt 值在算术表达式中与常规 number 值混合，或者它们相互隐式转换，则抛出 TypeError。


##### String 类型

String 类型表示文本数据并编码为 UTF-16 码元的 16 位无符号整数值序列。字符串中的每个元素在字符串中占据一个位置。第一个元素的索引为 0，下一个是索引 1，依此类推。字符串的长度是它的元素的数量。

JavaScript 字符串是不可变的。这意味着一旦字符串被创建，就不可能修改它。字符串方法基于当前字符串的内容创建一个新的字符串——例如：

- 使用 substring() 获取原始的子字符串。
- 使用串联运算符（+）或 concat() 将两个字符串串联。


##### Symbol 类型

[Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 是唯一并且不可变的原始值并且可以用来作为对象属性的键（如下）。在某些程序语言当中，Symbol 也被称作“原子（atom）类型”。symbol 的目的是去创建一个唯一属性键，保证不会与其他代码中的键产生冲突。


#### Object

在计算机科学中，对象（object）是指内存中的可以被标识符引用的一块区域。在 JavaScript 中，对象是唯一可变的值。事实上，函数也是具有额外可调用能力的对象。


##### 属性

在 JavaScript 中，对象可以被看作是一组属性的集合。用对象字面量语法来定义一个对象时，会自动初始化一组有限的属性；然后，这些属性还可以被添加和移除。对象属性等价于键值对。属性键要么是字符串类型，要么是 symbol。属性值可以是任何类型的值，包括其他对象，从而可以构建复杂的数据结构。

有两种对象属性的类型：数据属性和访问器属性。每个属性都有对应的特性（attribute）。JavaScript 引擎可在内部访问每个属性，但是你可以通过 Object.defineProperty() 设置它们，或通过 Object.getOwnPropertyDescriptor() 读取它们。



##### Date

当表示日期时，最好的选择是使用在 JavaScript 内置的 Date 工具类。

##### 索引类集合：数组和类型化数组


##### 带键的集合：Map、Set、WeakMap、WeakSet


##### 结构化数据：JSON


##### 标准库中的更多对象

JavaScript 有一个内置对象的[标准库](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)。



#### 强制类型转换


#### 参见

[JavaScript 中的计算机科学](https://github.com/humanwhocodes/computer-science-in-javascript)



### JavaScript 中的相等性判断


JavaScript 提供三种不同的值比较运算：

- === ——严格相等（三个等号）
- == ——宽松相等（两个等号）
- Object.is()


#### 使用 === 进行严格相等比较

严格相等比较两个值是否相等。两个被比较的值在比较前都不进行隐式转换。如果两个被比较的值具有不同的类型，这两个值是不相等的。否则，如果两个被比较的值类型相同，值也相同，并且都不是 number 类型时，两个值相等。最后，如果两个值都是 number 类型，当两个都不是 NaN，并且数值相同，或是两个值分别为 +0 和 -0 时，两个值被认为是相等的。

```js
const num = 0;
const obj = new String("0");
const str = "0";

console.log(num === num); // true
console.log(obj === obj); // true
console.log(str === str); // true

console.log(num === obj); // false
console.log(num === str); // false
console.log(obj === str); // false
console.log(null === undefined); // false
console.log(obj === null); // false
console.log(obj === undefined); // false
```


#### 使用 == 进行宽松相等比较

宽松相等是对称的：对于任何 A 和 B 的值，A == B 总是与 B == A 具有相同的语义（除了转换应用的顺序）。使用 == 执行宽松相等的行为如下：

1. 如果操作数具有相同的类型，则按以下方式进行比较：
    - Object：仅当两个操作数引用相同的对象时，才返回 true。
    - String：仅当两个操作数具有相同的字符并且顺序相同，才返回 true。
    - Number：仅当两个操作数具有相同的值时，才返回 true。+0 和 -0 被视为相同的值。如果任一操作数为 NaN，则返回 false；因此 NaN 永远不等于 NaN。
    - Boolean：仅当操作数都是 true 或 false 时，才返回 true。
    - BigInt：仅当两个操作数具有相同的值时，才返回 true。
    - Symbol：仅当两个操作数引用相同的 symbol 时，才返回 true。
2. 如果操作数之一为 null 或 undefined，则另一个操作数必须为 null 或 undefined 才返回 true。否则返回 false。
3. 如果操作数之一是对象，而另一个是原始值，则将对象转换为原始值。
4. 在这一步骤中，两个操作数都被转换为原始值（String、Number、Boolean、Symbol 和 BigInt 之一）。剩余的转换将分情况完成。
    - 如果它们是相同类型的，则使用步骤 1 进行比较。
    - 如果操作数中有一个是 Symbol，但另一个不是，则返回 false。
    - 如果操作数之一是 Boolean，而另一个不是，则将 Boolean 转换为 Number：true 转换为 1，false 转换为 0。然后再次对两个操作数进行宽松比较。
    - Number 转 String：将 String 转换为 Number。转换失败会得到 NaN，这将确保相等性为 false。
    - Number 转 BigInt：按照其数值进行比较。如果 Number 是 ±Infinity 或 NaN，返回 false。
    - String 转 BigInt: 使用与 BigInt() 构造函数相同的算法将字符串转换为 BigInt。如果转换失败，则返回 false。

一般而言，根据 ECMAScript 规范，所有原始类型和对象都不与 undefined 和 null 宽松相等。但是大部分浏览器允许非常有限的一类对象（即，所有页面中的 document.all 对象）在某些情况下表现出模拟 undefined 值特性。宽松相等就是这些情况之一：当且仅当 A 是一个模拟 undefined 的对象时，null == A 和 undefined == A 将会计算得到 true。在其他所有情况下，一个对象都不会与 undefined 或 null 宽松相等。

在大多数情况下，不建议使用宽松相等。使用严格相等进行比较的结果更容易预测，并且由于缺少类型强制转换可以更快地执行。

```js
const num = 0;
const big = 0n;
const str = "0";
const obj = new String("0");

console.log(num == str); // true
console.log(big == num); // true
console.log(str == big); // true

console.log(num == obj); // true
console.log(big == obj); // true
console.log(str == obj); // true
```

#### 使用 Object.is() 进行同值相等比较

同值相等决定了两个值在所有上下文中是否在功能上相同。

```js
// 向 Nmuber 构造函数添加一个不可变的属性 NEGATIVE_ZERO
Object.defineProperty(Number, "NEGATIVE_ZERO", {
  value: -0,
  writable: false,
  configurable: false,
  enumerable: false,
});

function attemptMutation(v) {
  Object.defineProperty(Number, "NEGATIVE_ZERO", { value: v });
```

当尝试更改不可变属性时，Object.defineProperty 会抛出异常，但如果没有请求实际更改，则不会执行任何操作。如果 v 是 -0，则没有请求更改，也不会抛出错误。在内部，重新定义不可变属性时，使用同值相等将新指定的值与当前值进行比较。

同值相等由 Object.is 方法提供。语言内部期望一个值等于另一个时，几乎所有地方都使用同值相等。

#### 零值相等

类似于同值相等，但 +0 和 -0 被视为相等。

零值相等不作为 JavaScript API 公开，但可以通过自定义代码实现：

```js
function sameValueZero(x, y) {
  if (typeof x === "number" && typeof y === "number") {
    // x 和 y 相等（可能是 -0 和 0）或它们都是 NaN
    return x === y || (x !== x && y !== y);
  }
  return x === y;
}
```

[JS 比较表](https://dorey.github.io/JavaScript-Equality-Table/)


### 属性的可枚举性和所有权

可枚举属性是指那些内部“可枚举”标志设置为 true 的属性，对于通过直接的赋值和属性初始化的属性，该标识值默认为即为 true，对于通过 Object.defineProperty 等定义的属性，该标识值默认为 false。可枚举的属性可以通过 for...in 循环进行遍历（除非该属性名是一个 Symbol）。属性的所有权是通过判断该属性是否直接属于某个对象决定的，而不是通过原型链继承的。一个对象的所有的属性可以一次性的获取到。有一些内置的方法可以用于判断、迭代/枚举以及获取对象的一个或一组属性

[列举](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)


### 闭包

闭包（closure）是一个函数以及其捆绑的周边环境状态（lexical environment，词法环境）的引用的组合。换而言之，闭包让开发者可以从内部函数访问外部函数的作用域。在 JavaScript 中，闭包会随着函数的创建而被同时创建。



如果不是某些特定任务需要使用闭包，在其他函数中创建函数是不明智的，因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响。


### 继承与原型链

当谈到继承时，JavaScript 只有一种结构：对象。每个对象（object）都有一个私有属性指向另一个名为原型（prototype）的对象。原型对象也有一个自己的原型，层层向上直到一个对象的原型为 null。根据定义，null 没有原型，并作为这个原型链（prototype chain）中的最后一个环节。可以改变原型链中的任何成员，甚至可以在运行时换出原型，因此 JavaScript 中不存在静态分派的概念。



#### 使用不同的方法来创建对象和改变原型链

##### 使用语法结构创建对象

```js
const o = { a: 1 };
// 新创建的对象 o 以 Object.prototype 作为它的 [[Prototype]]
// Object.prototype 的原型为 null。
// o ---> Object.prototype ---> null

const b = ["yo", "whadup", "?"];
// 数组继承了 Array.prototype（具有 indexOf、forEach 等方法）
// 其原型链如下所示：
// b ---> Array.prototype ---> Object.prototype ---> null

function f() {
  return 2;
}
// 函数继承了 Function.prototype（具有 call、bind 等方法）
// f ---> Function.prototype ---> Object.prototype ---> null

const p = { b: 2, __proto__: o };
// 可以通过 __proto__ 字面量属性将新创建对象的
// [[Prototype]] 指向另一个对象。
// （不要与 Object.prototype.__proto__ 访问器混淆）
// p ---> o ---> Object.prototype ---> null
```

##### 使用构造函数

```js
function Graph() {
  this.vertices = [];
  this.edges = [];
}

Graph.prototype.addVertex = function (v) {
  this.vertices.push(v);
};

const g = new Graph();
// g 是一个带有自有属性“vertices”和“edges”的对象。
// 在执行 new Graph() 时，g.[[Prototype]] 是 Graph.prototype 的值。
```


##### 使用 Object.create()

```js
const a = { a: 1 };
// a ---> Object.prototype ---> null

const b = Object.create(a);
// b ---> a ---> Object.prototype ---> null
console.log(b.a); // 1 (inherited)

const c = Object.create(b);
// c ---> b ---> a ---> Object.prototype ---> null

const d = Object.create(null);
// d ---> null（d 是一个直接以 null 为原型的对象）
console.log(d.hasOwnProperty);
// undefined，因为 d 没有继承 Object.prototype
```


##### 使用类

```js
class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}

class Square extends Polygon {
  constructor(sideLength) {
    super(sideLength, sideLength);
  }

  get area() {
    return this.height * this.width;
  }

  set sideLength(newLength) {
    this.height = newLength;
    this.width = newLength;
  }
}

const square = new Square(2);
// square ---> Square.prototype ---> Polygon.prototype ---> Object.prototype ---> null
```


##### 使用 Object.setPrototypeOf()

```js
const obj = { a: 1 };
const anotherObj = { b: 2 };
Object.setPrototypeOf(obj, anotherObj);
// obj ---> anotherObj ---> Object.prototype ---> null
```


##### 使用 `__proto__` 访问器

所有对象都继承了 Object.prototype.`__proto__` 访问器，它可以用来设置现有对象的 [[Prototype]]（如果对象没有覆盖 `__proto__` 属性）。

```js
const obj = {};
// 请不要使用该方法：仅作为示例。
obj.__proto__ = { barProp: "bar val" };
obj.__proto__.__proto__ = { fooProp: "foo val" };
console.log(obj.fooProp);
console.log(obj.barProp);
```


JavaScript 中的所有构造函数都有一个被称为 `prototype` 的特殊属性，它与 new 运算符一起使用。对原型对象的引用被复制到新实例的内部属性 `[[Prototype]]` 中。例如，当你执行 const a1 = new A() 时，JavaScript（在内存中创建对象之后，为其定义 this 并执行 A() 之前）设置 `a1.[[Prototype]] = A.prototype`。然后，当你访问实例的属性时，JavaScript 首先检查它们是否直接存在于该对象上，如果不存在，则在 `[[Prototype]]` 中查找。会递归查询 `[[Prototype]]`，即 a1.doSomething、Object.getPrototypeOf(a1).doSomething、Object.getPrototypeOf(Object.getPrototypeOf(a1)).doSomething，以此类推，直至找到或 `Object.getPrototypeOf` 返回 `null`。这意味着在 `prototype` 上定义的所有属性实际上都由所有实例共享，并且甚至可以更改 `prototype` 的部分内容，使得更改被应用到所有现有的实例中。

除非是为了与新的 JavaScript 特性兼容，否则 __永远不应__ 扩展原生原型。

### 内存管理

像 C 语言这样的底层语言一般都有底层的内存管理接口，比如 malloc()和free()。相反，JavaScript 是在创建变量（对象，字符串等）时自动进行了分配内存，并且在不使用它们时“自动”释放。释放的过程称为垃圾回收。这个“自动”是混乱的根源，并让 JavaScript（和其他高级语言）开发者错误的感觉他们可以不关心内存管理。


#### 内存生命周期

不管什么程序语言，内存生命周期基本是一致的：

1. 分配你所需要的内存
2. 使用分配到的内存（读、写）
3. 不需要时将其释放\归还

所有语言第二部分都是明确的。第一和第三部分在底层语言中是明确的，但在像 JavaScript 这些高级语言中，大部分都是隐含的。


#### JavaScript 的内存分配

##### 值的初始化

为了不让程序员费心分配内存，JavaScript 在定义变量时就完成了内存分配。


##### 通过函数调用分配内存

有些函数调用结果是分配对象内存：

```js
var d = new Date(); // 分配一个 Date 对象

var e = document.createElement("div"); // 分配一个 DOM 元素
```

有些方法分配新变量或者新对象：

```js
var s = "azerty";
var s2 = s.substr(0, 3); // s2 是一个新的字符串
// 因为字符串是不变量，
// JavaScript 可能决定不分配内存，
// 只是存储了 [0-3] 的范围。

var a = ["ouais ouais", "nan nan"];
var a2 = ["generation", "nan nan"];
var a3 = a.concat(a2);
// 新数组有四个元素，是 a 连接 a2 的结果
```

##### 使用值

使用值的过程实际上是对分配内存进行读取与写入的操作。读取与写入可能是写入一个变量或者一个对象的属性值，甚至传递函数的参数。


##### 当内存不再需要使用时释放

高级语言解释器嵌入了“垃圾回收器”，它的主要工作是跟踪内存的分配和使用，以便当分配的内存不再使用时，自动释放它。这只能是一个近似的过程，因为要知道是否仍然需要某块内存是无法判定的（无法通过某种算法解决）。


#### 垃圾回收

如上文所述自动寻找是否一些内存“不再需要”的问题是无法判定的。因此，垃圾回收实现只能有限制的解决一般问题。本节将解释必要的概念，了解主要的垃圾回收算法和它们的局限性。


##### 引用

垃圾回收算法主要依赖于引用的概念。在内存管理的环境中，一个对象如果有访问另一个对象的权限（隐式或者显式），叫做一个对象引用另一个对象。例如，一个 Javascript 对象具有对它原型的引用（隐式引用）和对它属性的引用（显式引用）。

在这里，“对象”的概念不仅特指 JavaScript 对象，还包括函数作用域（或者全局词法作用域）。


##### 引用计数垃圾收集


这是最初级的垃圾收集算法。此算法把“对象是否不再需要”简化定义为“对象有没有其他对象引用到它”。如果没有引用指向该对象（零引用），对象将被垃圾回收机制回收。

```js
var o = {
  a: {
    b: 2,
  },
};
// 两个对象被创建，一个作为另一个的属性被引用，另一个被分配给变量 o
// 很显然，没有一个可以被垃圾收集

var o2 = o; // o2 变量是第二个对“这个对象”的引用

o = 1; // 现在，“这个对象”只有一个 o2 变量的引用了，“这个对象”的原始引用 o 已经没有

var oa = o2.a; // 引用“这个对象”的 a 属性
// 现在，“这个对象”有两个引用了，一个是 o2，一个是 oa

o2 = "yo"; // 虽然最初的对象现在已经是零引用了，可以被垃圾回收了
// 但是它的属性 a 的对象还在被 oa 引用，所以还不能回收

oa = null; // a 属性的那个对象现在也是零引用了
// 它可以被垃圾回收了
```

###### 限制：循环引用

该算法有个限制：无法处理循环引用的事例。在下面的例子中，两个对象被创建，并互相引用，形成了一个循环。它们被调用之后会离开函数作用域，所以它们已经没有用了，可以被回收了。然而，引用计数算法考虑到它们互相都有至少一次引用，所以它们不会被回收。

```js
function f() {
  var o = {};
  var o2 = {};
  o.a = o2; // o 引用 o2
  o2.a = o; // o2 引用 o

  return "azerty";
}

f();
```

##### 标记 - 清除算法


这个算法把“对象是否不再需要”简化定义为“对象是否可以获得”。

这个算法假定设置一个叫做根（root）的对象（在 Javascript 里，根是全局对象）。垃圾回收器将定期从根开始，找所有从根开始引用的对象，然后找这些对象引用的对象……从根开始，垃圾回收器将找到所有可以获得的对象和收集所有不能获得的对象。

这个算法比前一个要好，因为“有零引用的对象”总是不可获得的，但是相反却不一定，参考“循环引用”。

从 2012 年起，所有现代浏览器都使用了标记 - 清除垃圾回收算法。所有对 JavaScript 垃圾回收算法的改进都是基于标记 - 清除算法的改进，并没有改进标记 - 清除算法本身和它对“对象是否不再需要”的简化定义。


###### 限制：那些无法从根对象查询到的对象都将被清除

尽管这是一个限制，但实践中我们很少会碰到类似的情况，所以开发者不太会去关心垃圾回收机制。




### 并发模型与事件循环


JavaScript 有一个基于事件循环的并发模型，事件循环负责执行代码、收集和处理事件以及执行队列中的子任务。这个模型与其他语言中的模型截然不同，比如 C 和 Java。


#### 运行时概念


![可视化描述](../img/the_javascript_runtime_environment_example.svg)


##### 栈

函数调用形成了一个由若干帧组成的栈。

##### 堆

对象被分配在堆中，堆是一个用来表示一大块（通常是非结构化的）内存区域的计算机术语。


##### 队列

一个 JavaScript 运行时包含了一个待处理消息的消息队列。每一个消息都关联着一个用以处理这个消息的回调函数。

在 事件循环 期间的某个时刻，运行时会从最先进入队列的消息开始处理队列中的消息。被处理的消息会被移出队列，并作为输入参数来调用与之关联的函数。正如前面所提到的，调用一个函数总是会为其创造一个新的栈帧。

函数的处理会一直进行到执行栈再次为空为止；然后事件循环将会处理队列中的下一个消息（如果还有的话）。



#### 事件循环

之所以称之为 __事件循环__，是因为它经常按照类似如下的方式来被实现：

```js
while (queue.waitForMessage()) {
  queue.processNextMessage();
}
```

queue.waitForMessage() 会同步地等待消息到达 (如果当前没有任何消息等待被处理)。


##### "执行至完成"

每一个消息完整地执行后，其他消息才会被执行。这为程序的分析提供了一些优秀的特性，包括：当一个函数执行时，它不会被抢占，只有在它运行完毕之后才会去运行任何其他的代码，才能修改这个函数操作的数据。这与 C 语言不同，例如，如果函数在线程中运行，它可能在任何位置被终止，然后在另一个线程中运行其他代码。

这个模型的一个缺点在于当一个消息需要太长时间才能处理完毕时，Web 应用程序就无法处理与用户的交互，例如点击或滚动。为了缓解这个问题，浏览器一般会弹出一个“这个脚本运行时间过长”的对话框。一个良好的习惯是缩短单个消息处理时间，并在可能的情况下将一个消息裁剪成多个消息。


##### 添加消息

在浏览器里，每当一个事件发生并且有一个事件监听器绑定在该事件上时，一个消息就会被添加进消息队列。如果没有事件监听器，这个事件将会丢失。所以当一个带有点击事件处理器的元素被点击时，就会像其他事件一样产生一个类似的消息。

函数 setTimeout 接受两个参数：待加入队列的消息和一个时间值（可选，默认为 0）。这个时间值代表了消息被实际加入到队列的最小延迟时间。如果队列中没有其他消息并且栈为空，在这段延迟时间过去之后，消息会被马上处理。但是，如果有其他消息，setTimeout 消息必须等待其他消息处理完。因此第二个参数仅仅表示 __最少延迟时间__，而非确切的等待时间。

下面的例子演示了这个概念（setTimeout 并不会在计时器到期之后直接执行）：

```js
const s = new Date().getSeconds();

setTimeout(function () {
  // 输出 "2"，表示回调函数并没有在 500 毫秒之后立即执行
  console.log("Ran after " + (new Date().getSeconds() - s) + " seconds");
}, 500);

while (true) {
  if (new Date().getSeconds() - s >= 2) {
    console.log("Good, looped for 2 seconds");
    break;
  }
}
```




##### 零延迟

零延迟并不意味着回调会立即执行。以 0 为第二参数调用 setTimeout 并不表示在 0 毫秒后就立即调用回调函数。

其等待的时间取决于队列里待处理的消息数量。在下面的例子中，"这是一条消息" 将会在回调获得处理之前输出到控制台，这是因为延迟参数是运行时处理请求所需的最小等待时间，但并不保证是准确的等待时间。

基本上，setTimeout 需要等待当前队列中所有的消息都处理完毕之后才能执行，即使已经超出了由第二参数所指定的时间。

```js
(function () {
  console.log("这是开始");

  setTimeout(function cb() {
    console.log("这是来自第一个回调的消息");
  });

  console.log("这是一条消息");

  setTimeout(function cb1() {
    console.log("这是来自第二个回调的消息");
  }, 0);

  console.log("这是结束");
})();

// "这是开始"
// "这是一条消息"
// "这是结束"
// "这是来自第一个回调的消息"
// "这是来自第二个回调的消息"
```

##### 多个运行时互相通信

一个 web worker 或者一个跨域的 iframe 都有自己的栈、堆和消息队列。两个不同的运行时只能通过 [postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage) 方法进行通信。如果另一个运行时侦听 message 事件，则此方法会向该运行时添加消息。



#### 永不阻塞

JavaScript 的事件循环模型与许多其他语言不同的一个非常有趣的特性是，它永不阻塞。处理 I/O 通常通过事件和回调来执行，所以当一个应用正等待一个 [IndexedDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API) 查询返回或者一个 [XHR](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 请求返回时，它仍然可以处理其他事情，比如用户输入。

由于历史原因有一些例外，如 alert 或者同步 XHR，但应该尽量避免使用它们。注意，[例外的例外](https://stackoverflow.com/questions/2734025/is-javascript-guaranteed-to-be-single-threaded/2734311#2734311)也是存在的（但通常是实现错误而非其他原因）。
















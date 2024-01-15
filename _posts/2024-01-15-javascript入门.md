---
layout:     post
title:      javascript入门
subtitle:   javascript入门
date:       2024-01-15
author:     soushuuu
header-img: img/the-first.png
catalog: false
tags:
    - 前端
---

JavaScript（JS）是一种具有函数优先特性的轻量级、解释型或者说即时编译型的编程语言。虽然作为 Web 页面中的脚本语言被人所熟知，但是它也被用到了很多非浏览器环境中，例如 Node.js、Apache CouchDB、Adobe Acrobat 等。进一步说，JavaScript 是一种基于原型、多范式、单线程的动态语言，并且支持面向对象、命令式和声明式（如函数式编程）风格。

JavaScript 的动态特性包括运行时对象的构造、变量参数列表、函数变量、动态脚本创建（通过 eval）、对象内枚举（通过 for...in 和 Object 工具方法）和源代码恢复（JavaScript 函数会存储其源代码文本，可以使用 toString() 进行检索）。

[ECMAScript 语言规范（ECMAScript Language Specification）](https://tc39.es/ecma262/)（ECMA-262）和ECMAScript 国际化 API 规范（ECMAScript Internationalization API specification）（ECMA-402）是 Javascript 的标准。当某个 ECMAScript 新特性的提案已经被一些浏览器实现时，MDN 上的文档或示例就可能会涉及到这些新特性。


### 语法和数据类型

JavaScript 是区分大小写的，并使用 Unicode 字符集。

#### 注释

#### 声明

JavaScript 有三种声明方式。

- var 声明一个变量，可选初始化一个值。
- let 声明一个块作用域的局部变量，可选初始化一个值。
- const 声明一个块作用域的只读常量。

三种方式声明变量：

- 使用关键词 var 。例如 var x = 42。这个语法可以用来声明局部变量和全局变量。
- 直接赋值。例如 x = 42。在函数外使用这种形式赋值，会产生一个全局变量。在严格模式下会产生错误。因此你不应该使用这种方式来声明变量。
- 使用关键词 let 。例如 let y = 13。这个语法可以用来声明块作用域的局部变量。

##### 变量的作用域

在函数之外声明的变量，叫做全局变量，因为它可被当前文档中的任何其他代码所访问。在函数内部声明的变量，叫做局部变量，因为它只能在当前函数的内部访问。



##### 变量提升


JavaScript 变量的另一个不同寻常的地方是，你可以先使用变量稍后再声明变量而不会引发异常。这一概念称为 __变量提升__；JavaScript 变量感觉上是被“提升”或移到了函数或语句的最前面。但是，提升后的变量将返回 undefined 值。因此在使用或引用某个变量之后进行声明和初始化操作，这个被提升的变量仍将返回 undefined 值。

在 ECMAScript 6 中，let 和 const 同样会被提升变量到代码块的顶部但是 __不会被赋予初始值__。在变量声明之前引用这个变量，将抛出引用错误（ReferenceError）。这个变量将从代码块一开始的时候就处在一个“暂时性死区”，直到这个变量被声明为止。


##### 函数提升

对于函数来说，只有函数声明会被提升到顶部，而函数表达式不会被提升。



##### 全局变量

实际上，全局变量是全局对象的属性。在网页中，（译注：缺省的）全局对象是 `window` ，所以你可以用形如 window.variable 的语法来设置和访问全局变量。

因此，你可以通过指定 window 或 frame 的名字，在当前 window 或 frame 访问另一个 window 或 frame 中声明的变量。例如，在文档里声明一个叫 phoneNumber 的变量，那么你就可以在子框架里使用 parent.phoneNumber 的方式来引用它。


##### 常量

你可以用关键字 `const` 创建一个只读的常量。常量标识符的命名规则和变量相同：必须以字母、下划线（_）或美元符号（$）开头并可以包含有字母、数字或下划线。

- 对象属性被赋值为常量是不受保护的，所以下面的语句执行时不会产生错误。
    ```js
    const MY_OBJECT = { key: "value" };
    MY_OBJECT.key = "otherValue"; 
    ```
- 数组的被定义为常量也是不受保护的，所以下面的语句执行时也不会产生错误。
    ```js
    const MY_ARRAY = ["HTML", "CSS"];
    MY_ARRAY.push("JAVASCRIPT");
    console.log(MY_ARRAY); //logs ['HTML','CSS','JAVASCRIPT'];
    ```

#### 数据结构和类型

最新的 ECMAScript 标准定义了 8 种数据类型：

- 七种基本数据类型：
    - 布尔值（Boolean），有 2 个值分别是：true 和 false。
    - null，一个表明 null 值的特殊关键字。JavaScript 是大小写敏感的，因此 null 与 Null、NULL或变体完全不同。
    - undefined，和 null 一样是一个特殊的关键字，undefined 表示变量未赋值时的属性。
    - 数字（Number），整数或浮点数，例如： 42 或者 3.14159。
    - 任意精度的整数（BigInt），可以安全地存储和操作大整数，甚至可以超过数字的安全整数限制。
    - 字符串（String），字符串是一串表示文本值的字符序列，例如："Howdy"。
    - 代表（Symbol，在 ECMAScript 6 中新添加的类型）。一种实例是唯一且不可改变的数据类型。
- 对象（Object）。

##### 数据类型的转换

JavaScript 是一种动态类型语言 (dynamically typed language)。这意味着你在声明变量时可以不必指定数据类型，而数据类型会在代码执行时会根据需要自动转换。



##### 数字转换为字符串

在包含的数字和字符串的表达式中使用加法运算符（+），JavaScript 会把数字转换成字符串。



##### 字符串转换为数字

有一些方法可以将内存中表示一个数字的字符串转换为对应的数字。

- parseInt()
- parseFloat()
- 将字符串转换为数字的另一种方法是使用一元加法运算符。
    ```js
    "1.1" + "1.1" = "1.11.1"
    (+"1.1") + (+"1.1") = 2.2
    // 注意：加入括号为清楚起见，不是必需的。
    ```

#### 字面量

#### 数组字面量

数组字面值是一个封闭在方括号对 ([]) 中的包含有零个或多个表达式的列表，其中每个表达式代表数组的一个元素。当你使用数组字面值创建一个数组时，该数组将会以指定的值作为其元素进行初始化，而其长度被设定为元素的个数。

```js
var coffees = ["French Roast", "Colombian", "Kona"];
var a = [3];
console.log(a.length); // 1
console.log(a[0]); // 3
```

你不必列举数组字面值中的所有元素。若你在同一行中连写两个逗号（,），数组中就会产生一个没有被指定的元素，其初始值是 undefined。

```js
var fish = ["Lion", , "Angel"];
```

#### 布尔字面量

布尔类型有两种字面量：true和false。


#### 数字字面量

JavaScript 数字字面量包括多种基数的整数字面量和以 10 为基数的浮点数字面量


##### 整数字面量

整数可以用十进制（基数为 10）、十六进制（基数为 16）、八进制（基数为 8）以及二进制（基数为 2）表示。

- 十进制整数字面量由一串数字序列组成，且没有前缀 0。
- 八进制的整数以 0（或 0O、0o）开头，只能包括数字 0-7。
- 十六进制整数以 0x（或 0X）开头，可以包含数字（0-9）和字母 a~f 或 A~F。
- 二进制整数以 0b（或 0B）开头，只能包含数字 0 和 1。

严格模式下，八进制整数字面量必须以 0o 或 0O 开头，而不能以 0 开头。

```
0, 117 and -345 (十进制，基数为 10)
015, 0001 and -0o77 (八进制，基数为 8)
0x1123, 0x00111 and -0xF1A7 (十六进制，基数为 16 或"hex")
0b11, 0b0011 and -0b11 (二进制，基数为 2)
```


##### 浮点数字面量

浮点数字面值可以有以下的组成部分：

- 一个十进制整数，可以带正负号（即前缀“+”或“-”），
- 小数点（“.”），
- 小数部分（由一串十进制数表示），
- 指数部分。

指数部分以“e”或“E”开头，后面跟着一个整数，可以有正负号（即前缀“+”或“-”）。浮点数字面量至少有一位数字，而且必须带小数点或者“e”（大写“E”也可）。

```js
[(+|-)][digits][.digits][(E|e)[(+|-)]digits]
```

#### 对象字面量

对象字面值是封闭在花括号对（{}）中的一个对象的零个或多个“属性名—值”对的（元素）列表。


以下是一个对象字面值的例子。对象car的第一个元素（译注：即一个属性/值对）定义了属性 myCar；第二个元素，属性 getCar，引用了一个函数调用（即 CarTypes("Honda")）；第三个元素，属性 special，使用了一个已有的变量（即 Sales）。

```js
var Sales = "Toyota";

function CarTypes(name) {
  return name === "Honda" ? name : "Sorry, we don't sell " + name + ".";
}

var car = { myCar: "Saturn", getCar: CarTypes("Honda"), special: Sales };

console.log(car.myCar); // Saturn
console.log(car.getCar); // Honda
console.log(car.special); // Toyota
```

##### 增强的对象字面量


在 ES2015，对象字面值扩展支持在创建时设置原型，简写了 foo: foo 形式的属性赋值，方法定义，支持父方法调用，以及使用表达式动态计算属性名。总之，这些也使对象字面值和类声明更加紧密地联系起来，让基于对象的设计从这些便利中更加受益。

```js
var obj = {
  // __proto__
  __proto__: theProtoObj,
  // Shorthand for ‘handler: handler’
  handler,
  // Methods
  toString() {
    // Super calls
    return "d " + super.toString();
  },
  // Computed (dynamic) property names
  ["prop_" + (() => 42)()]: 42,
};
```


##### RegExp 字面值

```js
var re = /ab+c/;
```

##### 字符串字面量


字符串字面量是由双引号（"）对或单引号（'）括起来的零个或多个字符。字符串被限定在同种引号之间；也即，必须是成对单引号或成对双引号。


```js
'foo'
"bar"
'1234'
'one line \n another line'
"Joyo's cat"
```

你可以在字符串字面值上使用字符串对象的所有方法——JavaScript 会自动将字符串字面值转换为一个临时字符串对象，调用该方法，然后废弃掉那个临时的字符串对象。

```js
console.log("John's cat".length);
// 将打印字符串中的字符个数（包括空格）
// 结果为：10
```

##### 模板字面量

```js
// Basic literal string creation
`In JavaScript '\n' is a line-feed.` // Multiline strings
`In JavaScript this is
 not legal.`;

// String interpolation
var name = "Bob",
  time = "today";
`Hello ${name}, how are you ${time}?`;

// Construct an HTTP request prefix is used to interpret the replacements and construction
POST`http://foo.org/bar?a=${a}&b=${b}
     Content-Type: application/json
     X-Credentials: ${credentials}
     { "foo": ${foo},
       "bar": ${bar}}`(myOnReadyStateChangeHandler);

var poem = `Roses are red,
Violets are blue.
Sugar is sweet,
and so is foo.`;

```


##### 在字符串中使用的特殊字符

作为一般字符的扩展，你可以在字符串中使用特殊字符

```js
"one line \n another line";
```

```
字符	        意思
\0	            Null 字节
\b	            退格符
\f	            换页符
\n	            换行符
\r	            回车符
\t	            Tab (制表符)
\v	            垂直制表符
\'	            单引号
\"	            双引号
\\	            反斜杠字符（\）
\XXX	        由从 0 到 377 最多三位八进制数XXX表示的 Latin-1 字符。例如，\251 是版权符号的八进制序列。
\xXX	        由从 00 和 FF 的两位十六进制数字 XX 表示的 Latin-1 字符。例如，\ xA9 是版权符号的十六进制序列。
\uXXXX	        由四位十六进制数字 XXXX 表示的 Unicode 字符。例如，\ u00A9 是版权符号的 Unicode 序列。见Unicode escape sequences (Unicode 转义字符).
\u*{XXXXX}*	    Unicode 代码点 (code point) 转义字符。例如，\u{2F804} 相当于 Unicode 转义字符 \uD87E\uDC04 的简写。
\               转义字符
```


### 流程控制与错误处理


#### 语句块

最基本的语句是用于组合语句的语句块。该块由一对大括号界定：

```
{
   statement_1;
   statement_2;
   statement_3;
   .
   .
   .
   statement_n;
}
```
在 ECMAScript 6 标准之前，Javascript 没有块作用域。在一个块中引入的变量的作用域是包含函数或脚本，并且设置它们的效果会延续到块之外。换句话说，块语句不定义范围。

从 ECMAScript 2015 开始，使用 let 和const变量是块作用域的。



#### 条件判断语句

- if...else 语句
    - 错误的值 下面这些值将被计算出 false (also known as Falsy values):
        false
        undefined
        null
        0
        NaN
        空字符串（""）
- switch 语句

#### 异常处理语句

你可以用 throw 语句抛出一个异常并且用 try...catch 语句捕获处理它。

- throw语句
- try...catch语句


##### 异常类型


##### throw 语句

使用throw语句抛出一个异常。当你抛出异常，你规定一个含有值的表达式要被抛出。

```js
throw "Error2"; // String type
throw 42; // Number type
throw true; // Boolean type
throw {
  toString: function () {
    return "I'm an object!";
  },
};
```


##### try...catch 语句


try...catch 语句标记一块待尝试的语句，并规定一个以上的响应应该有一个异常被抛出。如果我们抛出一个异常，try...catch语句就捕获它。


```js
openMyFile();
try {
  writeMyFile(theData); //This may throw a error
} catch (e) {
  handleError(e); // If we got a error we handle it
} finally {
  closeMyFile(); // always close the resource
}
```

###### catch 块

你可以使用catch块来处理所有可能在try块中产生的异常。

###### finally块

finally块包含了在 try 和 catch 块完成后、下面接着 try...catch 的语句之前执行的语句。finally块`无论是否抛出异常`都会执行。如果抛出了一个异常，就算没有异常处理，finally块里的语句也会执行。


###### 嵌套 try...catch 语句

你可以嵌套一个或多个try ... catch语句。如果一个内部try ... catch语句没有catch块，它需要有一个finally块，并且封闭的try ... catch语句的catch块被检查匹配。


##### 使用Error对象

根据错误类型，你也许可以用'name'和'message'获取更精炼的信息。'name'提供了常规的错误类（如 'DOMException' 或 'Error'），而'message'通常提供了一条从错误对象转换成字符串的简明信息。



### 循环与迭代

JavaScript 中提供了这些循环语句：

- for 语句
    ```
    for ([initialExpression]; [condition]; [incrementExpression])
        statement
    ```
- do...while 语句
    ```
    do
        statement
    while (condition);
    ```
- while 语句
    ```
    while (condition)
        statement
    ```
- label 语句
    一个 label 提供了一个让你在程序中其他位置引用它的标识符。例如，你可以用 label 标识一个循环，然后使用 break 或者 continue 来指出程序是否该停止循环还是继续循环。
    ```
    label :
        statement
    ```
- break 语句
    使用 break 语句来终止循环，switch，或者是链接到 label 语句。
    - 当你使用不带 label 的 break 时，它会立即终止当前所在的 while，do-while，for，或者 switch 并把控制权交回这些结构后面的语句。
    - 当你使用带 label 的 break 时，它会终止指定的带标记（label）的语句。
    ```
    break [label];
    ```
- continue 语句
    continue 语句可以用来继续执行（跳过代码块的剩余部分并进入下一循环）一个 while、do-while、for，或者 label 语句。
    - 当你使用不带 label 的 continue 时，它终止当前 while，do-while，或者 for 语句到结尾的这次的循环并且继续执行下一次循环。
    - 当你使用带 label 的 continue 时，它会应用被 label 标识的循环语句。
    ```
    continue [label];
    ```
- for...in 语句
    for...in 语句循环一个指定的变量来循环一个对象所有可枚举的属性。JavaScript 会为每一个不同的属性执行指定的语句。
    ```
    for (variable in object) {
        statements
    }
    ```
- for...of 语句
    for...of 语句在可迭代对象（包括Array、Map、Set、arguments 等等）上创建了一个循环，对值的每一个独特属性调用一次迭代。
    ```
    for (variable of object) {
        statement
    }
    ```

下面的这个例子展示了 for...of 和 for...in 两种循环语句之间的区别。 for...in 循环遍历的结果是数组元素的下标，而 for...of 遍历的结果是元素的值：

```js
let arr = [3, 5, 7];
arr.foo = "hello";

for (let i in arr) {
  console.log(i); // 输出 "0", "1", "2", "foo"
}

for (let i of arr) {
  console.log(i); // 输出 "3", "5", "7"
}

// 注意 for...of 的输出没有出现 "hello"
```



### 函数

#### 定义函数

```js
function square(number) {
  return number * number;
}
```

#### 函数表达式

```js
const square = function (number) {
  return number * number;
};

console.log(square(4)); // 16
```

#### 函数提升

函数提升仅适用于函数声明，而不适用于函数表达式。

```js
// 所有函数声明实际上都位于作用域的顶部
function square(n) {
  return n * n;
}

console.log(square(5)); // 25
```



#### 函数作用域

在函数内定义的变量不能在函数之外的任何地方访问，因为变量仅仅在该函数的作用域内定义。相对应的，一个函数可以访问定义在其范围内的任何变量和函数。

换言之，定义在全局域中的函数可以访问所有定义在全局域中的变量。在另一个函数中定义的函数也可以访问在其父函数中定义的所有变量和父函数有权访问的任何其他变量。


```js
// 下面的变量定义在全局作用域中
const num1 = 20;
const num2 = 3;
const name = "Chamakh";

// 此函数定义在全局作用域中
function multiply() {
  return num1 * num2;
}

console.log(multiply()); // 60

// 嵌套函数示例
function getScore() {
  const num1 = 2;
  const num2 = 3;

  function add() {
    return `${name} 的得分为 ${num1 + num2}`;
  }

  return add();
}

console.log(getScore()); // "Chamakh 的得分为 5"
```



#### 作用域和函数栈

##### 递归

一个函数可以指向并调用自身。有三种方法可以达到这个目的：

1. 函数名
2. arguments.callee
3. 作用域内一个指向该函数的变量名

```js
const foo = function bar() {
  // 这里编写语句
};
```
在这个函数体内，以下的语句是等价的：

1. bar()
2. arguments.callee()
3. foo()




#### 嵌套函数和闭包


你可以在一个函数里面嵌套另外一个函数。嵌套（内部）函数对其容器（外部）函数是私有的。

它自身也形成了一个`闭包`（closure）。闭包是可以拥有独立变量以及绑定了这些变量的环境（“封闭”了表达式）的表达式（通常是函数）。

既然嵌套函数是一个闭包，就意味着一个嵌套函数可以“继承”容器函数的参数和变量。换句话说，内部函数包含外部函数的作用域。

可以总结如下：

- 内部函数只可以在外部函数中访问。
- 内部函数形成了一个闭包：它可以访问外部函数的参数和变量，但是外部函数却不能使用它的参数和变量。



##### 保存变量

##### 多层嵌套函数

函数可以被多层嵌套。例如：

- 函数（A）可以包含函数（B），后者可以再包含函数（C）。
- 这里的函数 B 和 C 都形成了闭包，所以 B 可以访问 A，C 可以访问 B。
- 此外，因为 C 可以访问 B（而 B 可以访问 A），所以 C 也可以访问 A。

因此，闭包可以包含多个作用域；它们递归地包含了所有包含它的函数作用域。这个称之为作用域链。

```js
function A(x) {
  function B(y) {
    function C(z) {
      console.log(x + y + z);
    }
    C(3);
  }
  B(2);
}
A(1); // 打印 6（即 1 + 2 + 3）
```

在这个示例中，C 可以访问 B 的 y 和 A 的 x。

这是因为：

1. B 形成了一个包含 A 的闭包（即，B 可以访问 A 的参数和变量）
2. C 形成了一个包含 B 的闭包。
3. C 的闭包包含 B，且 B 的闭包包含 A，所以 C 的闭包也包含 A。这意味着 C 同时可以访问 B 和 A 的参数和变量。换言之，C 用这个顺序链接了 B 和 A 的作用域。

反过来却不是这样。A 不能访问 C，因为 A 不能访问 B 中的参数和变量，C 是 B 中的一个变量，所以 C 是 B 私有的。



##### 命名冲突


当同一个闭包作用域下两个参数或者变量同名时，就会产生命名冲突。更近的作用域有更高的优先权，所以最近的优先级最高，最远的优先级最低。这就是`作用域链`。链的第一个元素就是最里面的作用域，最后一个元素便是最外层的作用域。



#### 闭包

闭包是 JavaScript 中最强大的特性之一。JavaScript 允许函数嵌套，并且内部函数具有定义在外部函数中的所有变量和函数（以及外部函数能访问的所有变量和函数）的完全访问权限。

但是，外部函数却不能访问定义在内部函数中的变量和函数。这给内部函数的变量提供了一种封装。

此外，由于内部函数可以访问外部函数的作用域，因此当内部函数生存周期大于外部函数时，外部函数中定义的变量和函数的生存周期将比内部函数执行的持续时间要长。当内部函数以某一种方式被任何一个外部函数之外的任何作用域访问时，就会创建闭包。

```js
// 外部函数定义了一个名为“name”的变量
const pet = function (name) {
  const getName = function () {
    // 内部函数可以访问外部函数的“name”变量
    return name;
  };
  return getName; // 返回内部函数，从而将其暴露给外部作用域
};
const myPet = pet("Vivie");

console.log(myPet()); // "Vivie"
```

#### 使用 arguments 对象

函数的实际参数会被保存在一个类似数组的 arguments 对象中。在函数内，你可以按如下方式找出传入的参数：

```js
arguments[i];

function myConcat(separator) {
  let result = ""; // 初始化列表
  // 迭代 arguments
  for (let i = 1; i < arguments.length; i++) {
    result += arguments[i] + separator;
  }
  return result;
}
```

#### 函数参数

有两种特殊的参数语法：`默认参数`和`剩余参数`。


##### 默认参数

```js
function multiply(a, b = 1) {
  return a * b;
}

console.log(multiply(5)); // 5
```


##### 剩余参数

剩余参数语法允许将不确定数量的参数表示为数组。

```js
function multiply(multiplier, ...theArgs) {
  return theArgs.map((x) => multiplier * x);
}

const arr = multiply(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```


#### 箭头函数


箭头函数表达式（也称胖箭头，以区分未来 JavaScript 中假设的 -> 语法）相比函数表达式具有较短的语法且没有它自己的 `this`、`arguments`、`super` 和 `new.target`。箭头函数总是匿名的。

有两个因素会影响对箭头函数的引入：更简洁的函数和 this 的无绑定性。


```js
const a = ["Hydrogen", "Helium", "Lithium", "Beryllium"];

const a2 = a.map(function (s) {
  return s.length;
});

console.log(a2); // [8, 6, 7, 9]

const a3 = a.map((s) => s.length);

console.log(a3); // [8, 6, 7, 9]



function Person() {
  this.age = 0;

  setInterval(() => {
    this.age++; // 这里的 `this` 正确地指向 person 对象
  }, 1000);
}

const p = new Person();


```


#### 预定义函数

JavaScript 语言有几个顶级的内置函数：

- eval()
    eval() 方法执行方法计算以字符串表示的 JavaScript 代码。

- isFinite()
    isFinite() 全局函数判断传入的值是否是有限的数值。如果需要的话，其参数首先被转换为一个数值。

- isNaN()
    isNaN() 函数判断一个值是否是 NaN。注意：isNaN 函数内部的强制转换规则十分有趣。你也可以使用 Number.isNaN() 来判断该值是否为 NaN。

- parseFloat()
    parseFloat() 函数解析字符串参数，并返回一个浮点数。

- parseInt()
    parseInt() 函数解析字符串参数，并返回指定的基数（基础数学中的数制）的整数。

- decodeURI()
    decodeURI() 函数对先前经过 encodeURI 函数或者其他类似方法编码过的统一资源标志符（URI）进行解码。

- decodeURIComponent()
    decodeURIComponent() 方法对先前经过 encodeURIComponent 函数或者其他类似方法编码的统一资源标志符（URI）进行解码。

- encodeURI()
    encodeURI() 方法通过以表示字符的 UTF-8 编码的一个、两个、三个或四个转义序列替换统一资源标识符（URI）的某些字符来进行编码（对于由两个“代理（surrogate）”字符组成的字符，只会编码为四个转义序列）。

- encodeURIComponent()
    encodeURIComponent() 方法通过以表示字符的 UTF-8 编码的一个、两个、三个或四个转义序列替换统一资源标识符（URI）的某些字符来进行编码（对于由两个“代理”字符组成的字符，只会编码为四个转义序列）。




### 表达式与运算符


#### 运算符


- 赋值运算符（Assignment operators）
- 比较运算符（Comparison operators）
- 算数运算符（Arithmetic operators）
- 位运算符（Bitwise operators）
- 逻辑运算符（Logical operators）
    - 短路求值
        - false && anything // 被短路求值为 false
        - true || anything // 被短路求值为 true
- 字符串运算符（String operators）
    - 除了比较操作符，它可以在字符串值中使用，连接操作符（+）连接两个字符串值相连接，返回另一个字符串，它是两个操作数串的结合。
- 条件（三元）运算符（Conditional operator）
    - 条件 ? 值 1 : 值 2
- 逗号运算符（Comma operator）
- 一元运算符（Unary operators）
    - delete
        - delete操作符，删除一个对象的属性或者一个数组中某一个键值。
        `delete objectName.property;`
    - typeof
            - typeof 操作符返回一个表示 operand 类型的字符串值。operand 可为字符串、变量、关键词或对象，其类型将被返回。operand 两侧的括号为可选。
            `typeof operand;`
            `typeof (operand);`
    - void
            - void 运算符，表明一个运算没有返回值。expression 是 javaScript 表达式，括号中的表达式是一个可选项，当然使用该方式是一种好的形式。
            `void expression;`
            `void (expression);`
- 关系运算符（Relational operator）
    - in
        - in操作符，如果所指定的属性确实存在于所指定的对象中，则会返回true
        `propNameOrNumber in objectName;`
    - instanceof
        - 如果所判别的对象确实是所指定的类型，则返回true。
        `objectName instanceof objectType;`


#### 表达式


表达式是一组代码的集合，它返回一个值。

- this
    - this关键字被用于指代当前的对象，通常，this指代的是方法中正在被调用的对象。
- 分组操作符
    - 分组操作符（）控制了表达式中计算的优先级。
- 左值表达式
    - new
        - 你可以使用new 运算符创建一个自定义类型或者是预置类型的对象实例。
    - super
        - super 关键字可以用来调用一个对象父类的函数，它在用来调用一个类的父类的构造函数时非常有用



### 数字和日期

#### 数字

在 JavaScript 里面，数字均为双精度浮点类型（double-precision 64-bit binary format IEEE 754），即一个介于 ±2^−1023 和 ±2^+1024 之间的数字，或约为 ±10^−308 到 ±10^+308，数字精度为 53 位。整数数值仅在 ±(2^53 - 1) 的范围内可以表示准确。

除了能够表示浮点数，数字类型也还能表示三种符号值：`+Infinity`（正无穷）、`-Infinity`（负无穷）和 `NaN` (not-a-number，非数字)。

JavaScript 最近添加了 `BigInt` 的支持，能够用于表示极大的数字。使用 BigInt 的时候有一些注意事项，例如，你不能让 `BigInt` 和 `Number` 直接进行运算，你也不能用 Math 对象去操作 BigInt 数字。

你可以使用四种数字进制：十进制、二进制、八进制和十六进制。

#### 数字对象


内置的 Number 对象有一些有关数字的常量属性，如最大值、不是一个数字和无穷大的。


#### 数学对象（Math）

对于内置的Math数学常项和函数也有一些属性和方法。比方说， Math 对象的 PI 属性会有属性值 pi (3.141...)


#### 日期对象


JavaScript 没有日期数据类型。但是你可以在你的程序里使用 `Date` 对象和其方法来处理日期和时间。Date 对象有大量的设置、获取和操作日期的方法。它并不含有任何属性。

`Date` 对象的范围是相对距离 UTC 1970 年 1 月 1 日 的前后 100,000,000 天。

创建一个日期对象：

```js
var dateObjectName = new Date([parameters]);
```
- 无参数 : 创建今天的日期和时间，例如： today = new Date();.
- 一个符合以下格式的表示日期的字符串："月 日，年 时：分：秒"。例如： var Xmas95 = new Date("December 25, 1995 13:30:00")。如果你省略时、分、秒，那么他们的值将被设置为 0。
- 一个年，月，日的整型值的集合，例如： var Xmas95 = new Date(1995, 11, 25)。
- 一个年，月，日，时，分，秒的集合，例如： var Xmas95 = new Date(1995, 11, 25, 9, 30, 0);




### 文本格式化


#### 字符串

##### String 字面量

- 可以使用单引号或双引号创建简单的字符串
- 16 进制转义序列
- Unicode 转义序列
- Unicode 字元逸出


##### 字符串对象

String 对象是对原始 string 类型的封装

##### 多行模板字符串

```js
console.log(`string text line 1
string text line 2`);

const five = 5;
const ten = 10;
console.log(`Fifteen is ${five + ten} and not ${2 * five + ten}.`);
// "Fifteen is 15 and not 20."
```

#### 国际化


`Intl` 对象是 ECMAScript 国际化 API 的命名空间，它提供了语言敏感的字符串比较，数字格式化和日期时间格式化功能。Collator, NumberFormat, 和 DateTimeFormat 对象的构造函数是Intl对象的属性。


##### 日期和时间格式化

`DateTimeFormat` 对象在日期和时间的格式化方面很有用。

##### 数字格式化

`NumberFormat` 对象在数字的格式化方面很有用，比如货币数量值。

##### 定序

`Collator` 对象在字符串比较和排序方面很有用。



### 正则表达式


正则表达式是用于匹配字符串中字符组合的模式。在 JavaScript 中，正则表达式也是对象。这些模式被用于 `RegExp` 的 `exec` 和 `test` 方法，以及 `String` 的 `match`、`matchAll`、`replace`、`search` 和 `split` 方法。


#### 创建一个正则表达式

```js
var re = /ab+c/;

var re = new RegExp("ab+c");
```

#### 使用正则表达式


```
方法	        描述
exec	        一个在字符串中执行查找匹配的 RegExp 方法，它返回一个数组（未匹配到则返回 null）。
test	        一个在字符串中测试是否匹配的 RegExp 方法，它返回 true 或 false。
match	        一个在字符串中执行查找匹配的 String 方法，它返回一个数组，在未匹配到时会返回 null。
matchAll	    一个在字符串中执行查找所有匹配的 String 方法，它返回一个迭代器（iterator）。
search	        一个在字符串中测试匹配的 String 方法，它返回匹配到的位置索引，或者在失败时返回 -1。
replace	        一个在字符串中执行查找匹配的 String 方法，并且使用替换字符串替换掉匹配到的子字符串。
split	        一个使用正则表达式或者一个固定字符串分隔一个字符串，并将分隔后的子字符串存储到数组中的 String 方法。
```

#### 通过标志进行高级搜索


正则表达式有六个可选参数 (flags) 允许全局和不分大小写搜索等。这些参数既可以单独使用也能以任意顺序一起使用，并且被包含在正则表达式实例中。


```
标志	描述
g	    全局搜索。
i	    不区分大小写搜索。
m	    多行搜索。
s	    允许 . 匹配换行符。
u	    使用 unicode 码的模式进行匹配。
y	    执行“粘性 (sticky)”搜索，匹配从目标字符串的当前位置开始。
```

```js
var re = /pattern/flags;

var re = /\w+\s/g;
var str = "fee fi fo fum";
var myArray = str.match(re);
console.log(myArray);

// ["fee ", "fi ", "fo "]
```



### 索引集合类

按索引值排序的数据集合。包括数组和类数组结构，如 Array 对象和 TypedArray 对象。


### 带键的集合

由键索引的数据集合；Map 和 Set 对象包含可按插入顺序迭代的元素。


#### Map、WeakMap

Map 对象就是一个简单的键/值对映射集合，可以按照数据插入时的顺序遍历所有的元素。

WeakMap 是键/值对的集合，其键必须是对象或非注册符号，其值为任意 JavaScript 类型，并且不会对其键创建强引用。也就是说，一个对象作为键出现在 WeakMap 中并不会阻止该对象被垃圾回收。一旦作为键的对象被收集，其在任何 WeakMap 中的相应值也会被垃圾收集，只要它们没有在其他地方被强引用。唯一可用作 WeakMap 键的原始类型类型是 symbol，更具体地说，是非注册 symbol，因为非注册 symbol 保证是唯一的，并且不能被重新创建。

```js
const privates = new WeakMap();

function Public() {
  const me = {
    // 这里是私有数据
  };
  privates.set(this, me);
}

Public.prototype.method = function () {
  const me = privates.get(this);
  // 处理 `me` 中的私有数据
  // …
};

module.exports = Public;
```

#### Set、WeakSet

Set 对象是一组唯一值的集合，可以按照添加顺序来遍历。Set 中的值只能出现一次；它在集合 Set 中是唯一的。

WeakSet 对象是可收集垃圾值的集合，包括对象和非注册 symbol。WeakSet 中的值只能出现一次。它在 WeakSet 的集合中是唯一的。

与 Set 对象的主要区别有：

- 与 Set 相反，WeakSet 是对象或 symbol 的集合，而不是任何类型的任意值的集合。
- WeakSet 的弱是指集合中的对象是弱引用的。如果 WeakSet 中存储的一个对象不存在其他的引用，那么它就会被垃圾回收。这也意味着集合中不再存储当前对象。
- WeakSet 不可枚举。



### 使用对象


JavaScript 的设计是一个简单的基于对象的范式。一个对象就是一系列属性的集合，一个属性包含一个名和一个值。一个属性的值可以是函数，这种情况下属性也被称为方法。除了浏览器里面预定义的那些对象之外，你也可以定义你自己的对象。


#### 枚举一个对象的所有属性


从 ECMAScript 5 开始，有三种原生的方法用于列出或枚举对象的属性：

- for...in 循环 该方法依次访问一个对象及其原型链中所有可枚举的属性。
- Object.keys(o) 该方法返回对象 o 自身包含（不包括原型中）的所有可枚举属性的名称的数组。
- Object.getOwnPropertyNames(o) 该方法返回对象 o 自身包含（不包括原型中）的所有属性 (无论是否可枚举) 的名称的数组。




#### 创建新对象


##### 使用对象初始化器

```js
var obj = {
  property_1: value_1, // property_# 可以是一个标识符...
  2: value_2, // 或一个数字...
  ["property" + 3]: value_3, //  或一个可计算的 key 名...
  // ...,
  "property n": value_n,
}; // 或一个字符串
```


##### 使用构造函数

你可以通过两步来创建对象：

1. 通过创建一个构造函数来定义对象的类型。首字母大写是非常普遍而且很恰当的惯用法。
2. 通过 new 创建对象实例。

```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

var mycar = new Car("Eagle", "Talon TSi", 1993);
```


##### 使用 Object.create 方法

```js
// Animal properties and method encapsulation
var Animal = {
  type: "Invertebrates", // 属性默认值
  displayType: function () {
    // 用于显示 type 属性的方法
    console.log(this.type);
  },
};

// 创建一种新的动物——animal1
var animal1 = Object.create(Animal);
animal1.displayType(); // Output:Invertebrates

// 创建一种新的动物——Fishes
var fish = Object.create(Animal);
fish.type = "Fishes";
fish.displayType(); // Output:Fishes
```


#### 继承


所有的 JavaScript 对象至少继承于一个对象。被继承的对象被称作原型，并且继承的属性可通过构造函数的 prototype 对象找到。



#### 对象属性索引

```js
Car.prototype.color = null;
car1.color = "black";
```


#### 定义方法

```js
objectName.methodname = function_name;

var myObj = {
  myMethod: function(params) {
    // ...do something
  }

  // 或者 这样写也可以

  myOtherMethod(params) {
    // ...do something else
  }
};
```


#### 通过 this 引用对象

JavaScript 有一个特殊的关键字 this，它可以在方法中使用以指代当前对象。


#### 定义 getter 与 setter

一个 getter 是一个获取某个特定属性的值的方法。一个 setter 是一个设定某个属性的值的方法。你可以为预定义的或用户定义的对象定义 getter 和 setter 以支持新增的属性。定义 getter 和 setter 的语法采用对象字面量语法。

```js
var o = {
  a: 7,
  get b() {
    return this.a + 1;
  },
  set c(x) {
    this.a = x / 2;
  },
};

console.log(o.a); // 7
console.log(o.b); // 8
o.c = 50;
console.log(o.a); // 25
```


#### 删除属性

你可以用 delete 操作符删除一个不是继承而来的属性。

```js
//Creates a new object, myobj, with two properties, a and b.
var myobj = new Object();
myobj.a = 5;
myobj.b = 12;

//Removes the a property, leaving myobj with only the b property.
delete myobj.a;
```


#### 比较对象


在 JavaScript 中 objects 是一种引用类型。两个独立声明的对象永远也不会相等，即使他们有相同的属性，只有在比较一个对象和这个对象的引用时，才会返回 true.

```js
// 两个变量，两个具有同样的属性、但不相同的对象
var fruit = { name: "apple" };
var fruitbear = { name: "apple" };

fruit == fruitbear; // return false
fruit === fruitbear; // return false
```



### 使用类

JavaScript 是一个`基于原型的语言`——__一个对象的行为取决于它自身的属性及其原型的属性__。对类来说，相较于与其他面向对象的语言，譬如 Java，创建对象的多层级结构及其属性的继承关系需要更多的代码行。


在许多其他语言中，类（或构造函数）与对象（或实例），是两个不同的概念。在 JavaScript 中，类可以看作是已有的原型继承机制的一种抽象——所有语法都可以转换为原型继承。类本身也是不过是 JavaScript 里一种普通的值，它们有其自己的原型链。事实上，大多数 JavaScript 纯函数都可用作构造函数——你可以用 new 运算符来调用一个构造函数以创建出一个新的对象。


#### 类的概述

类的基本概念：

- 类通过 new 运算符创建对象。
- 每个对象都有一些属性（数据或方法），这些属性是由类添加的。
- 类本身也有一些属性（数据或方法），这些属性通常用于与实例进行交互。

这些对应于类的三个关键特征：

- 构造函数；
- 实例方法和实例字段；
- 静态方法和静态字段。


#### 声明一个类


```js
class MyClass {
  // 构造函数
  constructor() {
    // 构造函数体
  }
  // 实例字段
  myField = "foo";
  // 实例方法
  myMethod() {
    // myMethod 体
  }
  // 静态字段
  static myStaticField = "bar";
  // 静态方法
  static myStaticMethod() {
    // myStaticMethod 体
  }
  // 静态块
  static {
    // 静态初始化代码
  }
  // 字段、方法、静态字段、静态方法、静态块都可以使用私有形式
  #myPrivateField = "bar";
}

// 旧版本
function MyClass() {
  this.myField = "foo";
  // 构造函数体
}
MyClass.myStaticField = "bar";
MyClass.myStaticMethod = function () {
  // myStaticMethod 体
};
MyClass.prototype.myMethod = function () {
  // myMethod 体
};

(function () {
  // 静态初始化代码
})();
```


##### 构造一个类

```js
const myInstance = new MyClass();
console.log(myInstance.myField); // 'foo'
myInstance.myMethod();
```

##### 类声明提升

与函数声明不同，__类声明并`不会`被提升__（或者，在某些解释器中，可以被提升，但是有暂时性死区的限制），这意味着你不能在声明之前使用类。

```js
new MyClass(); // ReferenceError: Cannot access 'MyClass' before initialization

class MyClass {}
```    

##### 类表达式

```js
const MyClass = class {
  // 类体...
};


const MyClass = class MyClassLongerName {
  // 类体。这里 MyClass 和 MyClassLongerName 指向同一个类
};
new MyClassLongerName(); // ReferenceError: MyClassLongerName is not defined
```


#### 构造函数

类最重要的工作之一就是作为对象的“工厂”。例如，当我们使用 Date 构造函数时，我们期望它给我们一个新的对象，这个对象代表了我们传入的日期数据，而且我们可以使用该实例所暴露的其他方法来操作它。在类中，实例的创建是通过构造函数来完成的。

```js
class Color {
  constructor(r, g, b) {
    // 将 RGB 值作为 `this` 的属性
    this.values = [r, g, b];
  }
}
```

#### 实例方法

如果一个类只有构造函数，那么它与一个只创建普通对象的 createX 工厂函数并没有太大的区别。然而，类的强大之处在于它们可以作为“模板”，自动将方法分配给实例。

```js
class Color {
  constructor(r, g, b) {
    this.values = [r, g, b];
  }
  getRed() {
    return this.values[0];
  }
}

const red = new Color(255, 0, 0);
console.log(red.getRed()); // 255
```


#### 私有字段

私有字段是以 #（井号）开头的标识符。井号是这个字段名的必要部分，这也就意味着私有字段永远不会与公共属性发生命名冲突。为了在类中的任何地方引用一个私有字段，你必须在类体中声明它（你不能在类体外部创建私有字段）。除此之外，私有字段与普通属性几乎是等价的。


```js
class Color {
  // 声明：每个 Color 实例都有一个名为 #values 的私有字段。
  #values;
  constructor(r, g, b) {
    this.#values = [r, g, b];
  }
  getRed() {
    return this.#values[0];
  }
  setRed(value) {
    this.#values[0] = value;
  }
}

const red = new Color(255, 0, 0);
console.log(red.getRed()); // 255
```


#### getter 字段

```js
class Color {
  constructor(r, g, b) {
    this.values = [r, g, b];
  }
  get red() {
    return this.values[0];
  }
  set red(value) {
    this.values[0] = value;
  }
}

const red = new Color(255, 0, 0);
red.red = 0;
console.log(red.red); // 0
```

#### 公共字段

```js
class MyClass {
  luckyNumber = Math.random();
}
console.log(new MyClass().luckyNumber); // 0.5
console.log(new MyClass().luckyNumber); // 0.3
```

#### 静态属性

```js
class Color {
  static isValid(r, g, b) {
    return r >= 0 && r <= 255 && g >= 0 && g <= 255 && b >= 0 && b <= 255;
  }
}

Color.isValid(255, 0, 0); // true
Color.isValid(1000, 0, 0); // false
```


#### 扩展与继承

```js
class ColorWithAlpha extends Color {
  #alpha;
  // …
  toString() {
    // 调用父类的 toString()，并以此构建新的返回值
    return `${super.toString()}, ${this.#alpha}`;
  }
}

console.log(new ColorWithAlpha(255, 0, 0, 0.5).toString()); // '255, 0, 0, 0.5'


class ColorWithAlpha extends Color {
  // ...
  static isValid(r, g, b, a) {
    // 调用父类的 isValid()，并在此基础上增强返回值
    return super.isValid(r, g, b) && a >= 0 && a <= 1;
  }
}

console.log(ColorWithAlpha.isValid(255, 0, 0, -1)); // false
```



### 使用 Promise

Promise 是一个对象，它代表了一个异步操作的最终完成或者失败。

本质上 Promise 是一个`函数返回的对象`，我们可以`在它上面绑定回调函数`，这样我们就不需要在一开始把回调函数作为参数传入这个函数了。


假设现在有一个名为 createAudioFileAsync() 的函数，它接收一些配置和两个回调函数，然后异步地生成音频文件。一个回调函数在文件成功创建时被调用，另一个则在出现异常时被调用。

以下为使用 createAudioFileAsync() 的示例：

```js
// 成功的回调函数
function successCallback(result) {
  console.log("音频文件创建成功：" + result);
}

// 失败的回调函数
function failureCallback(error) {
  console.log("音频文件创建失败：" + error);
}

createAudioFileAsync(audioSettings, successCallback, failureCallback);
```

如果重写 createAudioFileAsync() 为返回 Promise 的形式，你可以把回调函数附加到它上面：

```js
createAudioFileAsync(audioSettings).then(successCallback, failureCallback);
```

#### 链式调用


连续执行两个或者多个异步操作是一个常见的需求，在上一个操作执行成功之后，开始下一个的操作，并带着上一步操作所返回的结果。在旧的回调风格中，这种操作会导致经典的`回调地狱`：

```js
doSomething(function (result) {
  doSomethingElse(result, function (newResult) {
    doThirdThing(newResult, function (finalResult) {
      console.log(`得到最终结果：${finalResult}`);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);
```

then() 函数会返回一个和原来不同的 __新的 Promise__：

```js
const promise = doSomething();
const promise2 = promise.then(successCallback, failureCallback);
```

`promise2` 不仅表示 `doSomething()` 函数的完成，也代表了你传入的 `successCallback` 或者 `failureCallback` 的完成，这两个函数也可以返回一个 Promise 对象，从而形成另一个异步操作，这样的话，在 `promise2` 上新增的回调函数会排在这个 Promise 对象的后面。


就像这样，每一个 Promise 都代表了链中另一个异步过程的完成。此外，`then` 的参数是可选的，`catch(failureCallback)` 等同于 `then(null, failureCallback)`——所以如果你的错误处理代码对所有步骤都是一样的，你可以把它附加到链的末尾：

```js
doSomething()
  .then(function (result) {
    return doSomethingElse(result);
  })
  .then(function (newResult) {
    return doThirdThing(newResult);
  })
  .then(function (finalResult) {
    console.log(`得到最终结果：${finalResult}`);
  })
  .catch(failureCallback);


doSomething()
  .then((result) => doSomethingElse(result))
  .then((newResult) => doThirdThing(newResult))
  .then((finalResult) => {
    console.log(`得到最终结果：${finalResult}`);
  })
  .catch(failureCallback);

```

注意：一定要有返回值，否则，回调将无法获取上一个 Promise 的结果。（如果使用箭头函数，() => x 比 () => { return x; } 更简洁一些，但后一种保留 return 的写法才支持使用多个语句）。如果上一个处理程序启动了一个 Promise 但并没有返回它，那就没有办法再追踪它的状态了，这个 Promise 就是“漂浮”的。

```js
doSomething()
  .then((url) => {
    // 忘记返回了！
    fetch(url);
  })
  .then((result) => {
    // 结果是 undefined，因为上一个处理程序没有返回任何东西。
    // 无法得知 fetch() 的返回值，不知道它是否成功。
  });
```

如果有竞争条件的话，情况会更糟——如果上一个处理程序的 Promise 没有返回，那么下一个 then 处理程序会提前调用，而它读取的任何值都可能是不完整的。

```js
const listOfIngredients = [];

doSomething()
  .then((url) => {
    // 忘记返回了！
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        listOfIngredients.push(data);
      });
  })
  .then(() => {
    console.log(listOfIngredients);
    // 永远是 []，因为 fetch 请求还没有完成。
  });
```

因此，一个经验法则是，每当你的操作遇到一个 Promise，就返回它，并把它的处理推迟到下一个 then 处理程序中。

```js
const listOfIngredients = [];

doSomething()
  .then((url) =>
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        listOfIngredients.push(data);
      }),
  )
  .then(() => {
    console.log(listOfIngredients);
  });

// 或

doSomething()
  .then((url) => fetch(url))
  .then((res) => res.json())
  .then((data) => {
    listOfIngredients.push(data);
  })
  .then(() => {
    console.log(listOfIngredients);
  });
```



#### 嵌套

对比上述两个例子，第一个例子中有一个 Promise 链嵌套在另一个 then() 处理程序的返回值中；而第二个例子则是完全扁平化的链。简洁的 Promise 链式编程最好保持扁平化，_不要_ 嵌套 Promise，因为嵌套经常会是粗心导致的。


嵌套 Promise 是一种可以限制 catch 语句的作用域的控制结构写法。明确来说，嵌套的 catch 只会捕获其作用域及以下的错误，而不会捕获链中更高层的错误。如果使用正确，可以实现高精度的错误恢复。

```js
doSomethingCritical()
  .then((result) =>
    doSomethingOptional() // 可选操作
      .then((optionalResult) => doSomethingExtraNice(optionalResult))
      .catch((e) => {
        console.log(e.message);
      }),
  ) // 即便可选操作失败了，也会继续执行
  .then(() => moreCriticalStuff())
  .catch((e) => console.log(`严重失败：${e.message}`));
```
这个内部的 catch 语句仅能捕获到 doSomethingOptional() 和 doSomethingExtraNice() 的失败，并将该错误与外界屏蔽，之后就恢复到 moreCriticalStuff() 继续执行。值得注意的是，如果 doSomethingCritical() 失败，这个错误仅会被最后的（外部）catch 语句捕获到，并不会被内部 catch 吞掉。


##### Catch 的后续链式操作

有可能会在一个回调失败之后继续使用链式操作，即，使用一个 catch，这对于在链式操作中抛出一个失败之后，再次进行新的操作会很有用。请阅读下面的例子：

```js
new Promise((resolve, reject) => {
  console.log("初始化");

  resolve();
})
  .then(() => {
    throw new Error("有哪里不对了");

    console.log("执行「这个」");
  })
  .catch(() => {
    console.log("执行「那个」");
  })
  .then(() => {
    console.log("执行「这个」，无论前面发生了什么");
  });


/*
输出结果如下：

初始化
执行「那个」
执行「这个」，无论前面发生了什么

*/
```

##### 常见错误

```js
// 错误示例，包含 3 个问题！

doSomething()
  .then(function (result) {
    // 忘记返回 Promise + 不必要的嵌套
    doSomethingElse(result).then((newResult) => doThirdThing(newResult));
  })
  .then(() => doFourthThing());
// 忘记使用 catch 终止 Promise 链
```

- 第一个错误是没有正确地链接。当我们创建一个新 Promise 但忘记返回它时，就会发生这种情况。结果就是，这条链断掉了——或者更确切地说，我们有两个独立的链在竞争。这意味着 doFourthThing() 不会等待 doSomethingElse() 或 doThirdThing() 完成，而是会与它们同时运行——这很可能不是我们想要的。单独的链也有单独的错误处理，这会导致错误无法被捕获。

- 第二个错误是不必要的嵌套。嵌套限制了内部错误处理程序的作用域，如果不是有意为之，可能会导致未捕获的错误。该错误的一个变体是 Promise 构造函数反模式，它将一个 Promise 代码片段嵌入了另一个 Promise 构造函数里。

- 第三个错误是忘记用 catch 终止链。在大多数浏览器中，未终止的 Promise 链会导致 Promise 的拒绝事件无法被捕获。

一个好的经验法则是总是返回或终止 Promise 链，并且一旦你得到一个新的 Promise，就立即返回它，最终的链应是扁平化的：

```js
doSomething()
  .then(function (result) {
    // 如果使用完整的函数表达式：返回 Promise
    return doSomethingElse(result);
  })
  // 如果使用箭头函数：省略大括号并隐式返回结果
  .then((newResult) => doThirdThing(newResult))
  // 即便上一个 Promise 返回了一个结果，后一个 Promise 也不一定非要使用它。
  // 你可以传入一个不使用前一个结果的处理程序。
  .then((/* 忽略上一个结果 */) => doFourthThing())
  // 总是使用 catch 终止 Promise 链，以保证任何未处理的拒绝事件都能被捕获！
  .catch((error) => console.error(error));
```

使用 async/await 可以解决以上大多数错误，使用 async/await 时，最常见的语法错误就是忘记了 await 关键字。


#### 错误处理

通常，一遇到异常抛出，浏览器就会顺着 Promise 链寻找下一个 onRejected 失败回调函数或者由 .catch() 指定的回调函数。这和以下同步代码的工作原理（执行过程）非常相似。

```js
try {
  let result = syncDoSomething();
  let newResult = syncDoSomethingElse(result);
  let finalResult = syncDoThirdThing(newResult);
  console.log(`得到最终结果：${finalResult}`);
} catch (error) {
  failureCallback(error);
}
```

这种异步代码的对称性在 async/await 语法中达到了极致：

```js
async function foo() {
  try {
    const result = await doSomething();
    const newResult = await doSomethingElse(result);
    const finalResult = await doThirdThing(newResult);
    console.log(`得到最终结果：${finalResult}`);
  } catch (error) {
    failureCallback(error);
  }
}
```



##### Promise 拒绝事件

当一个 Promise 拒绝事件未被任何处理器处理时，它会冒泡到调用栈的顶部，主机需要将其暴露出来。在 Web 上，当 Promise 被拒绝时，会有下文所述的两个事件之一被派发到全局作用域（通常而言，就是 [window](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)；如果是在 web worker 中使用的话，就是 Worker 或者其他基于 [worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Worker) 的接口）。这两个事件如下所示：

[rejectionhandled](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/rejectionhandled_event)
- 当 Promise 被拒绝、并且在 reject 函数处理该拒绝事件之后会派发此事件。

[unhandledrejection](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/unhandledrejection_event)
- 当 Promise 被拒绝，但没有提供 reject 函数来处理该拒绝事件时，会派发此事件。


在 [Node.js](https://developer.mozilla.org/zh-CN/docs/Glossary/Node.js) 中，对拒绝事件的处理稍有不同。你可以通过为 Node.js 的 `unhandledRejection` 事件添加处理器（注意名称的大小写不同）来捕获未处理的拒绝，就像这样：

```js
process.on("unhandledRejection", (reason, promise) => {
  /* 你可以在这里添加一些代码，以便检查 promise 和 reason */
});
```


#### 组合


有四个[组合工具](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise#promise_%E5%B9%B6%E5%8F%91)可用来并发异步操作：Promise.all()、Promise.allSettled()、Promise.any() 和 Promise.race()。



#### 在旧式回调 API 中创建 Promise

理想状态下，所有的异步函数应该会返回 Promise。但有一些 API 仍然使用旧方式来传入成功（或者失败）的回调。最典型的例子就是 setTimeout() (en-US) 函数：

```js
setTimeout(() => saySomething("10 秒钟过去了"), 10 * 1000);
```

我们可以将 setTimeout 封装入 Promise 内。最好的做法是，将这些有问题的函数封装起来，留在底层，并且永远不要再直接调用它们：

```js
const wait = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

wait(10 * 1000)
  .then(() => saySomething("10 秒钟"))
  .catch(failureCallback);
```


#### 时序

关于注册的回调函数何时被调用。

##### 保证

Promise 是一种 __控制反转的形式——API 的实现者不控制回调何时被调用__。相反，维护回调队列并决定何时调用回调的工作被委托给了 Promise 的实现者，这样一来，API 的使用者和开发者都会自动获得强大的语义保证，包括：

- 被添加到 then() 的回调永远不会在 JavaScript 事件循环的当前运行完成之前被调用。
- 即使异步操作已经完成（成功或失败），在这之后通过 then() 添加的回调函数也会被调用。
- 通过多次调用 then() 可以添加多个回调函数，它们会按照插入顺序进行执行。

##### 任务队列 vs. 微任务

[微任务与 Javascript 运行时环境](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth)



##### 当 Promise 与 任务冲突时

你可能遇到如下情况：你的一些 Promise 和任务（例如事件或回调）会以不可预测的顺序启动。此时，你或许可以通过使用微任务检查状态或平衡 Promise，并以此有条件地创建 Promise。

[微任务指南](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide)





### JavaScript 类型化数组


JavaScript 类型化数组是一种类似数组的对象，并提供了一种用于在内存缓冲中访问原始二进制数据的机制。

引入类型化数组并非是为了取代 JavaScript 中数组的任何一种功能。相反，它为开发者提供了一个 __操作二进制数据__ 的接口。

#### 缓冲

有两种类型的缓冲：[ArrayBuffer](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 和 [SharedArrayBuffer](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer)。


#### 视图

目前主要有两种视图：类型化数组视图和 [DataView](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/DataView)。类型化数组提供了实用方法，可以方便地转换二进制数据。DataView 更底层，可以精确控制数据的访问方式。


#### 使用类型化数组的 Web API

下面是一些使用类型化数组的 API 示例，其并不完整

[FileReader.prototype.readAsArrayBuffer()](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader/readAsArrayBuffer)
FileReader.prototype.readAsArrayBuffer() 读取对应的 Blob 或 File 的内容

[XMLHttpRequest.prototype.send()](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/send)
XMLHttpRequest 实例的 send() 方法现在支持使用类型化数组和 ArrayBuffer 对象作为参数。

[ImageData.data](https://developer.mozilla.org/zh-CN/docs/Web/API/ImageData)
是一个 [Uint8ClampedArray](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray) 对象，用来描述包含按照 RGBA 序列的颜色数据的一维数组，其值的范围在 0 到 255 之间。




### 迭代器和生成器

迭代器和生成器将迭代的概念直接带入核心语言，并提供了一种机制来自定义 for...of 循环的行为。

- [迭代协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)
- [for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)
- [function*](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*) 和 [Generator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator)
- [yield](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield) 和 [yield*](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield*)



#### 迭代器

```js
function makeRangeIterator(start = 0, end = Infinity, step = 1) {
  let nextIndex = start;
  let iterationCount = 0;

  const rangeIterator = {
    next() {
      let result;
      if (nextIndex < end) {
        result = { value: nextIndex, done: false };
        nextIndex += step;
        iterationCount++;
        return result;
      }
      return { value: iterationCount, done: true };
    },
  };
  return rangeIterator;
}



let it = makeRangeIterator(1, 10, 2);

let result = it.next();
while (!result.done) {
  console.log(result.value); // 1 3 5 7 9
  result = it.next();
}

console.log(`已迭代序列的大小：${result.value}`); // 5

```


#### 生成器函数


虽然自定义迭代器是一个有用的工具，但由于需要显式地维护其内部状态，因此创建时要格外谨慎。生成器函数（Generator 函数）提供了一个强大的替代选择：它允许你定义一个非连续执行的函数作为迭代算法。生成器函数使用 function* 语法编写。

最初调用时，生成器函数不执行任何代码，而是返回一种称为生成器的特殊迭代器。通过调用 next() 方法消耗该生成器时，生成器函数将执行，直至遇到 yield 关键字。

可以根据需要多次调用该函数，并且每次都返回一个新的生成器，但每个生成器只能迭代一次。


```js
function* makeRangeIterator(start = 0, end = Infinity, step = 1) {
  let iterationCount = 0;
  for (let i = start; i < end; i += step) {
    iterationCount++;
    yield i;
  }
  return iterationCount;
}
```


#### 可迭代对象

为了实现可迭代，一个对象必须实现 __@@iterator__ 方法，这意味着这个对象（或其[原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)中的任意一个对象）必须具有一个键值为 Symbol.iterator 的属性。

```js
function* makeIterator() {
  yield 1;
  yield 2;
}

const it = makeIterator();

for (const itItem of it) {
  console.log(itItem);
}

console.log(it[Symbol.iterator]() === it); // true

// 这个例子向我们展示了生成器（迭代器）是可迭代对象，
// 它有一个 @@iterator 方法返回 it（它自己），
// 因此，it 对象只能迭代*一次*。

// 如果我们将它的 @@iterator 方法改为一个返回新的迭代器/生成器对象的函数/生成器，
// 它（it）就可以迭代多次了。

it[Symbol.iterator] = function* () {
  yield 2;
  yield 1;
};
```


##### 自定义的可迭代对象

```js
var myIterable = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  },
};



for (let value of myIterable) {
  console.log(value);
}
// 1
// 2
// 3

[...myIterable]; // [1, 2, 3]
```


##### 内置可迭代对象

String、Array、TypedArray、Map 和 Set 都是内置可迭代对象，因为它们的原型对象都拥有一个 Symbol.iterator 方法。


##### 用于可迭代对象的语法

一些语句和表达式专用于可迭代对象，例如 for...of 循环、展开语法、yield* 和解构语法。

```js
for (let value of ["a", "b", "c"]) {
  console.log(value);
}
// "a"
// "b"
// "c"

[..."abc"]; // ["a", "b", "c"]

function* gen() {
  yield* ["a", "b", "c"];
}

gen().next(); // { value: "a", done: false }

// 解构语法
[a, b, c] = new Set(["a", "b", "c"]);
a; // "a"
```


#### 高级生成器

生成器会按需计算它们 yield 的值，这使得它们能够高效地表示一个计算成本很高的序列，甚至是一个无限序列。

next() 方法也接受一个参数用于修改生成器内部状态。传递给 next() 的参数值会被 yield 接收。


下面的是斐波那契数列生成器，它使用了 next(x) 来重启序列：

```js
function* fibonacci() {
  let current = 0;
  let next = 1;
  while (true) {
    const reset = yield current;
    [current, next] = [next, next + current];
    if (reset) {
      current = 0;
      next = 1;
    }
  }
}

const sequence = fibonacci();
console.log(sequence.next().value); // 0
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 2
console.log(sequence.next().value); // 3
console.log(sequence.next().value); // 5
console.log(sequence.next().value); // 8
console.log(sequence.next(true).value); // 0
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 2
```

你可以通过调用其 throw() 方法强制生成器抛出异常，并传递应该抛出的异常值。这个异常将从当前挂起的生成器的上下文中抛出，就好像当前挂起的 yield 是一个 throw value 语句。

如果该异常没有在生成器内部被捕获，则它将通过 throw() 的调用向上传播，对 next() 的后续调用将导致 done 属性为 true。

生成器的 [return()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator/return) 方法可返回给定的值并终结这个生成器。



### 元编程

[Proxy](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy) 和 [Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect) 对象允许你拦截并自定义基本语言操作（例如属性查找、赋值、枚举和函数调用等）。借助这两个对象，你可以在 JavaScript 进行元级别的编程。


#### 代理

Proxy 对象可以拦截某些操作并实现自定义行为。

例如获取一个对象上的属性：

```js
let handler = {
  get(target, name) {
    return name in target ? target[name] : 42;
  },
};

let p = new Proxy({}, handler);
p.a = 1;

console.log(p.a, p.b); // 1, 42
```

##### 术语

- [handler](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy)
    包含陷阱的占位符对象（下译作“处理器”）。
- 陷阱
    提供属性访问的方法（这类似于操作系统中陷阱的概念）。
- target
    代理所虚拟化的对象（下译作“目标”）。它通常用作代理的存储后端。JavaScript 会验证与不可扩展性或不可配置属性相关的不变式。
- 不变式
    实现自定义操作时保持不变的语义称为不变式。如果你破坏了处理器的不变式，则会引发 TypeError 异常。



#### 处理器和陷阱

[Proxy 对象可用的陷阱](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Meta_programming)

#### 可撤销的 Proxy

可以用 Proxy.revocable() 方法来创建可撤销的 Proxy 对象。这意味着可以通过 revoke 函数来撤销并关闭一个代理。

此后，对代理进行的任意的操作都会导致 TypeError。

```js
const revocable = Proxy.revocable(
  {},
  {
    get(target, name) {
      return `[[${name}]]`;
    },
  },
);
const proxy = revocable.proxy;
console.log(proxy.foo); // "[[foo]]"

revocable.revoke();

console.log(proxy.foo); // TypeError: Cannot perform 'get' on a proxy that has been revoked
proxy.foo = 1; // TypeError: Cannot perform 'set' on a proxy that has been revoked
delete proxy.foo; // TypeError: Cannot perform 'deleteProperty' on a proxy that has been revoked
console.log(typeof proxy); // "object", `typeof` 不会触发任何陷阱
```


#### 反射


[Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect) 是一个内置对象，它为可拦截的 JavaScript 操作提供了方法。这些方法与[代理处理器所提供的方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy)类似。

Reflect 并不是一个函数对象。

Reflect 将默认操作从处理器转发到 target。


#### 更好的 apply 函数


在不借助 Reflect 的情况下，我们通常使用 [Function.prototype.apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法调用一个具有给定 this 值和 arguments 数组（或类数组对象）的函数。

```js
Function.prototype.apply.call(Math.floor, undefined, [1.75]);
```

借助 [Reflect.apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/apply)，这些操作将变得更加简洁：

```js
Reflect.apply(Math.floor, undefined, [1.75]);
// 1;

Reflect.apply(String.fromCharCode, undefined, [104, 101, 108, 108, 111]);
// "hello"

Reflect.apply(RegExp.prototype.exec, /ab/, ["confabulation"]).index;
// 4

Reflect.apply("".charAt, "ponies", [3]);
// "i"
```

##### 检查属性定义是否成功

使用 [Object.defineProperty](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)，如果成功则返回一个对象，否则抛出一个 [TypeError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError)，你可使用 `try...catch` 块来捕获定义属性时发生的任何错误。因为 [Reflect.defineProperty](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/defineProperty) 返回一个布尔值表示的成功状态，你可以在这里使用 `if...else` 块：


```js
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}

```



### JavaScript 模块


#### 模块化的背景

JavaScript 程序本来很小——在早期，它们大多被用来执行独立的脚本任务，在你的 web 页面需要的地方提供一定交互，所以一般不需要多大的脚本。过了几年，我们现在有了运行大量 JavaScript 脚本的复杂程序，还有一些被用在其他环境（例如 Node.js）。

因此，近年来，有必要开始考虑提供一种 __将 JavaScript 程序拆分为可按需导入的单独模块__ 的机制。Node.js 已经提供这个能力很长时间了，还有很多的 JavaScript 库和框架已经开始了模块的使用（例如，CommonJS 和基于 AMD 的其他模块系统 如 RequireJS，以及最新的 Webpack 和 Babel）。



#### 浏览器兼容性

- javascript.statements.import
- javascript.statements.export


#### .mjs 与 .js

我们使用 .js 扩展名的模块文件，但在其他一些文章中，可能会看到 .mjs 扩展名的使用。V8 推荐了.mjs这样的做法，比如有下列理由：

- 比较清晰，这可以指出哪些文件是模块，哪些是常规的 JavaScript。
- 这能保证你的模块可以被运行时环境和构建工具识别，比如 Node.js 和 Babel。

为了使模块可以在浏览器中正常地工作，你需要确保你的服务器能够正常地处理 Content-Type 头，其应该包含 JavaScript 的 MIME 类型 text/javascript。如果没有这么做，你可能会得到 一个严格 MIME 类型检查错误：“The server responded with a non-JavaScript MIME type（服务器返回了非 JavaScript MIME 类型）”，并且浏览器会拒绝执行相应的 JavaScript 代码。多数服务器可以正确地处理 .js 文件的类型，但是 .mjs 还不行。已经可以正常响应 .mjs 的服务器有 GitHub 页面 和 Node.js 的 http-server。

- 一些工具不支持 .mjs，比如 TypeScript。
- `<script type="module">` 属性用于指示引入的模块



#### 导出模块的功能


为了获得模块的功能要做的第一件事是把它们导出来。使用 export 语句来完成。

```js
export const name = "square";

export function draw(ctx, length, x, y, color) {
  ctx.fillStyle = color;
  ctx.fillRect(x, y, length, length);

  return {
    length: length,
    x: x,
    y: y,
    color: color,
  };
}
```

能够导出函数，var，let，const, 和类。


#### 导入功能到你的脚本

你想在模块外面使用一些功能，那你就需要导入他们才能使用。最简单的就像下面这样的：

```js
import { name, draw, reportArea, reportPerimeter } from '/js-examples/modules/basic-modules/modules/square.js';
```

#### 应用模块到你的 HTML


首先，你需要把 type="module" 放到 `<script>` 标签中，来声明这个脚本是一个模块：

```js
<script type="module" src="main.js"></script>
```

你导入模块功能的脚本基本是作为顶级模块。如果省略它，Firefox 就会给出错误“SyntaxError: import declarations may only appear at top level of a module。

你只能在模块内部使用 import 和export 语句；不是普通脚本文件。


#### 其他模块与标准脚本的不同

- 你需要注意本地测试——如果你通过本地加载 HTML 文件（比如一个 file:// 路径的文件），你将会遇到 CORS 错误，因为 JavaScript 模块安全性需要。你需要通过一个服务器来测试。
- 另请注意，你可能会从模块内部定义的脚本部分获得与标准脚本中不同的行为。这是因为模块自动使用严格模式。
- 加载一个模块脚本时不需要使用 defer 属性 (see `<script>` [attributes](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script#attributes)) 模块会自动延迟加载。
- 最后一个但不是不重要，你需要明白模块功能导入到单独的脚本文件的范围——他们无法在全局获得。因此，你只能在导入这些功能的脚本文件中使用他们，你也无法通过 JavaScript console 中获取到他们，比如，在 DevTools 中你仍然能够获取到语法错误，但是你可能无法像你想的那样使用一些 debug 技术。


#### 默认导出 versus 命名导出

到目前为止我们导出的功能都是由 `named exports` 组成 —- 每个项目（无论是函数，常量等）在导出时都由其名称引用，并且该名称也用于在导入时引用它。

还有一种导出类型叫做 `default export` —- 这样可以很容易地使模块提供默认功能，并且还可以帮助 JavaScript 模块与现有的 CommonJS 和 AMD 模块系统进行互操作。

```js
export default function(ctx) {
  ...
}
```

#### 避免命名冲突


#### 重命名导出与导入

```js
// inside module.js
export { function1, function2 };

// inside main.js
import {
  function1 as newFunctionName,
  function2 as anotherNewFunctionName,
} from "/modules/module.js";

// in square.js
export {
  name as squareName,
  draw as drawSquare,
  reportArea as reportSquareArea,
  reportPerimeter as reportSquarePerimeter,
};
```

#### 创建模块对象

导入每一个模块功能到一个模块功能对象上。可以使用以下语法形式：

```js
import * as Module from "/modules/module.js";
```


#### 模块与类（class）

```js
class Square {
  constructor(ctx, listId, length, x, y, color) {
    // …
  }

  draw() {
    // …
  }

  // …
}

export { Square };

import { Square } from "./modules/square.js";

let square1 = new Square(myCanvas.ctx, myCanvas.listId, 50, 50, 100, "blue");
square1.draw();
square1.reportArea();
square1.reportPerimeter();

```


#### 合并模块

有时你会想要将模块聚合在一起。你可能有多个级别的依赖项，你希望简化事物，将多个子模块组合到一个父模块中。这可以使用父模块中以下表单的导出语法：

```js
export * from "x.js";
export { name } from "x.js";
```

 这实际上是导入后跟导出的简写，即“我导入模块 x.js，然后重新导出部分或全部导出”。




#### 动态加载模块


浏览器中可用的 JavaScript 模块功能的最新部分是动态模块加载。这允许你仅在需要时动态加载模块，而不必预先加载所有模块。

这个新功能允许你将 import() 作为函数调用，将模块的路径作为参数传递。它返回一个 promise，它用一个模块对象来实现（参见创建模块对象），让你可以访问该对象的导出，例如

```js
import("/modules/mymodule.js").then((module) => {
  // Do something with the module.
});
```

```js
squareBtn.addEventListener("click", () => {
  import("/js-examples/modules/dynamic-module-imports/modules/square.js").then(
    (Module) => {
      let square1 = new Module.Square(
        myCanvas.ctx,
        myCanvas.listId,
        50,
        50,
        100,
        "blue",
      );
      square1.draw();
      square1.reportArea();
      square1.reportPerimeter();
    },
  );
});
```

#### 故障排除

- 在前面已经提到了，在这里再重申一次： .mjs 后缀的文件需要以 MIME-type 为 javascript/esm 来加载 (或者其他的 JavaScript 兼容的 MIME-type，比如 application/javascript), 否则，你会一个严格的 MIME 类型检查错误，像是这样的 "The server responded with a non-JavaScript MIME type".
- 如果你尝试用本地文件加载 HTML 文件 (i.e. with a file:// URL)，由于 JavaScript 模块的安全性要求，你会遇到 CORS 错误。你需要通过服务器来做你的测试。GitHub pages is ideal as it also serves .mjs files with the correct MIME type.
- 因为 .mjs 是一个相当新的文件后缀，一些操作系统可能无法识别，或者尝试把它替换成别的。比如，我们发现 macOS 悄悄地在我们的 .mjs 后缀的文件后面添加上 .js 然后自动隐藏这个后缀。所以我们的文件实际上都是 x.mjs.js。当我们关闭自动隐藏文件后缀名，让它去接受认可 .mjs。问题解决。







































































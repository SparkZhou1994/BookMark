# 入门篇
## 导论
### 什么是JavaScript语言
JavaScript是脚本语言，因此不具备开发操作系统的能力，而是只用来编写控制其他大型应用程序（比如浏览器）； 也是嵌入式语言，只能用来做一些数学和逻辑运算，本身不提供任何与 I/O（输入/输出）相关的 API。
目前，已经嵌入 JavaScript 的宿主环境有多种，最常见的环境就是浏览器，另外还有服务器环境，也就是 Node 项目。
JavaScript 语言是一种“对象模型”语言，也支持其他编程范式(比如函数式编程)。
JavaScript可以调用标准库(如Array、Date、Math等)和宿主环境提供额外的API。
以游览器为例，浏览器控制类(操作浏览器)、DOM类(操作网页的各种元素)、Web类(实现互联网的各种功能)
以服务器为例，Node环境中的API。
### 为什么学习JavaScript
#### 操控浏览器的能力
#### 广泛的使用领域
- 浏览器的平台化
- Node
- 数据库操作(NoSQL)
- 移动平台开发
- 内嵌脚本语言
- 跨平台的桌面应用程序

#### 易学性
JavaScript 语言有一些设计缺陷。某些地方相当不合理，另一些地方则会出现怪异的运行结果。学习 JavaScript，很大一部分时间是用来搞清楚哪些地方有陷阱。Douglas Crockford 写过一本有名的书，名字就叫《JavaScript: The Good Parts》，言下之意就是这门语言不好的地方很多，必须写一本书才能讲清楚。另外一些程序员则感到，为了更合理地编写 JavaScript 程序，就不能用 JavaScript 来写，而必须发明新的语言，比如 CoffeeScript、TypeScript、Dart 这些新语言的发明目的，多多少少都有这个因素。

#### 强大的性能
- 灵活的语法，表达力强
JavaScript 既支持类似 C 语言清晰的过程式编程，也支持灵活的函数式编程，可以用来写并发处理（concurrent）。这些语法特性已经被证明非常强大，可以用于许多场合，尤其适用异步编程。
- 支持编译运行
JavaScript 语言本身，虽然是一种解释型语言，但是在现代浏览器中，JavaScript 都是编译后运行。程序会被高度优化，运行效率接近二进制程序。而且，JavaScript 引擎正在快速发展，性能将越来越好。
- 事件驱动和非阻塞式设计
JavaScript 程序可以采用事件驱动（event-driven）和非阻塞式（non-blocking）设计，在服务器端适合高并发环境，普通的硬件就可以承受很大的访问量。

#### 开放性
JavaScript 是一种开放的语言。它的标准 ECMA-262 是 ISO 国际标准，写得非常详尽明确；该标准的主要实现（比如 V8 和 SpiderMonkey 引擎）都是开放的，而且质量很高。这保证了这门语言不属于任何公司或个人，不存在版权和专利的问题。
### 实验环境
Chrome直接进入：按下Option + Command + J（Mac）或者Ctrl + Shift + J（Windows / Linux）
按Shift + Enter键，就是代码换行，不会触发执行。
## 历史
### JavaScript与ECMAScript的关系
ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现。
### 周边大事记
1996年，样式表标准 CSS 第一版发布。
1997年，DHTML（Dynamic HTML，动态 HTML）发布，允许动态改变网页内容。这标志着 DOM 模式（Document Object Model，文档对象模型）正式应用。
1999年，IE 5部署了 XMLHttpRequest 接口，允许 JavaScript 发出 HTTP 请求，为后来大行其道的 Ajax 应用创造了条件。
2001年，Douglas Crockford 提出了 JSON 格式，用于取代 XML 格式，进行服务器和网页之间的数据交换。JavaScript 可以原生支持这种格式，不需要额外部署代码。
2004年，Google 公司发布了 Gmail，促成了互联网应用程序（Web Application）这个概念的诞生。由于 Gmail 是在4月1日发布的，很多人起初以为这只是一个玩笑。
2004年，Dojo 框架诞生，为不同浏览器提供了同一接口，并为主要功能提供了便利的调用方法。这标志着 JavaScript 编程框架的时代开始来临。
2004年，WHATWG 组织成立，致力于加速 HTML 语言的标准化进程。
2005年，苹果公司在 KHTML 引擎基础上，建立了 WebKit 引擎。 Mozilla 的 Gecko、Google 的 Blink、还有苹果的的 WebKit（Blink 的近亲）。
Mozilla 的 Gecko、Google 的 Blink、还有苹果的的 WebKit（Blink 的近亲）。 Blink 是 Google Chrome 浏览器的渲染引擎，V8 是 Blink 内置的 JavaScript 引擎。Chromium 是 Google 公司一个开源浏览器项目，使用 Blink 渲染引擎驱动。
2005年，Ajax 方法（Asynchronous JavaScript and XML）正式诞生，Jesse James Garrett 发明了这个词汇。它开始流行的标志是，2月份发布的 Google Maps 项目大量采用该方法。它几乎成了新一代网站的标准做法，促成了 Web 2.0时代的来临。
2005年，Apache 基金会发布了 CouchDB 数据库。这是一个基于 JSON 格式的数据库，可以用 JavaScript 函数定义视图和索引。它在本质上有别于传统的关系型数据库，标识着 NoSQL 类型的数据库诞生。
2006年，jQuery 函数库诞生，作者为John Resig。jQuery 为操作网页 DOM 结构提供了非常强大易用的接口，成为了使用最广泛的函数库，并且让 JavaScript 语言的应用难度大大降低，推动了这种语言的流行。
2006年，Google推出 Google Web Toolkit 项目（缩写为 GWT），提供 Java 编译成 JavaScript 的功能，开创了将其他语言转为 JavaScript 的先河。
2007年，Webkit 引擎在 iPhone 手机中得到部署。它最初基于 KDE 项目，2003年苹果公司首先采用，2005年开源。这标志着 JavaScript 语言开始能在手机中使用了，意味着有可能写出在桌面电脑和手机中都能使用的程序。
2007年，Douglas Crockford 发表了名为《JavaScript: The good parts》的演讲，次年由 O'Reilly 出版社出版。这标志着软件行业开始严肃对待 JavaScript 语言，对它的语法开始重新认识。
2008年，V8 编译器诞生。这是 Google 公司为 Chrome 浏览器而开发的，它的特点是让 JavaScript 的运行变得非常快。它提高了 JavaScript 的性能，推动了语法的改进和标准化，改变外界对 JavaScript 的不佳印象。同时，V8 是开源的，任何人想要一种快速的嵌入式脚本语言，都可以采用 V8，这拓展了 JavaScript 的应用领域。
2009年，Node.js 项目诞生，创始人为 Ryan Dahl，它标志着 JavaScript 可以用于服务器端编程，从此网站的前端和后端可以使用同一种语言开发。并且，Node.js 可以承受很大的并发流量，使得开发某些互联网大规模的实时应用变得容易。
2009年，Jeremy Ashkenas 发布了 CoffeeScript 的最初版本。CoffeeScript 可以被转换为 JavaScript 运行，但是语法要比 JavaScript 简洁。这开启了其他语言转为 JavaScript 的风潮。
2009年，PhoneGap 项目诞生，它将 HTML5 和 JavaScript 引入移动设备的应用程序开发，主要针对 iOS 和 Android 平台，使得 JavaScript 可以用于跨平台的应用程序开发。
2010年，三个重要的项目诞生，分别是 NPM、BackboneJS 和 RequireJS，标志着 JavaScript 进入模块化开发的时代。
2011年，Google 发布了 Dart 语言，目的是为了结束 JavaScript 语言在浏览器中的垄断，提供更合理、更强大的语法和功能。Chromium浏览器有内置的 Dart 虚拟机，可以运行 Dart 程序，但 Dart 程序也可以被编译成 JavaScript 程序运行。
2012年，单页面应用程序框架（single-page app framework）开始崛起，AngularJS 项目和 Ember 项目都发布了1.0版本。
2012年，微软发布 TypeScript 语言。该语言被设计成 JavaScript 的超集，这意味着所有 JavaScript 程序，都可以不经修改地在 TypeScript 中运行。同时，TypeScript 添加了很多新的语法特性，主要目的是为了开发大型程序，然后还可以被编译成 JavaScript 运行。
2012年，Mozilla 基金会提出 asm.js 规格。asm.js 是 JavaScript 的一个子集，所有符合 asm.js 的程序都可以在浏览器中运行，它的特殊之处在于语法有严格限定，可以被快速编译成性能良好的机器码。这样做的目的，是为了给其他语言提供一个编译规范，使其可以被编译成高效的 JavaScript 代码。同时，Mozilla 基金会还发起了 Emscripten 项目，目标就是提供一个跨语言的编译器，能够将 LLVM 的位代码（bitcode）转为 JavaScript 代码，在浏览器中运行。因为大部分 LLVM 位代码都是从 C / C++ 语言生成的，这意味着 C / C++ 将可以在浏览器中运行。此外，Mozilla 旗下还有 LLJS （将 JavaScript 转为 C 代码）项目和 River Trail （一个用于多核心处理器的 ECMAScript 扩展）项目。目前，可以被编译成 JavaScript 的语言列表，共有将近40种语言。
2013年，ECMA 正式推出 JSON 的国际标准，这意味着 JSON 格式已经变得与 XML 格式一样重要和正式了。
2013年5月，Facebook 发布 UI 框架库 React，引入了新的 JSX 语法，使得 UI 层可以用组件开发，同时引入了网页应用是状态机的概念。
2015年4月，Angular 框架宣布，2.0 版将基于微软公司的TypeScript语言开发，这等于为 JavaScript 语言引入了强类型。
2015年5月，Google 公司的 Polymer 框架发布1.0版。该项目的目标是生产环境可以使用 WebComponent 组件，如果能够达到目标，Web 开发将进入一个全新的以组件为开发基础的阶段。
2015年6月，ECMA 标准化组织正式批准了 ECMAScript 6 语言标准，定名为《ECMAScript 2015 标准》。JavaScript 语言正式进入了下一个阶段，成为一种企业级的、开发大规模应用的语言。这个标准从提出到批准，历时10年，而 JavaScript 语言从诞生至今也已经20年了。
2015年6月，Mozilla 在 asm.js 的基础上发布 WebAssembly 项目。这是一种 JavaScript 引擎的中间码格式，全部都是二进制，类似于 Java 的字节码，有利于移动设备加载 JavaScript 脚本，执行速度提高了 20+ 倍。这意味着将来的软件，会发布 JavaScript 二进制包。
2017年11月，所有主流浏览器全部支持 WebAssembly，这意味着任何语言都可以编译成 JavaScript，在浏览器运行。

## 基本语法
### 变量
#### 概念
如果使用var重新声明一个已经存在的变量，是无效的。但是，如果第二次声明的时候还进行了赋值，则会覆盖掉前面的值。
#### 变量提升
JavaScript引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。
### 注释
由于历史上JavaScript可以兼容HTML代码的注释，所以<!--和-->也被视为合法的单行注释。
需要注意的是，-->只有在行首，才会被当成单行注释，否则会当作正常的运算(-- >)。
### 区块
JavaScript 使用大括号，将多个相关的语句组合在一起，称为“区块”（block）。
对于var命令来说，JavaScript的区块不构成单独的作用域（scope）。
### 条件语句
#### if结构
if后面的表达式之中，不要混淆赋值表达式（=）、严格相等运算符（===）和相等运算符（==）。尤其是赋值表达式不具有比较作用。
为了避免这种情况，有些开发者习惯将常量写在运算符的左边，这样的话，一旦不小心将相等运算符写成赋值运算符，就会报错，因为常量不能被赋值。
#### switch结构
需要注意的是，switch语句后面的表达式，与case语句后面的表示式比较运行结果时，采用的是严格相等运算符（===），而不是相等运算符（==），这意味着比较时不会发生类型转换。
```
var x = 1;

switch (x) {
  case true:
    console.log('x 发生类型转换');
    break;
  default:
    console.log('x 没有发生类型转换');
}
// x 没有发生类型转换，所以不会执行case true的情况。这表明，switch语句内部采用的是“严格相等运算符”
```
### 循环语句
#### 标签（label）
JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下。
```
label:
  语句
```
标签通常与break语句和continue语句配合使用，跳出特定的循环。
```
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
```
上面代码为一个双重循环区块，break命令后面加上了top标签（注意，top不用加引号），满足条件时，直接跳出双层循环。如果break语句后面不使用标签，则只能跳出内层循环，进入下一次的外层循环。
```
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) continue top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
// i=2, j=0
// i=2, j=1
// i=2, j=2
```
上面代码中，continue命令后面有一个标签名，满足条件时，会跳过当前循环，直接进入下一轮外层循环。如果continue语句后面不使用标签，则只能进入下一轮的内层循环。

# 数据类型
## 概述
### 简介
undefined：表示“未定义”或不存在，即由于目前没有定义，所以此处暂时没有任何值。
null：表示空值，即此处的值为空。
对象是最复杂的数据类型，又可以分成三个子类型。
- 狭义的对象（object）
- 数组（array）
- 函数（function）

### typeof 运算符
JavaScript 有三种方法，可以确定一个值到底是什么类型。
- typeof运算符
- instanceof运算符
- Object.prototype.toString方法
typeof返回number、string、boolean、function、object、undefined。可以用来检查一个没有声明的变量，而不报错。
typeof将null、array和object都定义为object。instanceof运算符可以区分数组和对象。null的类型是object，这是由于历史原因造成的。1995年的 JavaScript 语言第一版，只设计了五种数据类型（对象、整数、浮点数、字符串和布尔值），没考虑null，只把它当作object的一种特殊值。后来null独立出来，作为一种单独的数据类型，为了兼容以前的代码，typeof null返回object就没法改变了。

## null, undefined 和布尔值
### null 和 undefined
#### 概述
将一个变量赋值为undefined或null，老实说，语法效果几乎没区别。
在if语句中，它们都会被自动转为false，相等运算符（==）甚至直接报告两者相等。
谷歌公司开发的 JavaScript 语言的替代品 Dart 语言，就明确规定只有null，没有undefined！
根据 C 语言的传统，null可以自动转为0。JavaScript 的设计者 Brendan Eich觉得，如果null自动转为0，很不容易发现错误。因此，他又设计了一个undefined。区别是这样的：null是一个表示“空”的对象，转为数值时为0；undefined是一个表示“此处无定义”的原始值，转为数值时为NaN。
#### 用法和含义
undefined表示“未定义”，下面是返回undefined的典型场景。
```
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于 undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
```
### 布尔值
 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。转换规则是除了下面六个值被转为false，其他值都视为true。注意，空数组（[]）和空对象（{}）对应的布尔值，都是true。
 - undefined
 - null
 - false
 - 0
 - NaN
 - ""或''（空字符串）

## 数值
### 概述
#### 整数和浮点数
JavaScript 内部，所有数字都是以64位浮点数形式储存，即使整数也是如此。所以，1与1.0是相同的，是同一个数。容易造成混淆的是，某些运算只有整数才能完成，此时 JavaScript 会自动把64位浮点数，转成32位整数，然后再进行运算。
### 数值的表示法
八进制：有前缀0o或0O的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值。前导0表示八进制，处理时很容易造成混乱。ES5 的严格模式和 ES6，已经废除了这种表示法，但是浏览器为了兼容以前的代码，目前还继续支持这种表示法。
十六进制：有前缀0x或0X的数值。
二进制：有前缀0b或0B的数值。
### 特殊数值
#### 正零和负零
几乎所有场合，正零和负零都会被当作正常的0。唯一有区别的场合是，+0或-0当作分母，返回的值是不相等的。之所以出现这样结果，是因为除以正零得到+Infinity，除以负零得到-Infinity，这两者是不相等的
#### NaN
NaN是 JavaScript 的特殊值，表示“非数字”（Not a Number），主要出现在将字符串解析成数字出错的场合。
0除以0也会得到NaN。
NaN不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number，使用typeof运算符可以看得很清楚。
NaN不等于任何值，包括它本身。
NaN与任何数（包括它自己）的算术运算，得到的都是NaN。但是，ES6 引入指数运算符（**）后，出现了一个例外。
#### Infinity
由于数值正向溢出（overflow）、负向溢出（underflow）和被0除，JavaScript 都不报错，所以单纯的数学运算几乎没有可能抛出错误。
Infinity大于一切数值（除了NaN），-Infinity小于一切数值（除了NaN）
Infinity与NaN比较，总是返回false。0乘以Infinity，返回NaN；Infinity减去或除以Infinity，得到NaN。Infinity与null计算时，null会转成0，等同于与0的计算。Infinity与undefined计算，返回的都是NaN。
#### 与数值相关的全局方法
##### parseInt()
字符串转为整数的时候，是一个个字符依次转换，如果遇到不能转为数字的字符，就不再进行下去，返回已经转好的部分。
如果字符串的第一个字符不能转化为数字（后面跟着数字的正负号除外），返回NaN。
如果字符串以0x或0X开头，parseInt会将其按照十六进制数解析。但如果字符串以0开头，将其按照10进制解析。对于那些会自动转为科学计数法的数字，parseInt会将科学计数法的表示方法视为字符串，因此导致一些奇怪的结果。
parseInt方法还可以接受第二个参数（2到36之间），表示被解析的值的进制，返回该值对应的十进制数。如果第二个参数不是数值，会被自动转为一个整数。这个整数只有在2到36之间，才能得到有意义的结果，超出这个范围，则返回NaN。如果第二个参数是0、undefined和null，则直接忽略。如果字符串包含对于指定进制无意义的字符，则从最高位开始，只返回可以转换的数值。如果最高位无法转换，则直接返回NaN。
一些意外的结果
```
parseInt(0x11, 36) // 43
parseInt(0x11, 2) // 1

// 等同于
parseInt(String(0x11), 36)
parseInt(String(0x11), 2)

// 等同于
parseInt('17', 36) //将十六进制0x11转为十进制17，再转为字符串。用36进制或二进制解读字符串17
parseInt('17', 2)

// 再一个八进制前缀0案例
parseInt(011, 2) // NaN

// 等同于
parseInt(String(011), 2)

// 等同于
parseInt(String(9), 2) // 第一行的011会被先转为字符串9，因为9不是二进制的有效字符，所以返回NaN。

// 再一个二进制案例
parseInt('011', 2) // 3
```
##### parseFloat()
如果参数不是字符串，则会先转为字符串再转换。如果字符串的第一个字符不能转化为浮点数，则返回NaN。
```
parseFloat([1.23]) // 1.23
// 等同于
parseFloat(String([1.23])) // 1.23
```
这些特点使得parseFloat的转换结果不同于Number函数。
```
parseFloat(true)  // NaN
Number(true) // 1

parseFloat(null) // NaN
Number(null) // 0

parseFloat('') // NaN
Number('') // 0

parseFloat('123.45#') // 123.45
Number('123.45#') // NaN
```
##### isNaN()
isNaN方法可以用来判断一个值是否为NaN。isNaN只对数值有效，如果传入其他值，会被先转成数值。比如，传入字符串的时候，字符串会被先转成NaN，所以最后返回true，这一点要特别引起注意。也就是说，isNaN为true的值，有可能不是NaN，而是一个字符串。
```
isNaN('Hello') // true
// 相当于
isNaN(Number('Hello')) // true

isNaN({}) // true
// 等同于
isNaN(Number({})) // true

isNaN(['xzy']) // true
// 等同于
isNaN(Number(['xzy'])) // true

isNaN([]) // false
isNaN([123]) // false
isNaN(['123']) // false
```
因此，使用isNaN之前，最好判断一下数据类型。
```
function myIsNaN(value) {
  return typeof value === 'number' && isNaN(value);
}
```
判断NaN更可靠的方法是，利用NaN为唯一不等于自身的值的这个特点，进行判断。
```
function myIsNaN(value) {
  return value !== value;
}
```
##### isFinite()
isFinite方法返回一个布尔值，表示某个值是否为正常的数值。除了Infinity、-Infinity、NaN和undefined这几个值会返回false，isFinite对于其他的数值都会返回true。
## 字符串
### 概述
#### 定义
单引号字符串的内部，可以使用双引号。双引号字符串的内部，可以使用单引号。由于 HTML 语言的属性值使用双引号，所以很多项目约定 JavaScript 语言的字符串只使用单引号。
字符串默认只能写在一行内，分成多行将会报错。如果长字符串必须分成多行，可以在每一行的尾部使用反斜杠。但是，输出的时候还是单行，效果与写在同一行完全一样。注意，反斜杠的后面必须是换行符，而不能有其他字符（比如空格），否则会报错。
如果想输出多行字符串，有一种利用多行注释的变通方法。
```
(function () { /*
line 1
line 2
line 3
*/}).toString().split('\n').slice(1, -1).join('\n')
// "line 1
// line 2
// line 3"
```
#### 转义
反斜杠有三种特殊用法。
- \HHH
反斜杠后面紧跟三个八进制数（000到377），代表一个字符。HHH对应该字符的 Unicode 码点，比如\251表示版权符号。
- \xHH
\x后面紧跟两个十六进制数（00到FF），代表一个字符。
- \uXXXX
\u后面紧跟四个十六进制数（0000到FFFF），代表一个字符。XXXX对应该字符的 Unicode 码点。

### 字符集
JavaScript 引擎内部，所有字符都用 Unicode 表示。每个字符在 JavaScript 内部都是以16位（即2个字节）的 UTF-16 格式储存。
但是，UTF-16 有两种长度：对于码点在U+0000到U+FFFF之间的字符，长度为16位（即2个字节）；对于码点在U+10000到U+10FFFF之间的字符，长度为32位（即4个字节），而且前两个字节在0xD800到0xDBFF之间，后两个字节在0xDC00到0xDFFF之间。举例来说，码点U+1D306对应的字符为𝌆，它写成 UTF-16 就是0xD834 0xDF06。认为它们是两个字符（length属性为2）。所以处理的时候，必须把这一点考虑在内，也就是说，JavaScript 返回的字符串长度可能是不正确的。
### Base64 转码
有时，文本里面包含一些不可打印的符号，比如 ASCII 码0到31的符号都无法打印出来，这时可以使用 Base64 编码，将它们转成可以打印的字符。另一个场景是，有时需要以文本格式传递二进制数据，那么也可以使用 Base64 编码。
JavaScript 原生提供两个 Base64 相关的方法。但这两个方法不适合非 ASCII 码的字符，会报错。要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法。
- btoa()：任意值转为 Base64 编码
- atob()：Base64 编码转为原来的值

## 对象
### 概述
#### 生成方法
#### 键名
对象的所有键名都是字符串（ES6 又引入了 Symbol 值也可以作为键名），所以加不加引号都可以。如果键名不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），且也不是数字，则必须加上引号，否则会报错。
对象的每一个键名又称为“属性”（property），它的“键值”可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为“方法”，它可以像函数那样调用。
如果属性的值还是一个对象，就形成了链式引用。
属性可以动态创建，不必在对象声明时就指定。
#### 对象的引用
如果取消某一个变量对于原对象的引用，不会影响到另一个变量。
```
var o1 = {};
var o2 = o1;

o1 = 1;
o2 // {} o1和o2指向同一个对象，然后o1的值变为1，这时不会对o2产生影响，o2还是指向原来的那个对象。
```
#### 表达式还是语句？
为了避免这种歧义，JavaScript 引擎的做法是，如果遇到这种情况，无法确定是对象还是代码块，一律解释为代码块。
如果要解释为对象，最好在大括号前加上圆括号。因为圆括号的里面，只能是表达式，所以确保大括号只能解释为对象。
这种差异在eval语句（作用是对字符串求值）中反映得最明显。
```
eval('{foo: 123}') // 123
eval('({foo: 123})') // {foo: 123}
```
### 属性的操作
#### 属性的读取
```
var foo = 'bar';

var obj = {
  foo: 1,
  bar: 2
};

obj.foo  // 1
obj[foo]  // 2  如果使用方括号运算符，键名必须放在引号里面，否则会被当作变量处理。
obj['foo']  // 1
```
方括号运算符内部还可以使用表达式。数字键可以不加引号，因为会自动转成字符串。数值键名不能使用点运算符（因为会被当成小数点），只能使用方括号运算符。
#### 属性的查看
查看一个对象本身的所有属性，可以使用Object.keys方法。
#### 属性的删除：delete 命令
delete命令用于删除对象的属性，删除成功后返回true。删除一个不存在的属性，delete不报错，而且返回true。delete可能返回false，因为有些属性是不能删除的。即使delete返回true，该属性依然可能读取到值。
delete命令只能删除对象本身的属性，无法删除继承的属性。
#### 属性是否存在：in 运算符
in运算符的一个问题是，它不能识别哪些属性是对象自身的，哪些属性是继承的。这时，可以使用对象的hasOwnProperty方法判断一下，是否为对象自身的属性。
#### 属性的遍历：for...in 循环
- 它遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性。
- 它不仅遍历对象自身的属性，还遍历继承的属性。
一般情况下，都是只想遍历对象自身的属性，所以使用for...in的时候，应该结合使用hasOwnProperty方法，在循环内部判断一下，某个属性是否为对象自身的属性。
### with 语句
它的作用是操作同一个对象的多个属性时，提供一些书写的方便。如果with区块内部有变量的赋值操作，必须是当前对象已经存在的属性，否则会创造一个当前作用域的全局变量。这是因为with区块没有改变作用域，它的内部依然是当前作用域。这造成了with语句的一个很大的弊病，就是绑定对象不明确。这非常不利于代码的除错和模块化，编译器也无法对这段代码进行优化，只能留到运行时判断，这就拖慢了运行速度。因此，建议不要使用with语句，可以考虑用一个临时变量代替with。
```
// 示例
with (document.links[0]){
  console.log(href);
  console.log(title);
  console.log(style);
}
// 等同于
console.log(document.links[0].href);
console.log(document.links[0].title);
console.log(document.links[0].style);
```
## 函数
### 概述
#### 函数的声明
- function 命令
```
function print(s) {
  console.log(s);
}
```
- 函数表达式
```
var print = function x(){
  console.log(typeof x);
};

x
// ReferenceError: x is not defined

print()
// function
```
采用函数表达式声明函数时，function命令后面不带有函数名。如果加上函数名，该函数名只在函数体内部有效，在函数体外部无效。
这种写法的用处有两个，一是可以在函数体内部调用自身，二是方便除错（除错工具显示函数调用栈时，将显示函数名，而不再显示这里是一个匿名函数）。
- Function 构造函数
```
var add = new Function(
  'x',
  'y',
  'return x + y'
);

// 等同于
function add(x, y) {
  return x + y;
}
```
你可以传递任意数量的参数给Function构造函数，只有最后一个参数会被当做函数体，如果只有一个参数，该参数就是函数体。
#### 函数的重复声明
如果同一个函数被多次声明，后面的声明就会覆盖前面的声明。
而且，由于函数名的提升，前一次声明在任何时候都是无效的，这一点要特别注意。
#### 圆括号运算符，return 语句和递归
return语句不是必需的，如果没有的话，该函数就不返回任何值，或者说返回undefined。
#### 第一等公民
JavaScript 语言将函数看作一种值，与其它值（数值、字符串、布尔值等等）地位相同。凡是可以使用值的地方，就能使用函数。比如，可以把函数赋值给变量和对象的属性，也可以当作参数传入其他函数，或者作为函数的结果返回。函数只是一个可以执行的值，此外并无特殊之处。
```
function add(x, y) {
  return x + y;
}

// 将函数赋值给一个变量
var operator = add;

// 将函数作为参数和返回值
function a(op){
  return op;
}
a(add)(1, 1)
// 2
```
#### 函数名的提升
JavaScript 引擎将函数名视同变量名，所以采用function命令声明函数时，整个函数会像变量声明一样，被提升到代码头部。
但是，如果采用赋值语句定义函数，JavaScript 就会报错。
```
f();
var f = function (){};
// TypeError: undefined is not a function

// 上面的代码等同于下面的形式。
var f;
f();
f = function () {}; // 上面代码第二行，调用f的时候，f只是被声明了，还没有被赋值，等于undefined，所以会报错。

// 如果像下面例子那样，采用function命令和var赋值语句声明同一个函数，由于存在函数提升，最后会采用var赋值语句的定义。
var f = function () {
  console.log('1');
}

function f() {
  console.log('2');
}

f() // 1
// 上面例子中，表面上后面声明的函数f，应该覆盖前面的var赋值语句，但是由于存在函数提升，实际上正好反过来。
```
### 函数的属性和方法
#### name 属性
只有在变量的值是一个匿名函数时才返回变量名。如果变量的值是一个具名函数，那么name属性返回function关键字之后的那个函数名。
name属性的一个用处，就是获取参数函数的名字。
#### length 属性
函数的length属性返回函数预期传入的参数个数，即函数定义之中的参数个数。
length属性提供了一种机制，判断定义时和调用时参数的差异，以便实现面向对象编程的“方法重载”（overload）。
#### toString()
函数的toString()方法返回一个字符串，内容是函数的源码，内部的注释也可以返回。
对于那些原生的函数，toString()方法返回function (){[native code]}。 
### 函数作用域
#### 定义
函数内部定义的变量，会在该作用域内覆盖同名全局变量。
变量在条件判断区块之中(非函数内部)声明，结果就是一个全局变量，可以在区块之外读取。
#### 函数内部的变量提升
var命令声明的变量，不管在什么位置，变量声明都会被提升到函数体的头部。
#### 函数本身的作用域
```
var a = 1;
var x = function () {
  console.log(a);
};

function f() {
  var a = 2;
  x();
}

f() // 1
// 上面代码中，函数x是在函数f的外部声明的，所以它的作用域绑定外层，内部变量a不会到函数f体内取值，所以输出1，而不是2。
// 函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。
```
很容易犯错的一点是，如果函数A调用函数B，却没考虑到函数B不会引用函数A的内部变量。
```
var x = function () {
  console.log(a);
};

function y(f) {
  var a = 2;
  f();
}

y(x)
// ReferenceError: a is not defined
// 上面代码将函数x作为参数，传入函数y。但是，函数x是在函数y体外声明的，作用域绑定外层，因此找不到函数y的内部变量a，导致报错。
```
同样的，函数体内部声明的函数，作用域绑定函数体内部。
```
function foo() {
  var x = 1;
  function bar() {
    console.log(x);
  }
  return bar;
}

var x = 2;
var f = foo();
f() // 1
// 上面代码中，函数foo内部声明了一个函数bar，bar的作用域绑定foo。当我们在foo外部取出bar执行时，变量x指向的是foo内部的x，而不是foo外部的x。正是这种机制，构成了下文要讲解的“闭包”现象。
```
### 参数
#### 参数的省略
省略的参数的值就变为undefined。
但是，没有办法只省略靠前的参数，而保留靠后的参数。如果一定要省略靠前的参数，只有显式传入undefined。
#### 同名参数
如果有同名的参数，则取最后出现的那个值。即使后面的a没有值或被省略，也是以其为准，取值就变成了undefined。如果要获得第一个a的值，可以使用arguments对象。
#### arguments 对象
##### 定义
由于 JavaScript 允许函数有不定数目的参数，所以需要一种机制，可以在函数体内部读取所有参数。这就是arguments对象的由来。
正常模式下，arguments对象可以在运行时修改。
严格模式下，arguments对象与函数参数不具有联动关系。也就是说，修改arguments对象不会影响到实际的函数参数。
通过arguments对象的length属性，可以判断函数调用时到底带几个参数。
##### 与数组的关系
需要注意的是，虽然arguments很像数组，但它是一个对象。数组专有的方法（比如slice和forEach），不能在arguments对象上直接使用。
如果要让arguments对象使用数组方法，真正的解决方法是将arguments转为真正的数组。下面是两种常用的转换方法：slice方法和逐一填入新数组。
```
var args = Array.prototype.slice.call(arguments);

// 或者
var args = [];
for (var i = 0; i < arguments.length; i++) {
  args.push(arguments[i]);
}
```
##### callee 属性
arguments对象带有一个callee属性，返回它所对应的原函数。
可以通过arguments.callee，达到调用函数自身的目的。这个属性在严格模式里面是禁用的，因此不建议使用。
### 函数的其他知识点
#### 闭包
如果出于种种原因，需要得到函数内的局部变量。正常情况下，这是办不到的，只有通过变通方法才能实现。那就是在函数的内部，再定义一个函数。
```
function f1() {
  var n = 999;
  function f2() {
　　console.log(n); // 999
  }
}
```
f1内部的所有局部变量，对f2都是可见的。但是反过来就不行，f2内部的局部变量，对f1就是不可见的。这就是 JavaScript 语言特有的"链式作用域"结构（chain scope），子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。
闭包简单理解成“定义在一个函数内部的函数”。闭包最大的特点，就是它可以“记住”诞生的环境。在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
闭包的最大用处有两个，一个是可以读取外层函数内部的变量，另一个就是让这些变量始终保持在内存中，即闭包可以使得它诞生环境一直存在。
为什么闭包能够返回外层函数的内部变量？原因是闭包（上例的inc）用到了外层变量（start），导致外层函数（createIncrementor）不能从内存释放。只要闭包没有被垃圾回收机制清除，外层函数提供的运行环境也不会被清除，它的内部变量就始终保存着当前值，供闭包读取。
闭包的另一个用处，是封装对象的私有属性和私有方法。
```
function Person(name) {
  var _age;
  function setAge(n) {
    _age = n;
  }
  function getAge() {
    return _age;
  }

  return {
    name: name,
    getAge: getAge,
    setAge: setAge
  };
}

var p1 = Person('张三');
p1.setAge(25);
p1.getAge() // 25
```
注意，外层函数每次运行，都会生成一个新的闭包，而这个闭包又会保留外层函数的内部变量，所以内存消耗很大。因此不能滥用闭包，否则会造成网页的性能问题。
#### 立即调用的函数表达式（IIFE）
有时，我们需要在定义函数之后，立即调用该函数。这时，你不能在函数的定义之后加上圆括号，这会产生语法错误。
产生这个错误的原因是，function这个关键字既可以当作语句，也可以当作表达式。当作表达式时，函数可以定义后直接加圆括号调用。为了避免解析的歧义，JavaScript 规定，如果function关键字出现在行首，一律解释成语句。函数定义后立即调用的解决方法，就是不要让function出现在行首，让引擎将其理解成一个表达式。最简单的处理，就是将其放在一个圆括号里面。
```
// 语句
function f() {}

// 表达式
var f = function f() {} ();
(function(){ /* code */ }()); //IIFE
(function(){ /* code */ })(); //IIFE
```
通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。它的目的有两个：一是不必为函数命名，避免了污染全局变量；二是 IIFE 内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。
```
// 写法一
var tmp = newData;
processData(tmp);
storeData(tmp);

// 写法二
(function () {
  var tmp = newData;
  processData(tmp);
  storeData(tmp);
}());
```
上面代码中，写法二比写法一更好，因为完全避免了污染全局变量。
### eval 命令
#### 基本用法
eval命令接受一个字符串作为参数，并将这个字符串当作语句执行。不能执行无效语句或者return语句。
eval没有自己的作用域，都在当前作用域内执行，因此可能会修改当前作用域的变量的值，造成安全问题。为了防止这种风险，JavaScript 规定，如果使用严格模式，eval内部声明的变量，不会影响到外部作用域。不过，即使在严格模式下，eval依然可以读写当前作用域的变量。
```
(function f() {
  'use strict';
  eval('var foo = 123');
  console.log(foo);  // ReferenceError: foo is not defined

  var foo = 1;
  eval('foo = 2');
  console.log(foo);  // 2
})()
```
由于安全风险和不利于 JavaScript 引擎优化执行速度，一般不推荐使用。通常情况下，eval最常见的场合是解析 JSON 数据的字符串，不过正确的做法应该是使用原生的JSON.parse方法。
#### eval 的别名调用
为了保证eval的别名不影响代码优化，JavaScript 的标准规定，凡是使用别名执行eval，eval内部一律是全局作用域。样的话，引擎就能确认其别名不会对当前的函数作用域产生影响，优化的时候就可以把这一行排除掉。
```
var a = 1;

function f() {
  var a = 2;
  var e = eval;
  e('console.log(a)');
}

f() // 1
```
eval的别名调用的形式五花八门，只要不是直接调用，都属于别名调用，因为引擎只能分辨eval()这一种形式是直接调用。
```
eval.call(null, '...')
window.eval('...')
(1, eval)('...')
(eval, eval)('...')
```
## 数组
### 定义
任何类型的数据，都可以放入数组。
### 数组的本质
本质上，数组属于一种特殊的对象。typeof运算符会返回数组的类型是object。
Object.keys方法返回数组的所有键名。可以看到数组的键名就是整数0、1、2。
由于数组成员的键名是固定的（默认总是0、1、2...），因此数组不用为每个元素指定键名，而对象的每个成员都必须指定键名。JavaScript 语言规定，对象的键名一律为字符串，所以，数组的键名其实也是字符串。之所以可以用数值读取，是因为非字符串的键名会被转为字符串。
对象有两种读取成员的方法：点结构（object.key）和方括号结构（object[key]）。但是，对于数值的键名，不能使用点结构。
### length 属性
数组的length属性，返回数组的成员数量。JavaScript 使用一个32位整数，保存数组的元素个数。这意味着，数组成员最多只有 4294967295 个（232 - 1）个，也就是说length属性的最大值就是 4294967295。
清空数组的一个有效方法，就是将length属性设为0。
值得注意的是，由于数组本质上是一种对象，所以可以为数组添加属性，但是这不影响length属性的值。
```
var a = [];

a['p'] = 'abc';
a.length // 0

a[2.1] = 'abc';
a.length // 0
```
上面代码将数组的键分别设为字符串和小数，结果都不影响length属性。因为，length属性的值就是等于最大的数字键加1，而这个数组没有整数键，所以length属性保持为0。
### in 运算符
检查某个键名是否存在的运算符in，适用于对象，也适用于数组。如果数组的某个位置是空位，in运算符返回false。
### for...in 循环和数组的遍历
for...in不仅会遍历数组所有的数字键，还会遍历非数字键。所以，不推荐使用for...in遍历数组。
数组的遍历可以考虑使用for循环或while循环。数组的forEach方法，也可以用来遍历数组，详见《标准库》的 Array 对象一章。
```
var a = [1, 2, 3];

// for循环
for(var i = 0; i < a.length; i++) {
  console.log(a[i]);
}

// while循环
var i = 0;
while (i < a.length) {
  console.log(a[i]);
  i++;
}

var l = a.length;
while (l--) {
  console.log(a[l]);
}
```
### 数组的空位
如果最后一个元素后面有逗号，并不会产生空位。也就是说，有没有这个逗号，结果都是一样的。
```
var a = [1, 2, 3,];

a.length // 3
a // [1, 2, 3]
```
使用delete命令删除一个数组成员，会形成空位，并且不会影响length属性。
length属性不过滤空位。所以，使用length属性进行数组遍历，一定要非常小心。
数组的某个位置是空位，与某个位置是undefined，是不一样的。如果是空位，使用数组的forEach方法、for...in结构、以及Object.keys方法进行遍历，空位都会被跳过。如果某个位置是undefined，遍历的时候就不会被跳过。这就是说，空位就是数组没有这个元素，所以不会被遍历到，而undefined则表示数组有这个元素，值是undefined，所以遍历不会跳过。
```
var a = [, , ,];

a.forEach(function (x, i) {
  console.log(i + '. ' + x);
})
// 不产生任何输出

for (var i in a) {
  console.log(i);
}
// 不产生任何输出

Object.keys(a)
// []


var b = [undefined, undefined, undefined];

b.forEach(function (x, i) {
  console.log(i + '. ' + x);
});
// 0. undefined
// 1. undefined
// 2. undefined

for (var i in b) {
  console.log(i);
}
// 0
// 1
// 2

Object.keys(b)
// ['0', '1', '2']
```
### 类似数组的对象
如果一个对象的所有键名都是正整数或零，并且有length属性，那么这个对象就很像数组，语法上称为“类似数组的对象”（array-like object）。
“类似数组的对象”的根本特征，就是具有length属性。只要有length属性，就可以认为这个对象类似于数组。但是有一个问题，这种length属性不是动态值，不会随着成员的变化而变化。
典型的“类似数组的对象”是函数的arguments对象，以及大多数 DOM 元素集，还有字符串。
数组的slice方法可以将“类似数组的对象”变成真正的数组。
```
var arr = Array.prototype.slice.call(arrayLike);
```
除了转为真正的数组，“类似数组的对象”还有一个办法可以使用数组的方法，就是通过call()把数组的方法放到对象上面。
```
function print(value, index) {
  console.log(index + ' : ' + value);
}

Array.prototype.forEach.call(arrayLike, print); 
// arrayLike代表一个类似数组的对象，本来是不可以使用数组的forEach()方法的，但是通过call()，可以把forEach()嫁接到arrayLike上面调用。
//注意，这种方法比直接使用数组原生的forEach要慢，所以最好还是先将“类似数组的对象”转为真正的数组，然后再直接调用数组的forEach方法。
```
# 运算符
## 算术运算符
### 加法运算符
#### 基本规则
JavaScript 允许非数值的相加。
```
true + true // 2
1 + true // 2
```
如果一个运算子是字符串，另一个运算子是非字符串，这时非字符串会转成字符串，再连接在一起。
```
1 + 'a' // "1a"
false + 'a' // "falsea"

'3' + 4 + 5 // "345"
3 + 4 + '5' // "75"
```
除了加法运算符，其他算术运算符（比如减法、除法和乘法）都不会发生重载。它们的规则是：所有运算子一律转为数值，再进行相应的数学运算。
#### 对象的相加
如果运算子是对象，必须先转成原始类型的值，然后再相加。详细解释参见《数据类型的转换》一章。
```
var obj = { p: 1 };
obj + 2 // "[object Object]2"
```
对象转成原始类型的值，规则如下。
- 首先，自动调用对象的valueOf方法。
```
var obj = { p: 1 };
obj.valueOf() // { p: 1 }
```
- 一般来说，对象的valueOf方法总是返回对象自身，这时再自动调用对象的toString方法，将其转为字符串。如果是原始类型的值不再调用toString方法。
```
var obj = { p: 1 };
obj.valueOf().toString() // "[object Object]"
```
知道了这个规则以后，就可以自己定义valueOf方法或toString方法，得到想要的结果。
```
var obj = {
  valueOf: function () {
    return 1;
  }
};

obj + 2 // 3
```
这里有一个特例，如果运算子是一个Date对象的实例，那么会优先执行toString方法。
```
var obj = new Date();
obj.valueOf = function () { return 1 };
obj.toString = function () { return 'hello' };

obj + 2 // "hello2"
```
### 余数运算符
运算结果的正负号由第一个运算子的正负号决定。
```
-1 % 2 // -1
1 % -2 // 1

// 为了得到负数的正确余数值，可以先使用绝对值函数。
// 错误的写法
function isOdd(n) {
  return n % 2 === 1;
}
isOdd(-5) // false
isOdd(-4) // false

// 正确的写法
function isOdd(n) {
  return Math.abs(n % 2) === 1;
}
isOdd(-5) // true
isOdd(-4) // false
```
余数运算符还可以用于浮点数的运算。但是，由于浮点数不是精确的值，无法得到完全准确的结果。
### 自增和自减运算符
运算之后，变量的值发生变化，这种效应叫做运算的副作用（side effect）。自增和自减运算符是仅有的两个具有副作用的运算符，其他运算符都不会改变变量的值。
自增和自减运算符有一个需要注意的地方，就是放在变量之后，会先返回变量操作前的值，再进行自增/自减操作；放在变量之前，会先进行自增/自减操作，再返回变量操作后的值。
```
var x = 1;
var y = 1;

x++ // 1
++y // 2
```
### 数值运算符，负数值运算符
数值运算符（+）同样使用加号，但它是一元运算符（只需要一个操作数），而加法运算符是二元运算符（需要两个操作数）。
数值运算符的作用在于可以将任何值转为数值（与Number函数的作用相同）。具体的类型转换规则，参见《数据类型转换》一章。
```
+true // 1
+[] // 0
+{} // NaN
```
负数值运算符（-），也同样具有将一个值转为数值的功能，只不过得到的值正负相反。连用两个负数值运算符，等同于数值运算符。
数值运算符号和负数值运算符，都会返回一个新的值，而不会改变原始变量的值。
```
var x = 1;
-x // -1
-(-x) // 1  // 圆括号不可少，否则会变成自减运算符。
```
### 指数运算符
指数运算符（**）完成指数运算，前一个运算子是底数，后一个运算子是指数。
指数运算符是右结合，而不是左结合。即多个指数运算符连用时，先进行最右边的计算。
```
2 ** 4 // 16

// 相当于 2 ** (3 ** 2)
2 ** 3 ** 2
// 512
```
## 比较运算符
### 非相等运算符：非字符串的比较
#### 原始类型值
如果两个运算子都是原始类型的值，则是先转成数值再比较。这里需要注意与NaN的比较。任何值（包括NaN本身）与NaN使用非相等运算符进行比较，返回的都是false。
```
5 > '4' // true
// 等同于 5 > Number('4')
// 即 5 > 4

true > false // true
// 等同于 Number(true) > Number(false)
// 即 1 > 0

2 > true // true
// 等同于 2 > Number(true)
// 即 2 > 1

1 > NaN // false
1 <= NaN // false
'1' > NaN // false
'1' <= NaN // false
NaN > NaN // false
NaN <= NaN // false
```
#### 对象
如果运算子是对象，会转为原始类型的值，再进行比较。
```
var x = [2];
x > '11' // true
// 等同于 [2].valueOf().toString() > '11'
// 即 '2' > '11'

x.valueOf = function () { return '1' };
x > '11' // false
// 等同于 (function () { return '1' })() > '11'
// 即 '1' > '11'

[2] > [1] // true
// 等同于 [2].valueOf().toString() > [1].valueOf().toString()
// 即 '2' > '1'

[2] > [11] // true
// 等同于 [2].valueOf().toString() > [11].valueOf().toString()
// 即 '2' > '11'

({ x: 2 }) >= ({ x: 1 }) // true
// 等同于 ({ x: 2 }).valueOf().toString() >= ({ x: 1 }).valueOf().toString()
// 即 '[object Object]' >= '[object Object]'
```
### 严格相等运算符
JavaScript 提供两种相等运算符：==和===。

简单说，它们的区别是相等运算符（==）比较两个值是否相等，严格相等运算符（===）比较它们是否为“同一个值”。如果两个值不是同一类型，严格相等运算符（===）直接返回false，而相等运算符（==）会将它们转换成同一个类型，再用严格相等运算符进行比较。
#### 同一类的原始类型值
```
1 === 0x1 // true
NaN === NaN  // false
+0 === -0 // true
```
#### 复合类型值
两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个地址。
#### undefined 和 null
undefined和null与自身严格相等。由于变量声明后默认值是undefined，因此两个只声明未赋值的变量是相等的。
### 严格不相等运算符
严格相等运算符有一个对应的“严格不相等运算符”（!==），它的算法就是先求严格相等运算符的结果，然后返回相反值。
### 相等运算符
#### 原始类型值
原始类型的值会转换成数值再进行比较。
```
1 == true // true
// 等同于 1 === Number(true)

0 == false // true
// 等同于 0 === Number(false)

2 == true // false
// 等同于 2 === Number(true)

2 == false // false
// 等同于 2 === Number(false)

'true' == true // false
// 等同于 Number('true') === Number(true)
// 等同于 NaN === 1

'' == 0 // true
// 等同于 Number('') === 0
// 等同于 0 === 0

'' == false  // true
// 等同于 Number('') === Number(false)
// 等同于 0 === 0

'1' == true  // true
// 等同于 Number('1') === Number(true)
// 等同于 1 === 1

'\n  123  \t' == 123 // true
// 因为字符串转为数字时，省略前置和后置的空格
```
#### 对象与原始类型值比较
对象（这里指广义的对象，包括数组和函数）与原始类型的值比较时，对象转换成原始类型的值，再进行比较。
```
const obj = {
  valueOf: function () {
    console.log('执行 valueOf()');
    return obj;
  },
  toString: function () {
    console.log('执行 toString()');
    return 'foo';
  }
};

obj == 'foo'
// 执行 valueOf()
// 执行 toString()
// true
```
#### undefined 和 null
undefined和null只有与自身比较，或者互相比较时，才会返回true；与其他类型的值比较时，结果都为false。
#### 相等运算符的缺点
相等运算符隐藏的类型转换，会带来一些违反直觉的结果。因此建议不要使用相等运算符（==），最好只使用严格相等运算符（===）。
```
0 == ''             // true
0 == '0'            // true

2 == true           // false
2 == false          // false

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
```
### 不相等运算符
相等运算符有一个对应的“不相等运算符”（!=），它的算法就是先求相等运算符的结果，然后返回相反值。
## 布尔运算符
### 取反运算符（!）
对于非布尔值，取反运算符会将其转为布尔值。可以这样记忆，以下六个值取反后为true，其他值都为false。
```
!undefined // true
!null // true
!0 // true
!NaN // true
!"" // true // 空字符串 

!54 // false
!'hello' // false
![] // false
!{} // false

!!x
// 等同于
Boolean(x)
```
### 且运算符（&&）
它的运算规则是：如果第一个运算子的布尔值为false，则直接返回第一个运算子的值，且不再对第二个运算子求值；如果第一个运算子的布尔值为true，则反复该过程直到最后一个运算，返回最后一个运算子的值（注意是值，不是布尔值）；
```
't' && '' // ""
't' && 'f' // "f"
't' && (1 + 2) // 3
'' && 'f' // ""
'' && '' // ""
true && 'foo' && '' && 4 && 'foo' && true // ''
1 && 2 && 3 // 3

var x = 1;
(1 - 1) && ( x += 1) // 0
x // 1
```
有些程序员喜欢用它取代if结构，比如下面是一段if结构的代码，就可以用且运算符改写。
```
if (i) {
  doSomething();
}

// 等价于

i && doSomething();
```
### 或运算符（||）
或运算符常用于为一个变量设置默认值。
```
text = text || '';
```
### 三元条件运算符（?:）
在需要返回值的场合，只能使用三元条件表达式，而不能使用if..else。
## 二进制位运算符
### 概述
- 二进制或运算符（or）：符号为|，表示若两个二进制位都为0，则结果为0，否则为1。
- 二进制与运算符（and）：符号为&，表示若两个二进制位都为1，则结果为1，否则为0。
- 二进制否运算符（not）：符号为~，表示对一个二进制位取反。
- 异或运算符（xor）：符号为^，表示若两个二进制位不相同，则结果为1，否则为0。
- 左移运算符（left shift）：符号为<<，详见下文解释。
- 右移运算符（right shift）：符号为>>，详见下文解释。
- 头部补零的右移运算符（zero filled right shift）：符号为>>>，详见下文解释。

位运算符只对整数起作用，如果一个运算子不是整数，会自动转为整数后再执行。另外，虽然在 JavaScript 内部，数值都是以64位浮点数的形式储存，但是做位运算的时候，是以32位带符号的整数进行运算的，并且返回值也是一个32位带符号的整数。
利用这个特性，可以写出一个函数，将任意数值转为32位整数。对于一般的整数，返回值不会有任何变化。对于大于或等于2的32次方的整数，大于32位的数位都会被舍去。
```
function toInt32(x) {
  return x | 0;
  // 或者 return x ^ 0;
  // 或者 return x << 0;
}
```
### 二进制否运算符
JavaScript 内部采用补码形式表示负数，即需要将这个数减去1得反码，再取一次反（包括符号位），得原码。或者除符号位取反后加1（基于补码定义）
对一个整数连续两次二进制否运算，得到它自身。
使用二进制否运算取整，是所有取整方法中最快的一种。
对于其他类型的值，二进制否运算也是先用Number转为数值，然后再进行处理。
### 异或运算符
“异或运算”有一个特殊运用，连续对两个数a和b进行三次异或运算，a^=b; b^=a; a^=b;，可以互换它们的值。这意味着，使用“异或运算”可以在不引入临时变量的前提下，互换两个变量的值。这也是互换两个变量的值的最快方法。
```
var a = 10;
var b = 99;

a ^= b, b ^= a, a ^= b;

a // 99
b // 10
```
### 头部补零的右移运算符
一个数的二进制形式向右移动时，头部一律补零，而不考虑符号位。所以，该运算总是得到正值。这个运算实际上将一个值转为32位无符号整数。查看一个负整数在计算机内部的储存形式，最快的方法就是使用这个运算符。
```
-1 >>> 0 // 4294967295
```
### 开关作用
```
// 假定某个对象有四个开关，每个开关都是一个变量。那么，可以设置一个四位的二进制数，它的每个位对应一个开关。
var FLAG_A = 1; // 0001
var FLAG_B = 2; // 0010
var FLAG_C = 4; // 0100
var FLAG_D = 8; // 1000

var flags = 5; // 二进制的0101
if (flags & FLAG_C) {
  // 检查当前设置是否打开了指定开关。
}

// 现在假设需要打开A、B、D三个开关，我们可以构造一个掩码变量。
var mask = FLAG_A | FLAG_B | FLAG_D;
// 0001 | 0010 | 1000 => 1011
// 有了掩码，二进制或运算可以确保打开指定的开关。
flags = flags | mask;

// 全部关闭与开关设置不一样的项
flags = flags & mask;

// 反转开关
flags = flags ^ mask;
// 或者 flags = ~flags;
```
## 其他运算符，运算顺序
### void 运算符
void运算符的作用是执行一个表达式，然后不返回任何值，或者说返回undefined。
void运算符的优先性很高，如果不使用括号，容易造成错误的结果。比如，void 4 + 7实际上等同于(void 4) + 7。
这个运算符的主要用途是浏览器的书签工具（Bookmarklet），以及在超级链接中插入代码防止网页跳转。
```
<script>
function f() {
  console.log('Hello World');
}
</script>
<a href="http://example.com" onclick="f(); return false;">点击</a>
// 可以替换成
<a href="javascript: void(f())">文字</a>
// 常见使用方法
<a href="javascript: void(document.form.submit())">
  提交
</a>
```
### 逗号运算符
逗号运算符用于对两个表达式求值，并返回后一个表达式的值。
逗号运算符的一个用途是，在返回一个值之前，进行一些辅助操作。
```
var value = (console.log('Hi!'), true);
// Hi!

value // truevar value = (console.log('Hi!'), true);
```
### 运算顺序
#### 圆括号的作用
函数放在圆括号中，会返回函数本身。如果圆括号紧跟在函数的后面，就表示调用函数。圆括号之中，只能放置表达式，如果将语句放在圆括号之中，就会报错。
```
(expression)
// 等同于
expression

function f() {
  return 1;
}

(f) // function f(){return 1;}
f() // 1

(var a = 1)
// SyntaxError: Unexpected token var
```
#### 左结合与右结合
对于优先级别相同的运算符，同时出现的时候，就会有计算顺序的问题。
JavaScript 语言的大多数运算符是“左结合”, 少数运算符是“右结合”，其中最主要的是赋值运算符（=）,三元条件运算符（?:）和指数运算符（**）也是右结合。
```
w = x = y = z;
q = a ? b : c ? d : e ? f : g;
2 ** 3 ** 2
// 上面代码的解释方式如下
w = (x = (y = z));
q = a ? b : (c ? d : (e ? f : g));
2 ** (3 ** 2) // 512
```
# 语法专题
## 数据类型的转换
### 概述
JavaScript 是一种动态类型语言，变量没有类型限制，可以随时赋予任意值。这意味着，有些代码中变量的类型没法在编译阶段就知道，必须等到运行时才能知道。虽然变量的数据类型是不确定的，但是各种运算符对数据类型是有要求的。如果运算符发现，运算子的类型与预期不符，就会自动转换类型。
### 强制转换
#### Number()
使用Number函数，可以将任意类型的值转化成数值。
##### 原始类型值
```
// 数值：转换后还是原来的值
Number(324) // 324

// 字符串：如果可以被解析为数值，则转换为相应的数值
Number('324') // 324

// 字符串：如果不可以被解析为数值，返回 NaN。Number函数将字符串转为数值，要比parseInt函数严格很多。基本上，只要有一个字符无法转成数值，整个字符串就会被转为NaN。parseInt逐个解析字符，而Number函数整体转换字符串的类型。parseInt和Number函数都会自动过滤一个字符串前导和后缀的空格。
Number('324abc') // NaN

parseInt('\t\v\r12.34\n') // 12
Number('\t\v\r12.34\n') // 12.34

// 空字符串转为0
Number('') // 0

// 布尔值：true 转成 1，false 转成 0
Number(true) // 1
Number(false) // 0

// undefined：转成 NaN
Number(undefined) // NaN

// null：转成0
Number(null) // 0
```
##### 对象
Number方法的参数是对象时，将返回NaN，除非是包含单个数值的数组。
```
Number({a: 1}) // NaN
Number([1, 2, 3]) // NaN
Number([5]) // 5
```
Number背后的转换规则比较复杂。
- 第一步，调用对象自身的valueOf方法。如果返回原始类型的值，则直接对该值使用Number函数，不再进行后续步骤。
- 第二步，如果valueOf方法返回的还是对象，则改为调用对象自身的toString方法。如果toString方法返回原始类型的值，则对该值使用Number函数，不再进行后续步骤。
- 第三步，如果toString方法返回的是对象，就报错。

#### String()
##### 原始类型值
```
String(123) // "123"
String('abc') // "abc"
String(true) // "true"
String(undefined) // "undefined"
String(null) // "null"
```
##### 对象
String方法的参数如果是对象，返回一个类型字符串；如果是数组，返回该数组的字符串形式。
```
String({a: 1}) // "[object Object]"
String([1, 2, 3]) // "1,2,3"
```
String方法背后的转换规则，与Number方法基本相同，只是互换了valueOf方法和toString方法的执行顺序。
#### Boolean()
Boolean()函数可以将任意类型的值转为布尔值。
除了以下六个情况的转换结果为false，其他的值全部为true。因为 JavaScript 语言设计的时候，出于性能的考虑，如果对象需要计算才能得到布尔值，对于obj1 && obj2这样的场景，可能会需要较多的计算。为了保证性能，就统一规定，对象的布尔值为true。
```
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean(NaN) // false
Boolean('') // false
Boolean(false) // false

Boolean({}) // true
Boolean([]) // true
Boolean(new Boolean(false)) // true
```
### 自动转换
自动转换，它是以强制转换为基础的。自动转换的规则是这样的：预期什么类型的值，就调用该类型的转换函数。如果该位置既可以是字符串，也可能是数值，那么默认转为数值。
遇到以下三种情况时，JavaScript 会自动转换数据类型
- 不同类型的数据互相运算
- 对非布尔值类型的数据求布尔值
- 对非数值类型的值使用一元运算符（即+和-）

#### 自动转换为布尔值
有时也用于将一个表达式转为布尔值。它们内部调用的也是Boolean()函数。
```
// 写法一
expression ? true : false

// 写法二
!! expression
```
#### 自动转换为字符串
先将复合类型的值转为原始类型的值，再将原始类型的值转为字符串。当一个值为字符串，另一个值为非字符串，则后者转为字符串。
#### 自动转换为数值
除了加法运算符（+）有可能把运算子转为字符串，其他运算符都会把运算子自动转成数值。
null转为数值时为0，而undefined转为数值时为NaN。一元运算符也会把运算子转成数值。
```
+'abc' // NaN
-'abc' // NaN
+true // 1
-false // 0
```
## 错误处理机制
### Error实例对象
JavaScript 原生提供Error构造函数，所有抛出的错误都是这个构造函数的实例。
JavaScript 语言标准只提到，Error实例对象必须有message属性，表示出错时的提示信息，没有提到其他属性。大多数 JavaScript 引擎，对Error实例还提供name(错误名称)和stack属性(错误的堆栈)，分别表示错误的名称和错误的堆栈，但它们是非标准的，不是每种实现都有。
### 原生错误类型
Error实例对象是最一般的错误类型，在它的基础上，JavaScript 还定义了其他6种错误对象。也就是说，存在Error的6个派生对象。
#### SyntaxError 对象
SyntaxError对象是解析代码时发生的语法错误。
#### ReferenceError 对象
ReferenceError对象是引用一个不存在的变量时发生的错误。另一种触发场景是，将一个值分配给无法分配的对象，比如对函数的运行结果赋值。
#### RangeError 对象
RangeError对象是一个值超出有效范围时发生的错误。主要有几种情况，一是数组长度为负数，二是Number对象的方法参数超出范围，以及函数堆栈超过最大值。
#### TypeError 对象
TypeError对象是变量或参数不是预期类型时发生的错误。比如，调用对象不存在的方法。
#### URIError 对象
URIError对象是 URI 相关函数的参数不正确时抛出的错误，主要涉及encodeURI()、decodeURI()、encodeURIComponent()、decodeURIComponent()、escape()和unescape()这六个函数。
#### EvalError 对象
eval函数没有被正确执行时，会抛出EvalError错误。该错误类型已经不再使用了，只是为了保证与以前代码兼容，才继续保留。
## 编程风格
### 变量声明
JavaScript 会自动将变量声明“提升”（hoist）到代码块（block）的头部。JavaScript 会自动将变量声明“提升”（hoist）到代码块（block）的头部。所有函数都应该在使用之前定义。函数内部的变量声明，都应该放在函数的头部。
### 相等和严格相等
相等运算符会自动转换变量类型，造成很多意想不到的情况。
### 自增和自减运算符
建议自增（++）和自减（--）运算符尽量使用+=和-=代替。
### switch...case 结构
```
function doAction(action) {
  switch (action) {
    case 'hack':
      return 'hack';
    case 'slash':
      return 'slash';
    case 'run':
      return 'run';
    default:
      throw new Error('Invalid action.');
  }
}

// switch代码建议改写成对象结构
function doAction(action) {
  var actions = {
    'hack': function () {
      return 'hack';
    },
    'slash': function () {
      return 'slash';
    },
    'run': function () {
      return 'run';
    }
  };

  if (typeof actions[action] !== 'function') {
    throw new Error('Invalid action.');
  }

  return actions[action]();
}
```
## console 对象与控制台
### console 对象
console对象是 JavaScript 的原生对象，它有点像 Unix 系统的标准输出stdout和标准错误stderr，可以输出各种信息到控制台，并且还提供了很多有用的辅助方法。
### console 对象的静态方法
console对象的所有方法，都可以被覆盖。因此，可以按照自己的需要，定义console.log方法。
```
['log', 'info', 'warn', 'error'].forEach(function(method) {
  console[method] = console[method].bind(
    console,
    new Date().toISOString()
  );
});

console.log("出错了！");
// 2014-05-18T09:00.000Z 出错了！
```
#### console.log()，console.info()，console.debug()
##### console.log()
console.log方法支持以下占位符，不同类型的数据必须使用对应的占位符。
- %s 字符串
- %d 整数
- %i 整数
- %f 浮点数
- %o 对象的链接
- %c CSS 格式字符串

```
console.log(' %s + %s ', 1, 1, '= 2')
// 1 + 1  = 2
```
##### console.info()
与console.log方法的别名，用法完全一样。只不过console.info方法会在输出信息的前面，加上一个蓝色图标。
##### console.debug()
默认情况下，console.debug输出的信息不会显示，只有在打开显示级别在verbose的情况下，才会显示。
#### console.warn()，console.error()
warn方法输出信息时，在最前面加一个黄色三角，表示警告；error方法输出信息时，在最前面加一个红色的叉，表示出错。同时，还会高亮显示输出文字和错误发生的堆栈。
可以这样理解，log方法是写入标准输出（stdout），warn方法和error方法是写入标准错误（stderr）。
#### console.table()
对于某些复合类型的数据，console.table方法可以将其转为表格显示。
#### console.count()
count方法用于计数，输出它被调用了多少次。该方法可以接受一个字符串作为参数，作为标签，对执行次数进行分类。
```
function greet(user) {
  console.count(user);
  return "hi " + user;
}

greet('bob')
// bob: 1
// "hi bob"

greet('alice')
// alice: 1
// "hi alice"

greet('bob')
// bob: 2
// "hi bob"
```
#### console.dir()，console.dirxml()
##### console.dir()
dir方法用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示。
```
// 该方法对于输出 DOM 对象非常有用，因为会显示 DOM 对象的所有属性。
console.dir(document.body)
// Node 环境之中，还可以指定以代码高亮的形式输出。
console.dir(obj, {colors: true})
```
##### console.dirxml()
dirxml方法主要用于以目录树的形式，显示 DOM 节点。如果参数不是 DOM 节点，而是普通的 JavaScript 对象，console.dirxml等同于console.dir。
#### console.assert()
console.assert方法主要用于程序运行过程中，进行条件判断，如果不满足条件，就显示一个错误，但不会中断程序执行。这样就相当于提示用户，内部状态不正确。
#### console.time()，console.timeEnd()
这两个方法用于计时，可以算出一个操作所花费的准确时间。
```
console.time('Array initialize');

var array= new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
  array[i] = new Object();
};

console.timeEnd('Array initialize');
// Array initialize: 1914.481ms
```
#### console.group()，console.groupEnd()，console.groupCollapsed()
console.group和console.groupEnd这两个方法用于将显示的信息分组。它只在输出大量信息时有用，分在一组的信息，可以用鼠标折叠/展开。console.groupCollapsed方法与console.group方法很类似，唯一的区别是该组的内容，在第一次显示时是收起的（collapsed），而不是展开的。
```
console.group('一级分组');
console.log('一级分组的内容');

console.group('二级分组');
console.log('二级分组的内容');

console.groupEnd(); // 二级分组结束
console.groupEnd(); // 一级分组结束


// groupCollapsed(
console.groupCollapsed('Fetching Data');

console.log('Request Sent');
console.error('Error: Server not responding (500)');

console.groupEnd();
```
#### console.trace()，console.clear()
##### console.trace()
console.trace方法显示当前执行的代码在堆栈中的调用路径。
##### console.clear()
console.clear方法用于清除当前控制台的所有输出，将光标回置到第一行。如果用户选中了控制台的“Preserve log”选项，console.clear方法将不起作用。
### 控制台命令行 API
#### \$_
\$_属性返回上一个表达式的值。
#### \$0 - \$4
控制台保存了最近5个在 Elements 面板选中的 DOM 元素，\$0代表倒数第一个（最近一个），\$1代表倒数第二个，以此类推直到\$4。
#### \$(selector)
\$(selector)返回第一个匹配的元素，等同于document.querySelector()。注意，如果页面脚本对$有定义，则会覆盖原始的定义。比如，页面里面有 jQuery，控制台执行\$(selector)就会采用 jQuery 的实现，返回一个数组。
#### \$\$(selector)
\$\$(selector)返回选中的 DOM 对象，等同于document.querySelectorAll。
#### \$x(path)
\$x(path)方法返回一个数组，包含匹配特定 XPath 表达式的所有 DOM 元素。
#### inspect(object)
inspect(object)方法打开相关面板，并选中相应的元素，显示它的细节。
#### getEventListeners(object)
getEventListeners(object)方法返回一个对象，该对象的成员为object登记了回调函数的各种事件（比如click或keydown），每个事件对应一个数组，数组的成员为该事件的回调函数。
#### keys(object)，values(object)
keys(object)方法返回一个数组，包含object的所有键名。
values(object)方法返回一个数组，包含object的所有键值。
#### monitorEvents(object[, events]) ，unmonitorEvents(object[, events])
monitorEvents(object[, events])方法监听特定对象上发生的特定事件。事件发生时，会返回一个Event对象，包含该事件的相关信息。unmonitorEvents方法用于停止监听。
```
monitorEvents(window, "resize");
monitorEvents(window, ["resize", "scroll"])

monitorEvents($0, 'mouse');
unmonitorEvents($0, 'mousemove');
```
monitorEvents允许监听同一大类的事件。所有事件可以分成四个大类。
- mouse
"mousedown", "mouseup", "click", "dblclick", "mousemove", "mouseover", "mouseout", "mousewheel"
- key
"keydown", "keyup", "keypress", "textInput"
- touch
"touchstart", "touchmove", "touchend", "touchcancel"
- control
"resize", "scroll", "zoom", "focus", "blur", "select", "change", "submit", "reset"

#### 其他方法
- clear()：清除控制台的历史。
- copy(object)：复制特定 DOM 元素到剪贴板。
- dir(object)：显示特定对象的所有属性，是console.dir方法的别名。
- dirxml(object)：显示特定对象的 XML 形式，是console.dirxml方法的别名。

### debugger 语句
debugger语句主要用于除错，作用是设置断点。如果有正在运行的除错工具，程序运行到debugger语句时会自动停下。如果没有除错工具，debugger语句不会产生任何结果，JavaScript 引擎自动跳过这一句。
Chrome 浏览器中，当代码运行到debugger语句时，就会暂停运行，自动打开脚本源码界面。
```
for(var i = 0; i < 5; i++){
  console.log(i);
  if (i === 2) debugger;
}
```
上面代码打印出0，1，2以后，就会暂停，自动打开源码界面，等待进一步处理。
# 标准库
## Object 对象
### 概述
JavaScript 原生提供Object对象（注意起首的O是大写），本章介绍该对象原生的各种方法。
JavaScript 的所有其他对象都继承自Object对象，即那些对象都是Object的实例。
Object对象的原生方法分成两类：Object本身的方法与Object的实例方法。
#### Object对象本身的方法
所谓“本身的方法”就是直接定义在Object对象的方法。
```
Object.print = function (o) { console.log(o) };
```
#### Object的实例方法
所谓实例方法就是定义在Object原型对象Object.prototype上的方法。它可以被Object实例直接使用。
```
Object.prototype.print = function () {
  console.log(this);
};

var obj = new Object();
obj.print() // Object
```
关于原型对象object.prototype的详细解释，参见《面向对象编程》章节。这里只要知道，凡是定义在Object.prototype对象上面的属性和方法，将被所有实例对象共享就可以了。
### Object()
Object本身是一个函数，可以当作工具方法使用，将任意值转为对象。这个方法常用于保证某个值一定是对象。
如果参数为空（或者为undefined和null），Object()返回一个空对象。
如果参数是原始类型的值，Object方法将其转为对应的包装对象的实例。
如果Object方法的参数是一个对象，它总是返回该对象，即不用转换。
利用这一点，可以写一个判断变量是否为对象的函数。
```
function isObject(value) {
  return value === Object(value);
}

isObject([]) // true
isObject(true) // false
```
### Object 构造函数
Object不仅可以当作工具函数使用，还可以当作构造函数使用，即前面可以使用new命令。虽然用法相似，但是Object(value)与new Object(value)两者的语义是不同的，Object(value)表示将value转成一个对象，new Object(value)则表示新生成一个对象，它的值是value。
通过var obj = new Object()的写法生成新对象，与字面量的写法var obj = {}是等价的。或者说，后者只是前者的一种简便写法。
### Object 的静态方法
所谓“静态方法”，是指部署在Object对象自身的方法。
#### Object.keys()，Object.getOwnPropertyNames()
Object.keys方法和Object.getOwnPropertyNames方法都用来遍历对象的属性。
Object.keys方法的参数是一个对象，返回一个数组。该数组的成员都是该对象自身的（而不是继承的）所有属性名。
Object.getOwnPropertyNames方法与Object.keys类似，也是接受一个对象作为参数，返回一个数组，包含了该对象自身的所有属性名。
对于一般的对象来说，Object.keys()和Object.getOwnPropertyNames()返回的结果是一样的。只有涉及不可枚举属性时，才会有不一样的结果。Object.keys方法只返回可枚举的属性（详见《对象属性的描述对象》一章），Object.getOwnPropertyNames方法还返回不可枚举的属性名。
```
var a = ['Hello', 'World'];

Object.keys(a) // ["0", "1"]
Object.getOwnPropertyNames(a) // ["0", "1", "length"]
```
上面代码中，数组的length属性是不可枚举的属性，所以只出现在Object.getOwnPropertyNames方法的返回结果中。
由于 JavaScript 没有提供计算对象属性个数的方法，所以可以用这两个方法代替。
```
var obj = {
  p1: 123,
  p2: 456
};

Object.keys(obj).length // 2
Object.getOwnPropertyNames(obj).length // 2
```
一般情况下，几乎总是使用Object.keys方法，遍历对象的属性。
#### 其他方法
##### 对象属性模型的相关方法
- Object.getOwnPropertyDescriptor()：获取某个属性的描述对象。
- Object.defineProperty()：通过描述对象，定义某个属性。
- Object.defineProperties()：通过描述对象，定义多个属性。

##### 控制对象状态的方法
- Object.preventExtensions()：防止对象扩展。
- Object.isExtensible()：判断对象是否可扩展。
- Object.seal()：禁止对象配置。
- Object.isSealed()：判断一个对象是否可配置。
- Object.freeze()：冻结一个对象。
- Object.isFrozen()：判断一个对象是否被冻结。

##### 原型链相关方法
- Object.create()：该方法可以指定原型对象和属性，返回一个新的对象。
- Object.getPrototypeOf()：获取对象的Prototype对象。

### Object 的实例方法
除了静态方法，还有不少方法定义在Object.prototype对象。它们称为实例方法，所有Object的实例对象都继承了这些方法。
Object实例对象的方法，主要有以下六个。
- Object.prototype.valueOf()：返回当前对象对应的值。
- Object.prototype.toString()：返回当前对象对应的字符串形式。
- Object.prototype.toLocaleString()：返回当前对象对应的本地字符串形式。
- Object.prototype.hasOwnProperty()：判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性。
- Object.prototype.isPrototypeOf()：判断当前对象是否为另一个对象的原型。
- Object.prototype.propertyIsEnumerable()：判断某个属性是否可枚举。

#### Object.prototype.valueOf()
valueOf方法的作用是返回一个对象的“值”，默认情况下返回对象本身。
#### Object.prototype.toString()
数组、字符串、函数、Date 对象调用toString方法，并不会返回[object Object]，因为它们都自定义了toString方法，覆盖原始方法。
#### toString() 的应用：判断数据类型
调用空对象的toString方法，结果返回一个字符串[object Object]，其中第二个Object表示该值的构造函数。这是一个十分有用的判断数据类型的方法。
由于实例对象可能会自定义toString方法，覆盖掉Object.prototype.toString方法，所以为了得到类型字符串，最好直接使用Object.prototype.toString方法。通过函数的call方法，可以在任意值上调用这个方法，帮助我们判断这个值的类型。
```
Object.prototype.toString.call(value)

// 利用这个特性，可以写出一个比typeof运算符更准确的类型判断函数。
var type = function (o){
  var s = Object.prototype.toString.call(o);
  return s.match(/\[object (.*?)\]/)[1].toLowerCase();
};

['Null',
 'Undefined',
 'Object',
 'Array',
 'String',
 'Number',
 'Boolean',
 'Function',
 'RegExp'
].forEach(function (t) {
  type['is' + t] = function (o) {
    return type(o) === t.toLowerCase();
  };
});

type.isObject({}) // true
type.isNumber(NaN) // true
type.isRegExp(/abc/) // true
```
#### Object.prototype.toLocaleString()
这个方法的主要作用是留出一个接口，让各种不同的对象实现自己版本的toLocaleString，用来返回针对某些地域的特定的值。
目前，主要有三个对象自定义了toLocaleString方法。
- Array.prototype.toLocaleString()
- Number.prototype.toLocaleString()
- Date.prototype.toLocaleString()

#### Object.prototype.hasOwnProperty()
Object.prototype.hasOwnProperty方法接受一个字符串作为参数，返回一个布尔值，表示该实例对象自身是否具有该属性。
## 属性描述对象
### 概述
JavaScript 提供了一个内部数据结构，用来描述对象的属性，控制它的行为，比如该属性是否可写、可遍历等等。这个内部数据结构称为“属性描述对象”（attributes object）。每个属性都有自己对应的属性描述对象，保存该属性的一些元信息。
- value
value是该属性的属性值，默认为undefined。
- writable
writable是一个布尔值，表示属性值（value）是否可改变（即是否可写），默认为true。
- enumerable
enumerable是一个布尔值，表示该属性是否可遍历，默认为true。如果设为false，会使得某些操作（比如for...in循环、Object.keys()）跳过该属性。
- configurable
configurable是一个布尔值，表示属性的可配置性，默认为true。如果设为false，将阻止某些操作改写属性描述对象，比如无法删除该属性，也不得改变各种元属性（value属性除外）。也就是说，configurable属性控制了属性描述对象的可写性。
- get
get是一个函数，表示该属性的取值函数（getter），默认为undefined。
- set
set是一个函数，表示该属性的存值函数（setter），默认为undefined。

### Object.getOwnPropertyDescriptor()















































































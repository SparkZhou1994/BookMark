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






























































































































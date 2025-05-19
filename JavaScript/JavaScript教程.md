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




 




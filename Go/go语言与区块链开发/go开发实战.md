# 初识GO语言
## 第一个Go程序
### 代码分析
要生成Go可执行程序，必须建立一个名字为main的包，并且在该包中包含一个叫main()的函数（该函数是Go可执行程序的执行起点）。
Go语言的main()函数不能带参数，也不能定义返回值。
### 命令行运行程序
```
// 只会编译，不会执行
go build  test.go
// 执行
./test

// 只运行 不生成可执行程序
go run test.go
```

# 基本类型
## 变量
### 变量声明
```
var v1, v2 int
var {
	v1 int
	v2 int
}
```
### 变量初始化
```
var v1 int = 10
var v1 = 10
v2 := 10( 包含声明的初始化)
```
### 匿名变量
下划线是个特殊的变量名，任何赋予它的值都会被丢弃
## 常量
### iota 枚举
在一个const声明语句中，在第一个声明的常量所在的行，iota将会被置为0，然后在每一个有常量声明的行加一。
```
const (
    x = iota // x == 0
    y = iota // y == 1
    z = iota // z == 2
    w  // 这里隐式地说w = iota，因此w == 3。其实上面y和z可同样不用"= iota"
)

const v = iota // 每遇到一个const关键字，iota就会重置，此时v == 0

const (
    h, i, j = iota, iota, iota //h=0,i=0,j=0 iota在同一行值相同
)

const (
    a       = iota //a=0
    b       = "B"
    c       = iota             //c=2
    d, e, f = iota, iota, iota //d=3,e=3,f=3
    g       = iota             //g = 4
)

```
## 基础数据类型
### 布尔类型
布尔类型不能接受其他类型的赋值，不支持自动或强制的类型转换
### 字符类型
- byte 代表utf-8单个字节的值
- rune 代表单个unicode字符
可与int强转

### 字符串
反引号括起的字符串为Raw字符串，即字符串在代码中的形式就是打印时的形式，它没有字符转义，换行也将原样输出。
### 复数类型
```
var v1 complex64 // 由2个float32构成的复数类型
v1 = 3.2 + 12i
v2 := 3.2 + 12i        // v2是complex128类型
v3 := complex(3.2, 12) // v3结果同v2

fmt.Println(v1, v2, v3)
//内置函数real(v1)获得该复数的实部
//通过imag(v1)获得该复数的虚部
fmt.Println(real(v1), imag(v1))
```
## fmt包的格式化输出输入
### 格式说明
|格式|说明|
|:--:|:--:|
|%c|字符型|
|%d|十进制|
|%T|输出值的类型|
|%v|使用其String()方法|
### 输入
```
fmt.Scanf("%d", &v);
```
## 类型别名
```
type bigint int64 //int64类型改名为bigint

```
# 运算符
## 其他运算符
|运算符|术语|
|:--:|:---:|
|&|取地址运算符|
|*|取值运算符|

# 流程控制
## 选择结构
### if 语句
```
//支持一个初始化表达式, 初始化字句和条件表达式直接需要用分号分隔
if b := 3; b == 3 {
    fmt.Println("b==3")
}
```
### switch 语句
```
switch s1 := 90; s1 { //初始化语句;条件
	case xx : xx
}

switch { //这里没有写条件
    case s2 >= 90: //这里写判断语句
}

switch s3 := 90; { //只有初始化语句，没有条件
    case s3 >= 90: //这里写判断语句
}
```
## 循环语句
### range
```
for _, c := range s { // 忽略 index
    fmt.Printf("%c\n", c)
}
```
## 跳转语句
### goto
```
goto LABEL //跳转到标签LABEL，从标签处，执行代码

LABEL:
    fmt.Println("it is over")
```

# 函数
## 定义格式
函数名首字母小写即为private，大写即为public
```
func FuncName(/*参数列表*/) (o1 type1, o2 type2/*返回类型*/) {
    //函数体

    return v1, v2 //返回多个值
}
```
## 自定义函数
### 无参无返回
```
func Test() { //无参无返回值函数定义}
```
### 有参无返回
```
func Test02(v1, v2 int) { //方式2, v1, v2都是int类型}

//形如...type格式的类型只能作为函数的参数类型存在，并且必须是最后一个参数
func Test(args ...int) {}
```
### 无参有返回
```
func Test01() (int, string) { //方式1
    return 250, "sb"
}

func Test02() (a int, str string) { //方式2, 给返回值命名
    a = 250
    str = "sb"
    return
}
```
### 有参有返回
```
func MinAndMax(num1 int, num2 int) (min int, max int) {}
```
## 递归函数
```
//通过递归实现1+2+3……+100
func Test02(num int) int {
    if num == 1 {
        return 1
    }

    return num + Test02(num-1) //函数调用本身
}

//通过递归实现1+2+3……+100
func Test03(num int) int {
    if num == 100 {
        return 100
    }

    return num + Test03(num+1) //函数调用本身
}
```
## 函数类型
可以通过type来定义它，它的类型就是所有拥有相同的参数，相同的返回值的一种类型。
```
type FuncType func(int, int) int //声明一个函数类型, func后面没有函数名

//函数中有一个参数类型为函数类型：f FuncType
func Calc(a, b int, f FuncType) (result int) {
    result = f(a, b) //通过调用f()实现任务
    return
}

func Add(a, b int) int {
    return a + b
}

func Minus(a, b int) int {
    return a - b
}

func main() {
    //函数调用，第三个参数为函数名字，此函数的参数，返回值必须和FuncType类型一致
    result := Calc(1, 1, Add)
    fmt.Println(result) //2

    var f FuncType = Minus
    fmt.Println("result = ", f(10, 2)) //result =  8
}
```
## 匿名函数与闭包
它不关心这些捕获了的变量和常量是否已经超出了作用域，所以只有闭包还在使用它，这些变量就还会存在。
```
// 方式1
f1 := func() { //匿名函数，无参无返回值
    //引用到函数外的变量
    fmt.Printf("方式1：i = %d, str = %s\n", i, str)
}

f1() //函数调用

// 方式2
func() { //匿名函数，无参无返回值
    //引用到函数外的变量
    fmt.Printf("方式1：i = %d, str = %s\n", i, str)
} ()

```
### 闭包捕获外部变量特点
变量的生命周期不由它的作用域决定：squares返回后，变量x仍然隐式的存在于f中。
```
// squares返回一个匿名函数，func() int
// 该匿名函数每次被调用时都会返回下一个数的平方。
func squares() func() int {
    var x int
    return func() int {//匿名函数
        x++ //捕获外部变量
        return x * x
    }
}

func main() {
    f := squares()
    fmt.Println(f()) // "1"
    fmt.Println(f()) // "4"
    fmt.Println(f()) // "9"
    fmt.Println(f()) // "16"
}
```
## 延迟调用 defer
关键字 defer ⽤于延迟一个函数或者方法（或者当前所创建的匿名函数）的执行。注意，defer语句只能出现在函数或方法的内部。
defer语句经常被用于处理成对的操作，如打开、关闭、连接、断开连接、加锁、释放锁。
通过defer机制，不论函数逻辑多复杂，都能保证在任何执行路径下，资源被释放。释放资源的defer应该直接跟在请求资源的语句后。
### 多个defer执行顺序
如果一个函数中有多个defer语句，它们会以LIFO（后进先出）的顺序执行。哪怕函数或某个延迟调用发生错误，这些调用依旧会被执⾏。
### defer和匿名函数结合使用
```
func main() {
    a, b := 10, 20
    defer func(x int) { // a以值传递方式传给x
        fmt.Println("defer:", x, b) // b 闭包引用
    }(a)

    a += 10
    b += 100

    fmt.Printf("a = %d, b = %d\n", a, b)

    /*
        运行结果：
        a = 20, b = 120
        defer: 10 120
    */
}
```





























































































































































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
## 获取命令行参数
```
import {
    "fmt"
    "os"
}

func main() {
    args := os.Args
    if args == nil || len(args) < 2 {
        fmt.Println("err: xxx ip port")
        return
    }
    fmt.Println("args is %s\n", args[1])
}
```

# 工程管理
## 工作区
### 工作区介绍
- src 源码 
- pkg go install 后的 .a 归档文件 需要配置GOBIN参数
- bin go install 后的 可执行文件

目录src用于包含所有的源代码
若环境变量GOPATH中包含多个工作区的目录路径，像这样执行go install命令就会失效，此时必须设置环境变量GOBIN
### GOPATH 设置
为了能够构建这个工程，需要先把所需工程的根目录加入到环境变量GOPATH中。否则，即使处于同一工作目录(工作区)，代码之间也无法通过绝对代码包路径完成调用。
在实际开发环境中，工作目录往往有多个。这些工作目录的目录路径都需要添加至GOPATH。当有多个目录时，请注意分隔符，多个目录的时候Windows是分号，Linux系统是冒号，当有多个GOPATH时，默认会将go get的内容放在第一个目录下。
## 包
go env 查看GOPATH
### 自定义包
同一个目录，包名必须一样
同一个目录调用其他文件的函数，无需引包

不同目录，包名不一样
调用不同包的函数 格式 包名.函数名
函数名注意大小写
### main包
当编译器发现某个包的名字为 main 时，它一定也会发现名为 main()的函数，否则不会创建可执行文件。 main()函数是程序的入口，所以，如果没有这个函数，程序就没有办法开始执行。程序编译时，会使用声明 main 包的代码所在的目录的目录名作为二进制可执行文件的文件名。
### init函数
有时一个包会被多个包同时导入，那么它只会被导入一次（例如很多包可能都会用到fmt包，但它只会被导入一次，因为没有必要导入多次）。
### 导入包
标准库中的包会在安装 Go 的位置找到。 Go 开发者创建的包会在 GOPATH 环境变量指定的目录里查找。GOPATH 指定的这些目录就是开发者的个人工作空间。
#### 点操作
```
import (
    //这个点操作的含义是这个包导入之后在你调用这个包的函数时，可以省略前缀的包名
    . "fmt"
)

func main() {
    Println("hello go")
}

```
#### 别名操作
```
import (
    io "fmt" //fmt改为为io
)

func main() {
    io.Println("hello go") //通过io别名调用
}
```
#### 下划线操作
用户可能需要导入一个包，但是不需要引用这个包的标识符。在这种情况，可以使用空白标识符_来重命名这个导入
其实是引入该包，而不直接使用包里面的函数，而是调用了该包里面的init函数。

# 复合类型
## 指针
默认值 nil，没有 NULL 常量
不支持指针运算，不支持 "->" 运算符，直接⽤ "." 访问目标成员
### new 函数
我们只需使用new()函数，无需担心其内存的生命周期或怎样将其删除
```
func main() {
    var p1 *int
    p1 = new(int)              //p1为*int 类型, 指向匿名的int变量
    fmt.Println("*p1 = ", *p1) //*p1 =  0

    p2 := new(int) //p2为*int 类型, 指向匿名的int变量
    *p2 = 111
    fmt.Println("*p2 = ", *p2) //*p1 =  111
}
```
## 数组
### 概述
数组⻓度必须是常量，且是类型的组成部分。 [2]int 和 [3]int 是不同类型。
### 操作
内置函数 len(长度) 和 cap(容量) 都返回数组⻓度 (元素数量)
相同类型的数组之间可以使用 == 或 != 进行比较，但不可以使用 < 或 >，也可以相互赋值
```
// 初始化
a := [3]int{1, 2}           // 未初始化元素值为 0
b := [...]int{1, 2, 3}      // 通过初始化值确定数组长度
c := [5]int{2: 100, 4: 200} // 通过索引号初始化元素，未初始化元素值为 0
fmt.Println(a, b, c)        //[1 2 0] [1 2 3] [0 0 100 0 200]

//支持多维数组
d := [4][2]int{{10, 11}, {20, 21}, {30, 31}, {40, 41}}
e := [...][2]int{{10, 11}, {20, 21}, {30, 31}, {40, 41}} //第二维不能写"..."
f := [4][2]int{1: {20, 21}, 3: {40, 41}}
g := [4][2]int{1: {0: 20}, 3: {1: 41}}
fmt.Println(d, e, f, g)

```
### 在函数间传递数组
在函数之间传递变量时，总是以值的方式传递的。如果是数组会复制数组，开销大。故用指针传递。
## 切片(slice)
### 概述
切片并不是数组或数组指针，它通过内部指针和相关属性引⽤数组⽚段，以实现变⻓⽅案。
slice并不是真正意义上的动态数组，而是一个 引用类型 。slice总是指向一个底层array，slice的声明也可以像array一样，只是不需要长度。
### 创建及初始化
声明slice时，方括号内没有任何字符
make只能创建slice、map和channel，并且返回一个有初始值(非零)。
```
var s1 []int //声明切片和声明array一样，只是少了长度，此为空(nil)切片
s2 := []int{}

//make([]T, length, capacity) //capacity省略，则和length的值相同
var s3 []int = make([]int, 0)
s4 := make([]int, 0, 0)

s5 := []int{1, 2, 3} //创建切片并初始化
```
### 操作
#### 截取
s[low:high:max] 从切片s的索引位置low到high处所获得的切片，len=high-low，cap=max-low （low,high] 以1为起始位置
#### 内建函数
##### append
append函数会智能地底层数组的容量增长，一旦超过原底层数组容量，通常以2倍容量重新分配底层数组，并复制原来的数据
##### copy
函数 copy 在两个 slice 间复制数据，复制⻓度以 len 小的为准，两个 slice 可指向同⼀底层数组。
```
data := [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
s1 := data[8:]  //{8, 9}
s2 := data[:5] //{0, 1, 2, 3, 4}
copy(s2, s1)    // dst:s2, src:s1

fmt.Println(s2)   //[8 9 2 3 4]
fmt.Println(data) //[8 9 2 3 4 5 6 7 8 9]
```
## map
### 概述
map[keyType]valueType
在一个map里所有的键都是唯一的，而且必须是支持==和!=操作符的类型，切片、函数以及包含切片的结构类型这些类型由于具有引用语义，不能作为映射的键，使用这些类型会造成编译错误
### 创建和初始化
```
var m1 map[int]string  //只是声明一个map，没有初始化, 此为空(nil)map
fmt.Println(m1 == nil) //true
//m1[1] = "mike" //err, panic: assignment to entry in nil map

//m2, m3的创建方法是等价的
m2 := map[int]string{}
m3 := make(map[int]string)
fmt.Println(m2, m3) //map[] map[]

m4 := make(map[int]string, 10) //第2个参数指定容量
fmt.Println(m4)                //map[]



//1、定义同时初始化
var m1 map[int]string = map[int]string{1: "mike", 2: "yoyo"}
fmt.Println(m1) //map[1:mike 2:yoyo]

//2、自动推导类型 :=
m2 := map[int]string{1: "mike", 2: "yoyo"}
fmt.Println(m2)
```
### 操作
#### 遍历
```
//判断某个key所对应的value是否存在, 第一个返回值是value(如果存在的话)
value, ok := m1[1]
fmt.Println("value = ", value, ", ok = ", ok) //value =  mike , ok =  true
```
#### 删除
delete(m1, 2) //删除key值为3的map
## 结构体
### 结构体初始化
```
type Student struct {
    id   int
    name string
    sex  byte
    age  int
    addr string
}

func main() {
    //1、顺序初始化，必须每个成员都初始化
    var s1 Student = Student{1, "mike", 'm', 18, "sz"}
    s2 := Student{2, "yoyo", 'f', 20, "sz"}
    //s3 := Student{2, "tom", 'm', 20} //err, too few values in struct initializer

    //2、指定初始化某个成员，没有初始化的成员为零值
    s4 := Student{id: 2, name: "lily"}

    var s5 *Student = &Student{3, "xiaoming", 'm', 16, "bj"}
    s6 := &Student{4, "rocco", 'm', 3, "sh"}

    s7 := new(Student)
    s7.id = 3

    var p *Student = &s4
    //p.成员 和(*p).成员 操作是等价的
    p.id = 5
    (*p).name = "zzz"
}
```
### 结构体比较
如果结构体的全部成员都是可以比较的，那么结构体也是可以比较的，那样的话两个结构体将可以使用 == 或 != 运算符进行比较，但不支持 > 或 < 。
### 可见性
要使某个符号对其他包（package）可见（即可以访问），需要将该符号定义为以大写字母开头。

# 面向对象编程
## 概述
封装：通过方法实现
继承：通过匿名字段实现
多态：通过接口实现
## 匿名组合
### 匿名字段
```
//人
type Person struct {
    name string
    sex  byte
    age  int
}

//学生
type Student struct {
    Person // 匿名字段，那么默认Student就包含了Person的所有字段
    id     int
    addr   string
    name   string //和Person中的name同名
}

func main() {
    //顺序初始化
    s1 := Student{Person{"mike", 'm', 18}, 1, "sz"}
    //s1 = {Person:{name:mike sex:109 age:18} id:1 addr:sz}
    fmt.Printf("s1 = %+v\n", s1)

    //s2 := Student{"mike", 'm', 18, 1, "sz"} //err

    //部分成员初始化2
    s4 := Student{Person: Person{name: "tom"}, id: 3}
    //s4 = {Person:{name:tom sex:0 age:0} id:3 addr:}
    fmt.Printf("s4 = %+v\n", s4)

    //给Student的name
    s.name = "mike"
    //默认只会给最外层的成员赋值
    //给匿名同名成员赋值，需要显示调用
    s.Person.name = "yoyo"
}
```
### 其它匿名字段
#### 自定义类型
```
type mystr string //自定义类型

type Person struct {
    name string
    sex  byte
    age  int
}

type Student struct {
    Person // 匿名字段，结构体类型
    int     // 匿名字段，内置类型
    mystr   // 匿名字段，自定义类型
}

func main() {
    //初始化
    s1 := Student{Person{"mike", 'm', 18}, 1, "bj"}

    //{Person:{name:mike sex:109 age:18} int:1 mystr:bj}
    fmt.Printf("%+v\n", s1)

    //成员的操作，打印结果：mike, m, 18, 1, bj
    fmt.Printf("%s, %c, %d, %d, %s\n", s1.name, s1.sex, s1.age, s1.int, s1.mystr)
}
```
#### 结构体指针类型
```
type Person struct { //人
    name string
    sex  byte
    age  int
}

type Student struct { //学生
    *Person // 匿名字段，结构体指针类型
    id      int
    addr    string
}

func main() {
    //初始化
    s1 := Student{&Person{"mike", 'm', 18}, 1, "bj"}

    //{Person:0xc0420023e0 id:1 addr:bj}
    fmt.Printf("%+v\n", s1)
    //mike, m, 18
    fmt.Printf("%s, %c, %d\n", s1.name, s1.sex, s1.age)

    //声明变量
    var s2 Student
    s2.Person = new(Person) //分配空间
    s2.name = "yoyo"
    s2.sex = 'f'
    s2.age = 20

    s2.id = 2
    s2.addr = "sz"

    //yoyo 102 20 2 20
    fmt.Println(s2.name, s2.sex, s2.age, s2.id, s2.age)
}
```
## 方法
在这个对象中会包含一些函数，这种带有接收者的函数，我们称为方法(method)。
### 为类型添加方法
#### 为类型添加方法
```
//在函数定义时，在其名字之前放上一个变量，即是一个方法
type MyInt int //自定义类型，给int改名为MyInt

func (a MyInt) Add(b MyInt) MyInt { //面向对象
    return a + b
}

func main() {
    var a MyInt = 1
    var b MyInt = 1

    //调用func (a MyInt) Add(b MyInt)
    fmt.Println("a.Add(b) = ", a.Add(b)) //a.Add(b) =  2
}
```
#### 值语义和引用语义
```
type Person struct {
    name string
    sex  byte
    age  int
}

//指针作为接收者，引用语义
func (p *Person) SetInfoPointer() {
    //给成员赋值
    (*p).name = "yoyo"
    p.sex = 'f'
    p.age = 22
}

//值作为接收者，值语义
func (p Person) SetInfoValue() {
    //给成员赋值
    p.name = "yoyo"
    p.sex = 'f'
    p.age = 22
}

func main() {
    //指针作为接收者，引用语义
    p1 := Person{"mike", 'm', 18} //初始化
    fmt.Println("函数调用前 = ", p1)   //函数调用前 =  {mike 109 18}
    (&p1).SetInfoPointer()
    fmt.Println("函数调用后 = ", p1) //函数调用后 =  {yoyo 102 22}

    fmt.Println("==========================")

    p2 := Person{"mike", 'm', 18} //初始化
    //值作为接收者，值语义
    fmt.Println("函数调用前 = ", p2) //函数调用前 =  {mike 109 18}
    p2.SetInfoValue()
    fmt.Println("函数调用后 = ", p2) //函数调用后 =  {mike 109 18}
}
```
### 方法集
#### 类型 *T 方法集
如果在指针上调用一个接受值的方法，Go语言会聪明地将该指针解引用，并将指针所指的底层值作为方法的接收者。

#### 类型 T 方法集
因为如果我们只有一个值，仍然可以调用一个接收者为指针类型的方法，这可以借助于Go语言传值的地址能力实现
```
//指针作为接收者，引用语义
func (p *Person) SetInfoPointer() {
    (*p).name = "yoyo"
    p.sex = 'f'
    p.age = 22
}
//值作为接收者，值语义
func (p Person) SetInfoValue() {
    p.name = "xxx"
    p.sex = 'm'
    p.age = 33
}
func main() {
    //p 为指针类型
    var p *Person = &Person{"mike", 'm', 18}
    p.SetInfoPointer() //func (p) SetInfoPointer()
    p.SetInfoValue()    //func (*p) SetInfoValue()
    (*p).SetInfoValue() //func (*p) SetInfoValue()

    var j Person = Person{"mike", 'm', 18}
    (&j).SetInfoPointer() //func (&p) SetInfoPointer()
    j.SetInfoPointer()    //func (&p) SetInfoPointer()
    j.SetInfoValue()      //func (p) SetInfoValue()
    (&j).SetInfoValue()   //func (*&p) SetInfoValue()
}
```
### 匿名字段
#### 方法的继承
如果匿名字段实现了一个方法，那么包含这个匿名字段的struct也能调用该方法
#### 方法的重写
在继承的基础上，结构体重写，匿名字段的方法，并显示调用。
### 表达式
方法也可以进行赋值和传递。
根据调用者不同，方法分为两种表现形式：方法值和方法表达式。两者都可像普通函数那样赋值和传参，区别在于方法值绑定实例，⽽方法表达式则须显式传参。
```
type Person struct {
    name string
    sex  byte
    age  int
}

func (p *Person) PrintInfoPointer() {
    fmt.Printf("%p, %v\n", p, p)
}

func (p Person) PrintInfoValue() {
    fmt.Printf("%p, %v\n", &p, p)
}

func main() {
    p := Person{"mike", 'm', 18}
    p.PrintInfoPointer() //0xc0420023e0, &{mike 109 18}

    pFunc1 := p.PrintInfoPointer //方法值，隐式传递 receiver
    pFunc1()                     //0xc0420023e0, &{mike 109 18}

    pFunc2 := p.PrintInfoValue
    pFunc2() //0xc042048420, {mike 109 18}

    //方法表达式， 须显式传参
    //func pFunc1(p *Person))
    pFunc3 := (*Person).PrintInfoPointer
    pFunc3(&p) //0xc0420023e0, &{mike 109 18}

    pFunc4 := Person.PrintInfoValue
    pFunc4(p) //0xc042002460, {mike 109 18}
}
```
## 接口
### 接口的使用
#### 接口定义
- 命名以er结尾
- 接口可以匿名嵌入其他接口，或嵌入到结构中

### 接口组合
#### 接口嵌入
如果一个interface1作为interface2的一个嵌入字段，那么interface2隐式的包含了interface1里面的方法。
#### 接口转换
超集接⼝对象可转换为⼦集接⼝，反之出错
```
type Humaner interface {
    SayHi()
}

type Personer interface {
    Humaner //这里像写了SayHi()一样
    Sing(lyrics string)
}

func main() {
    //var i1 Humaner = &Student{"mike", 88.88}
    //var i2 Personer = i1 //err

    //Personer为超集，Humaner为子集
    var i1 Personer = &Student{"mike", 88.88}
    var i2 Humaner = i1
    i2.SayHi() //Student[mike, 88.880000] say hi!!
}
```
### 空接口
空接口(interface{})不包含任何的方法，正因为如此，所有的类型都实现了空接口，因此空接口可以存储任意类型的数值。它有点类似于C语言的void *类型。
当函数可以接受任意的对象实例时，我们会将其声明为interface{}，最典型的例子是标准库fmt中PrintXXX系列的函数，例如：
```
    func Printf(fmt string, args ...interface{})
    func Println(args ...interface{})
```

### 类型查询 
#### comma-ok 断言
value, ok = element.(T)，这里value就是变量的值，ok是一个bool类型，element是interface变量，T是断言的类型。
如果element里面确实存储了T类型的数值，那么ok返回true，否则返回false。
```
type Element interface{}

type Person struct {
    name string
    age  int
}

func main() {
    list := make([]Element, 3)
    list[0] = 1       // an int
    list[1] = "Hello" // a string
    list[2] = Person{"mike", 18}

    for index, element := range list {
        if value, ok := element.(int); ok {
            fmt.Printf("list[%d] is an int and its value is %d\n", index, value)
        } else if value, ok := element.(string); ok {
            fmt.Printf("list[%d] is a string and its value is %s\n", index, value)
        } else if value, ok := element.(Person); ok {
            fmt.Printf("list[%d] is a Person and its value is [%s, %d]\n", index, value.name, value.age)
        } else {
            fmt.Printf("list[%d] is of a different type\n", index)
        }
    }

    /*  打印结果：
    list[0] is an int and its value is 1
    list[1] is a string and its value is Hello
    list[2] is a Person and its value is [mike, 18]
    */
}
```
#### switch 测试
```
type Element interface{}

type Person struct {
    name string
    age  int
}

func main() {
    list := make([]Element, 3)
    list[0] = 1       //an int
    list[1] = "Hello" //a string
    list[2] = Person{"mike", 18}

    for index, element := range list {
        switch value := element.(type) {
        case int:
            fmt.Printf("list[%d] is an int and its value is %d\n", index, value)
        case string:
            fmt.Printf("list[%d] is a string and its value is %s\n", index, value)
        case Person:
            fmt.Printf("list[%d] is a Person and its value is [%s, %d]\n", index, value.name, value.age)
        default:
            fmt.Println("list[%d] is of a different type", index)
        }
    }
}
```
# 异常处理
## error接口
```
type error interface {
    Error() string
}

Go语言的标准库代码包errors为用户提供如下方法：
package errors

type errorString struct { 
    text string 
}

func New(text string) error { 
    return &errorString{text} 
}

func (e *errorString) Error() string { 
    return e.text 
}

// 或者使用标准库
package fmt
import "errors"

func Errorf(format string, args ...interface{}) error {
    return errors.New(Sprintf(format, args...))
}

```
## panic
在通常情况下，向程序使用方报告错误状态的方式可以是返回一个额外的error类型值。
但是，当遇到不可恢复的错误状态的时候，如数组访问越界、空指针引用等，这些运行时错误会引起painc异常。
反过来讲，在一般情况下，我们不应通过调用panic函数来报告普通的错误，而应该只把它作为报告致命错误的一种方式。当某些不应该发生的场景发生时，我们就应该调用panic。
## recover
Go语言为我们提供了专用于“拦截”运行时panic的内建函数——recover。它可以是当前的程序从运行时panic的状态中恢复并重新获得流程控制权。
注意：recover只有在defer调用的函数中有效。可以直接打印该函数，获取异常信息。
# 文本文件处理
## 字符串处理
### 字符串操作
```
fmt.Println(strings.Contains("seafood", "foo"))  // 运行结果: true

s := []string{"foo", "bar", "baz"}
fmt.Println(strings.Join(s, ", "))  // 运行结果: foo, bar, baz

fmt.Println(strings.Index("chicken", "ken"))  // 运行结果: 4

fmt.Println("ba" + strings.Repeat("na", 2))  // 运行结果: banana

fmt.Println(strings.Replace("oink oink oink", "k", "ky", 2))  // 运行结果: oinky oinky oink

fmt.Printf("%q\n", strings.Split("a,b,c", ","))  // 运行结果: ["a" "b" "c"]

fmt.Printf("[%q]", strings.Trim(" !!! Achtung !!! ", "! "))  // 运行结果: ["Achtung"]

fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   "))  // 运行结果: Fields are: ["foo" "bar" "baz"]
```
### 字符串转换
#### Append
Append 系列函数将整数等转换为字符串后，添加到现有的字节数组中。
```
str := make([]byte, 0, 100)
str = strconv.AppendInt(str, 4567, 10) //以10进制方式追加
str = strconv.AppendBool(str, false)
str = strconv.AppendQuote(str, "abcdefg")
str = strconv.AppendQuoteRune(str, '单')

fmt.Println(string(str)) //4567false"abcdefg"'单'
```
#### Format
Format 系列函数把其他类型的转换为字符串。
```
a := strconv.FormatBool(false)
b := strconv.FormatInt(1234, 10)
c := strconv.FormatUint(12345, 10)
d := strconv.Itoa(1023)

fmt.Println(a, b, c, d) //false 1234 12345 1023
```
#### Parse
Parse 系列函数把字符串转换为其他类型。
```
package main

import (
    "fmt"
    "strconv"
)

func checkError(e error) {
    if e != nil {
        fmt.Println(e)
    }
}
func main() {
    a, err := strconv.ParseBool("false")
    checkError(err)
    b, err := strconv.ParseFloat("123.23", 64)
    checkError(err)
    c, err := strconv.ParseInt("1234", 10, 64)
    checkError(err)
    d, err := strconv.ParseUint("12345", 10, 64)
    checkError(err)
    e, err := strconv.Atoi("1023")
    checkError(err)
    fmt.Println(a, b, c, d, e) //false 123.23 1234 12345 1023
}
```
## JSON处理
### 编码JSON
#### struct tag
- 字段的tag是"-"，那么这个字段不会输出到JSON
- tag中带有自定义名称，那么这个自定义名称会出现在JSON的字段名中
- tag中如果带有"omitempty"选项，那么如果该字段值为空，就不会输出到JSON串中
- 如果字段类型是bool, string, int, int64等，而tag中带有",string"选项，那么这个字段在输出到JSON的时候会把该字段对应的值转换成JSON字符串

```
type IT struct {
    //Company不会导出到JSON中
    Company string `json:"-"`

    // Subjects 的值会进行二次JSON编码
    Subjects []string `json:"subjects"`

    //转换为字符串，再输出
    IsOk bool `json:",string"`

    // 如果 Price 为空，则不输出到JSON串中
    Price float64 `json:"price, omitempty"`
}

func main() {
    t1 := IT{Company: "itcast", Subjects: []string{"Go", "C++", "Python", "Test"}, IsOk: true}

    b, err := json.Marshal(t1)
    //json.MarshalIndent(t1, "", "    ")
    if err != nil {
        fmt.Println("json err:", err)
    }
    fmt.Println(string(b))
    //输出结果：{"subjects":["Go","C++","Python","Test"],"IsOk":"true","price":0}
}
```
### 解码JSON
#### 解析到结构体
```
type IT struct {
    Company  string   `json:"company"`
    Subjects []string `json:"subjects"`
    IsOk     bool     `json:"isok"`
    Price    float64  `json:"price"`
}

func main() {
    b := []byte(`{
    "company": "itcast",
    "subjects": [
        "Go",
        "C++",
        "Python",
        "Test"
    ],
    "isok": true,
    "price": 666.666
}`)

var t IT
err := json.Unmarshal(b, &t)
if err != nil {
    fmt.Println("json err:", err)
}
fmt.Println(t)
//运行结果：{itcast [Go C++ Python Test] true 666.666}

//只想要Subjects字段
type IT2 struct {
    Subjects []string `json:"subjects"`
}

var t2 IT2
err = json.Unmarshal(b, &t2)
if err != nil {
    fmt.Println("json err:", err)
}
fmt.Println(t2)
//运行结果：{[Go C++ Python Test]}
}
```
#### 解析到interface
```
func main() {
    b := []byte(`{
        "company": "itcast",
        "subjects": [
            "Go",
            "C++",
            "Python",
            "Test"
        ],
        "isok": true,
        "price": 666.666
    }`)

    var t interface{}
    err := json.Unmarshal(b, &t)
    if err != nil {
        fmt.Println("json err:", err)
    }
    fmt.Println(t)

    //使用断言判断类型
    m := t.(map[string]interface{})
    for k, v := range m {
        switch vv := v.(type) {
        case string:
            fmt.Println(k, "is string", vv)
        case int:
            fmt.Println(k, "is int", vv)
        case float64:
            fmt.Println(k, "is float64", vv)
        case bool:
            fmt.Println(k, "is bool", vv)
        case []interface{}:
            fmt.Println(k, "is an array:")
            for i, u := range vv {
                fmt.Println(i, u)
            }
        default:
            fmt.Println(k, "is of a type I don't know how to handle")
        }
    }
}
```
## 文件操作
### 相关API介绍
```
// 建立文件
func Create(name string) (file *File, err Error)
// 根据提供的文件名创建新的文件，返回一个文件对象，默认权限是0666的文件，返回的文件对象是可读写的。

// 打开文件
func Open(name string) (file *File, err Error)
// 该方法打开一个名称为name的文件，但是是只读方式，内部实现其实调用了OpenFile。

// 写文件
func (file *File) Write(b []byte) (n int, err Error)
// 写入byte类型的信息到文件
func (file *File) WriteAt(b []byte, off int64) (n int, err Error)
// 在指定位置开始写入byte类型的信息
func (file *File) WriteString(s string) (ret int, err Error)
// 写入string信息到文件

// 读文件
func (file *File) Read(b []byte) (n int, err Error)
// 读取数据到b中
func (file *File) ReadAt(b []byte, off int64) (n int, err Error)
// 从off开始读取数据到b中

// 删除文件
func Remove(name string) Error
// 调用该函数就可以删除文件名为name的文件
```
#### 案例: 拷贝文件
```
package main

import (
    "fmt"
    "io"
    "os"
)

func main() {
    args := os.Args //获取用户输入的所有参数

    //如果用户没有输入,或参数个数不够,则调用该函数提示用户
    if args == nil || len(args) != 3 {
        fmt.Println("useage : xxx srcFile dstFile")
        return
    }

    srcPath := args[1] //获取输入的第一个参数
    dstPath := args[2] //获取输入的第二个参数
    fmt.Printf("srcPath = %s, dstPath = %s\n", srcPath, dstPath)

    if srcPath == dstPath {
        fmt.Println("源文件和目的文件名字不能相同")
        return
    }

    srcFile, err1 := os.Open(srcPath) //打开源文件
    if err1 != nil {
        fmt.Println(err1)
        return
    }

    dstFile, err2 := os.Create(dstPath) //创建目的文件
    if err2 != nil {
        fmt.Println(err2)
        return
    }

    buf := make([]byte, 1024) //切片缓冲区
    for {
        //从源文件读取内容，n为读取文件内容的长度
        n, err := srcFile.Read(buf)
        if err != nil && err != io.EOF {
            fmt.Println(err)
            break
        }

        if n == 0 {
            fmt.Println("文件处理完毕")
            break
        }

        //切片截取
        tmp := buf[:n]
        //把读取的内容写入到目的文件
        dstFile.Write(tmp)
    }

    //关闭文件
    srcFile.Close()
    dstFile.Close()
}
```
# 并发编程
## 概述
### Go语言并发优势
Go从语言层面就支持了并行。Go语言为并发编程而内置的上层API基于CSP(communicating sequential processes, 顺序通信进程)模型。这就意味着显式锁都是可以避免的，因为Go语言通过相册安全的通道发送和接受数据以实现同步，这大大地简化了并发程序的编写。
## goroutine
### goroutine 是什么
goroutine说到底其实就是协程，但是它比线程更小，十几个goroutine可能体现在底层就是五六个线程，Go语言内部帮你实现了这些goroutine之间的内存共享。执行goroutine只需极少的栈内存(大概是4~5KB)，当然会根据相应的数据伸缩。也正因为如此，可同时运行成千上万个并发任务。goroutine比thread更易用、更高效、更轻便。
### 创建 goroutine
只需在函数调⽤语句前添加 go 关键字，就可创建并发执⾏单元。开发⼈员无需了解任何执⾏细节，调度器会自动将其安排到合适的系统线程上执行。
主goroutine退出后，其它的工作goroutine也会自动退出
```
package main

import (
    "fmt"
    "time"
)

func newTask() {
    i := 0
    for {
        i++
        fmt.Printf("new goroutine: i = %d\n", i)
        time.Sleep(1 * time.Second) //延时1s
    }
}

func main() {
    //创建一个 goroutine，启动另外一个任务
    go newTask()

    i := 0
    //main goroutine 循环打印
    for {
        i++
        fmt.Printf("main goroutine: i = %d\n", i)
        time.Sleep(1 * time.Second) //延时1s
    }
}
```
### runtime 包
#### Gosched
runtime.Gosched() 用于让出CPU时间片，让出当前goroutine的执行权限，调度器安排其他等待的任务运行，并在下次某个时候从该位置恢复执行。
```
func main() {
    //创建一个goroutine
    go func(s string) {
        for i := 0; i < 2; i++ {
            fmt.Println(s)
        }
    }("world")

    for i := 0; i < 2; i++ {
        runtime.Gosched() //import "runtime"
        /*
            屏蔽runtime.Gosched()运行结果如下：
                hello
                hello

            有runtime.Gosched()运行结果如下：
                world
                world
                hello
                hello
        */
        fmt.Println("hello")
    }
}
```
#### Goexit
调用 runtime.Goexit() 将立即终止当前 goroutine 执⾏，调度器确保所有已注册 defer延迟调用被执行。
```
func main() {
    go func() {
        defer fmt.Println("A.defer")

        func() {
            defer fmt.Println("B.defer")
            runtime.Goexit() // 终止当前 goroutine, import "runtime"
            fmt.Println("B") // 不会执行
        }()

        fmt.Println("A") // 不会执行
    }() //别忘了()

    //死循环，目的不让主goroutine结束
    for {
    }
}
```
#### GOMAXPROCS
调用 runtime.GOMAXPROCS() 用来设置可以并行计算的CPU核数的最大值，并返回之前的值。
```
func main() {
    //n := runtime.GOMAXPROCS(1) //打印结果：111111111111111111110000000000000000000011111...
    n := runtime.GOMAXPROCS(2)     //打印结果：010101010101010101011001100101011010010100110...
    fmt.Printf("n = %d\n", n)

    for {
        go fmt.Print(0)
        fmt.Print(1)
    }
}
```
在第一次执行(runtime.GOMAXPROCS(1))时，最多同时只能有一个goroutine被执行。所以
会打印很多1。过了一段时间后，GO调度器会将其置为休眠，并唤醒另一个goroutine，这时候就开始打印很多0了，在打印的时候，goroutine是被调度到操作系统线程上的。

在第二次执行(runtime.GOMAXPROCS(2))时，我们使用了两个CPU，所以两个goroutine可以一起被执行，以同样的频率交替打印0和1。
## channel
goroutine运行在相同的地址空间，因此访问共享内存必须做好同步。goroutine 奉行通过通信来共享内存，而不是共享内存来通信。
### channel 类型
和map类似，channel也一个对应make创建的底层数据结构的引用。
当我们复制一个channel或用于函数参数传递时，我们只是拷贝了一个channel引用，因此调用者何被调用者将引用同一个channel对象。和其它的引用类型一样，channel的零值也是nil。
```
// 定义一个channel时，也需要定义发送到channel的值的类型。
make(chan Type, capacity)
```
当 capacity= 0 时，channel 是无缓冲阻塞读写的，当capacity> 0 时，channel 有缓冲、是非阻塞的，直到写满 capacity个元素才阻塞写入。
```
// channel通过操作符<-来接收和发送数据，发送和接收数据语法：
channel <- value      //发送value到channel
<-channel             //接收并将其丢弃
x := <-channel        //从channel中接收数据，并赋值给x
x, ok := <-channel    //功能同上，同时检查通道是否已关闭或者是否为空
```
默认情况下，channel接收和发送数据都是阻塞的，除非另一端已经准备好，这样就使得goroutine同步变的更加的简单，而不需要显式的lock。
### 无缓冲的 channel 与 有缓冲的 channel
这种类型的通道要求发送 goroutine 和接收 goroutine 同时准备好，才能完成发送和接收操作。如果两个goroutine没有同时准备好，通道会导致先执行发送或接收操作的 goroutine 阻塞等待。
```
func main() {
    c := make(chan int, 0) //无缓冲的通道 结果交替进行
    c := make(chan int, 3) //带缓冲的通道 结果一批一批输出
    //内置函数 len 返回未被读取的缓冲元素数量， cap 返回缓冲区大小
    fmt.Printf("len(c)=%d, cap(c)=%d\n", len(c), cap(c))

    go func() {
        defer fmt.Println("子协程结束")

        for i := 0; i < 3; i++ {
            c <- i
            fmt.Printf("子协程正在运行[%d]: len(c)=%d, cap(c)=%d\n", i, len(c), cap(c))
        }
    }()

    time.Sleep(2 * time.Second) //延时2s

    for i := 0; i < 3; i++ {
        num := <-c //从c中接收数据，并赋值给num
        fmt.Println("num = ", num)
    }

    fmt.Println("main协程结束")
}
```
### range 和 close
```
func main() {
    c := make(chan int)

    go func() {
        for i := 0; i < 5; i++ {
            c <- i
        }
        //把 close(c) 注释掉，程序会一直阻塞在 if data, ok := <-c; ok 那一行
        close(c) // 让接收者也能及时知道没有多余的值可接收将是有用的，因为接收者可以停止不必要的接收等待。close函数来关闭channel实现
    }()

    for {
        //ok为true说明channel没有关闭，为false说明管道已经关闭
        if data, ok := <-c; ok {
            fmt.Println(data)
        } else {
            break
        }
    }

    fmt.Println("Finished")
}
```
注意点
- channel不像文件一样需要经常去关闭，只有当你确实没有任何发送数据了，或者你想显式的结束range循环之类的，才去关闭channel
- 关闭channel后，无法向channel 再发送数据(引发 panic 错误后导致接收立即返回零值)
- 关闭channel后，可以继续向channel接收数据
- 对于nil channel，无论收发都会被阻塞
```
func main() {
    c := make(chan int)

    go func() {
        for i := 0; i < 5; i++ {
            c <- i
        }
        //把 close(c) 注释掉，程序会一直阻塞在 for data := range c 那一行
        close(c)
    }()

    for data := range c { // 可以使用 range 来迭代不断操作channel
        fmt.Println(data)
    }
    fmt.Println("Finished")
}
```

### 单方向的 channel
```
var ch1 chan int       // ch1是一个正常的channel，不是单向的
var ch2 chan<- float64 // ch2是单向channel，只用于写float64数据
var ch3 <-chan int     // ch3是单向channel，只用于读取int数据

// 可以将 channel 隐式转换为单向队列，只收或只发，不能将单向 channel 转换为普通 channel
c := make(chan int, 3)
var send chan<- int = c // send-only
var recv <-chan int = c // receive-only
send <- 1
//<-send //invalid operation: <-send (receive from send-only type chan<- int)
<-recv
//recv <- 2 //invalid operation: recv <- 2 (send to receive-only type <-chan int)

//不能将单向 channel 转换为普通 channel
d1 := (chan int)(send) //cannot convert send (type chan<- int) to type chan int
d2 := (chan int)(recv) //cannot convert recv (type <-chan int) to type chan int
```
### 定时器
#### Timer
Timer是一个定时器，代表未来的一个单一事件，你可以告诉timer你要等待多长时间，它提供一个channel，在将来的那个时间那个channel提供了一个时间值。只能执行一次。
```
import "fmt"
import "time"

func main() {
    //创建定时器，2秒后，定时器就会向自己的C字节发送一个time.Time类型的元素值
    timer1 := time.NewTimer(time.Second * 2)
    t1 := time.Now() //当前时间
    fmt.Printf("t1: %v\n", t1)

    t2 := <-timer1.C
    fmt.Printf("t2: %v\n", t2)

    //如果只是想单纯的等待的话，可以使用 time.Sleep 来实现
    timer2 := time.NewTimer(time.Second * 2)
    <-timer2.C
    fmt.Println("2s后")

    time.Sleep(time.Second * 2)
    fmt.Println("再一次2s后")

    <-time.After(time.Second * 2)
    fmt.Println("再再一次2s后")

    timer3 := time.NewTimer(time.Second)
    go func() {
        <-timer3.C
        fmt.Println("Timer 3 expired")
    }()

    stop := timer3.Stop() //停止定时器
    if stop {
        fmt.Println("Timer 3 stopped")
    }

    fmt.Println("before")
    timer4 := time.NewTimer(time.Second * 5) //原来设置3s
    timer4.Reset(time.Second * 1)            //重新设置时间
    <-timer4.C
    fmt.Println("after")
}
```
#### Ticker
Ticker是一个定时触发的计时器，它会以一个间隔(interval)往channel发送一个事件(当前时间)，而channel的接收者可以以固定的时间间隔从channel中读取事件。可以执行多次。
```
func main() {
    //创建定时器，每隔1秒后，定时器就会给channel发送一个事件(当前时间)
    ticker := time.NewTicker(time.Second * 1)

    i := 0
    go func() {
        for { //循环
            <-ticker.C
            i++
            fmt.Println("i = ", i)

            if i == 5 {
                ticker.Stop() //停止定时器
            }
        }
    }() //别忘了()

    //死循环，特地不让main goroutine结束
    for {
    }
}
```
## select
### select 作用
通过select可以监听channel上的数据流动。由select开始一个新的选择块，每个选择条件由case语句来描述。select有比较多的限制，其中最大的一条限制就是每个case语句里必须是一个IO操作。
```
select {
    case <-chan1:
        // 如果chan1成功读到数据，则进行该case处理语句
    case chan2 <- 1:
        // 如果成功向chan2写入数据，则进行该case处理语句
    default:
        // 如果上面都没有成功，则进入default处理流程
}
```
在一个select语句中，Go语言会按顺序从头至尾评估每一个发送和接收的语句。
如果其中的任意一语句可以继续执行(即没有被阻塞)，那么就从那些可以执行的语句中任意选择一条来使用。
如果没有任意一条语句可以执行(即所有的通道都被阻塞)，那么有两种可能的情况
- 如果给出了default语句，那么就会执行default语句，同时程序的执行会从select语句后的语句中恢复。
- 如果没有default语句，那么select语句将被阻塞，直到至少有一个通信可以进行下去
```
func fibonacci(c, quit chan int) {
    x, y := 1, 1
    for {
        select {
        case c <- x:
            x, y = y, x+y
        case <-quit:
            fmt.Println("quit")
            return
        }
    }
}

func main() {
    c := make(chan int)
    quit := make(chan int)

    go func() {
        for i := 0; i < 6; i++ {
            fmt.Println(<-c)
        }
        quit <- 0
    }()

    fibonacci(c, quit)
}
```

### 超时
```
func main() {
    c := make(chan int)
    o := make(chan bool)
    go func() {
        for {
            select {
            case v := <-c:
                fmt.Println(v)
            case <-time.After(5 * time.Second): 
            // 有时候会出现goroutine阻塞的情况，那么我们如何避免整个程序进入阻塞的情况呢？我们可以利用select来设置超时，通过如下的方式实现
                fmt.Println("timeout")
                o <- true
                break
            }
        }
    }()
    //c <- 666 // 注释掉，引发 timeout
    <-o
}
```
# 网络编程
## Socket 编程
### 服务器代码
```
package main

import (
    "fmt"
    "log"
    "net"
    "strings"
)

func dealConn(conn net.Conn) {

    defer conn.Close() //此函数结束时，关闭连接套接字

    //conn.RemoteAddr().String()：连接客服端的网络地址
    ipAddr := conn.RemoteAddr().String()
    fmt.Println(ipAddr, "连接成功")

    buf := make([]byte, 1024) //缓冲区，用于接收客户端发送的数据

    for {
        //阻塞等待用户发送的数据
        n, err := conn.Read(buf) //n代码接收数据的长度
        if err != nil {
            fmt.Println(err)
            return
        }
        //切片截取，只截取有效数据
        result := buf[:n]
        fmt.Printf("接收到数据来自[%s]==>[%d]:%s\n", ipAddr, n, string(result))
        if "exit" == string(result) { //如果对方发送"exit"，退出此链接
            fmt.Println(ipAddr, "退出连接")
            return
        }

        //把接收到的数据转换为大写，再给客户端发送
        conn.Write([]byte(strings.ToUpper(string(result))))
    }
}

func main() {
    //创建、监听socket
    listenner, err := net.Listen("tcp", "127.0.0.1:8000")
    if err != nil {
        log.Fatal(err) //log.Fatal()会产生panic
    }

    defer listenner.Close()

    for {
        conn, err := listenner.Accept() //阻塞等待客户端连接
        if err != nil {
            log.Println(err)
            continue
        }

        go dealConn(conn)
    }
}
```
### 客户端代码
```
package main

import (
    "fmt"
    "log"
    "net"
)

func main() {
    //客户端主动连接服务器
    conn, err := net.Dial("tcp", "127.0.0.1:8000")
    if err != nil {
        log.Fatal(err) //log.Fatal()会产生panic
        return
    }

    defer conn.Close() //关闭

    buf := make([]byte, 1024) //缓冲区
    for {
        fmt.Printf("请输入发送的内容：")
        fmt.Scan(&buf)
        fmt.Printf("发送的内容：%s\n", string(buf))

        //发送数据
        conn.Write(buf)

        //阻塞等待服务器回复的数据
        n, err := conn.Read(buf) //n代码接收数据的长度
        if err != nil {
            fmt.Println(err)
            return
        }

        //切片截取，只截取有效数据
        result := buf[:n]
        fmt.Printf("接收到数据[%d]:%s\n", n, string(result))
    }
}
```
## HTTP 编程
### HTTP 编程实例
#### HTTP 服务端
```
package main

import (
    "fmt"
    "net/http"
)

//服务端编写的业务逻辑处理程序
//hander函数： 具有func(w http.ResponseWriter, r *http.Requests)签名的函数
func myHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Println(r.RemoteAddr, "连接成功")  //r.RemoteAddr远程网络地址
    fmt.Println("method = ", r.Method) //请求方法
    fmt.Println("url = ", r.URL.Path)
    fmt.Println("header = ", r.Header)
    fmt.Println("body = ", r.Body)

    w.Write([]byte("hello go")) //给客户端回复数据
}

func main() {
    http.HandleFunc("/go", myHandler)

    //该方法用于在指定的 TCP 网络地址 addr 进行监听，然后调用服务端处理程序来处理传入的连接请求。
    //该方法有两个参数：第一个参数 addr 即监听地址；第二个参数表示服务端处理程序，通常为空
    //第二个参数为空意味着服务端调用 http.DefaultServeMux 进行处理
    http.ListenAndServe("127.0.0.1:8000", nil)
}
```
#### HTTP 客户端
```
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
)

func main() {

    //get方式请求一个资源
    //resp, err := http.Get("http://www.baidu.com")
    //resp, err := http.Get("http://www.neihan8.com/article/index.html")
    resp, err := http.Get("http://127.0.0.1:8000/go")
    if err != nil {
        log.Println(err)
        return
    }

    defer resp.Body.Close() //关闭

    fmt.Println("header = ", resp.Header)
    fmt.Printf("resp status %s\nstatusCode %d\n", resp.Status, resp.StatusCode)
    fmt.Printf("body type = %T\n", resp.Body)

    buf := make([]byte, 2048) //切片缓冲区
    var tmp string

    for {
        n, err := resp.Body.Read(buf) //读取body包内容
        if err != nil && err != io.EOF {
            fmt.Println(err)
            return
        }

        if n == 0 {
            fmt.Println("读取内容结束")
            break
        }
        tmp += string(buf[:n]) //累加读取的内容
    }

    fmt.Println("buf = ", string(tmp))
}
```


































































































































































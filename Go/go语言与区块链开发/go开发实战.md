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






































































































































































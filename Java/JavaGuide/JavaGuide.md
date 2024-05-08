JavaGuide
# Java
## 基础
### Java基础常见面试题总结（上）
#### 基础概念与常识
##### JVM vs JDK vs JRE
不过，从 JDK 9 开始，就不需要区分 JDK 和 JRE 的关系了，取而代之的是模块系统（JDK 被重新组织成 94 个模块）+ jlink 工具 (随 Java 9 一起发布的新命令行工具，用于生成自定义 Java 运行时映像，该映像仅包含给定应用程序所需的模块) 。并且，从 JDK 11 开始，Oracle 不再提供单独的 JRE 下载。
##### AOT有什么优点？为什么不全部使用AOT呢？
DK 9 引入了一种新的编译模式 AOT(Ahead of Time Compilation) 。和 JIT 不同的是，这种编译模式会在程序被执行前就将其编译成机器码，属于静态编译（C、 C++，Rust，Go 等语言就是静态编译）。AOT 避免了 JIT 预热等各方面的开销，可以提高 Java 程序的启动速度，避免预热时间长。并且，AOT 还能减少内存占用和增强 Java 程序的安全性（AOT 编译后的代码不容易被反编译和修改），特别适合云原生场景。
AOT 编译无法支持 Java 的一些动态特性，如反射、动态代理、动态加载、JNI（Java Native Interface）等。然而，很多框架和库（如 Spring、CGLIB）都用到了这些特性。
#### 基本语法
##### 移位运算符
- << :左移运算符，向左移若干位，高位丢弃，低位补零。x << 1,相当于 x 乘以 2(不溢出的情况下)。
- >> :带符号右移，向右移若干位，高位补符号位，低位丢弃。正数高位补 0,负数高位补 1。x >> 1,相当于 x 除以 2。
- >>> :无符号右移，忽略符号位，空位都以 0 补齐。
#### 基本数据类型
##### 基本类型和包装类型的区别？
- 用途：除了定义一些常量和局部变量之外，我们在其他地方比如方法参数、对象属性中很少会使用基本类型来定义变量。并且，包装类型可用于泛型，而基本类型不可以。
- 存储方式：基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中，基本数据类型的成员变量（未被 static 修饰 ）存放在 Java 虚拟机的堆中。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。
- 占用空间：相比于包装类型（对象类型）， 基本数据类型占用的空间往往非常小。
- 默认值：成员变量包装类型不赋值就是 null ，而基本类型有默认值且不是 null。
- 比较方式：对于基本数据类型来说，== 比较的是值。对于包装数据类型来说，== 比较的是对象的内存地址。所有整型包装类对象之间值的比较，全部使用 equals() 方法。
基本数据类型存放在栈中是一个常见的误区！ 
基本数据类型的存储位置取决于它们的作用域和声明方式。如果它们是局部变量，那么它们会存放在栈中；如果它们是成员变量，那么它们会存放在堆中。
#### 方法
#### 重载和重写有什么区别？
重写的返回值类型 这里需要额外多说明一下，上面的表述不太清晰准确：如果方法的返回类型是 void 和基本数据类型，则返回值重写时不可修改。但是如果方法的返回值是引用类型，重写时是可以返回该引用类型的子类的。
#### 遇到方法重载时会优先匹配固定参数还是可变参数的方法呢？
答案是会优先匹配固定参数的方法，因为固定参数的方法匹配度更高。
### Java基础常见面试题总结（中）
#### 面向对象基础
##### 创建一个对象用什么运算符?对象实体与对象引用有何不同?
- 一个对象引用可以指向 0 个或 1 个对象（一根绳子可以不系气球，也可以系一个气球）；
- 一个对象可以有 n 个引用指向它（可以用 n 条绳子系住一个气球）。
##### 对象的相等和引用相等的区别？
- 对象的相等一般比较的是内存中存放的内容是否相等。
- 引用相等一般比较的是他们指向的内存地址是否相等。
##### 构造方法有哪些特点？是否可被 override?
构造方法不能被 override（重写）,但是可以 overload（重载）,所以你可以看到一个类中有多个构造函数的情况。
##### 面向对象三大特征
###### 封装
###### 继承
关于继承如下 3 点请记住：
- 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，只是拥有。
- 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
- 子类可以用自己的方式实现父类的方法。（以后介绍）。
##### 接口和抽象类有什么共同点和区别？
Java 8 可以用 default 关键字在接口中定义默认方法
接口中的成员变量只能是 public static final 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。
#### Object
##### 为什么要有hashCode?
我们在前面也提到了添加元素进HashSet的过程，如果 HashSet 在对比的时候，同样的 hashCode 有多个对象，它会继续使用 equals() 来判断是否真的相同。也就是说 hashCode 帮助我们大大缩小了查找成本。
##### 为什么重写 equals() 时必须重写 hashCode() 方法？
- equals 方法判断两个对象是相等的，那这两个对象的 hashCode 值也要相等。
- 两个对象有相同的 hashCode 值，他们也不一定是相等的（哈希碰撞）。
#### String
##### String 为什么是不可变的?
String 类中使用 final 关键字修饰字符数组来保存字符串。
String 真正不可变有下面几点原因：
- 保存字符串的数组被 final 修饰且为私有的，并且String 类没有提供/暴露修改这个字符串的方法。
- String 类被 final 修饰导致其不能被继承，进而避免了子类破坏 String 不可变。
在 Java 9 之后，String、StringBuilder 与 StringBuffer 的实现改用 byte 数组存储字符串。
##### Java 9 为何要将 String 的底层实现由 char[] 改成了 byte[] ?
新版的 String 其实支持两个编码方案：Latin-1 和 UTF-16。如果字符串中包含的汉字没有超过 Latin-1 可表示范围内的字符，那就会使用 Latin-1 作为编码方案。Latin-1 编码方案下，byte 占一个字节(8 位)，char 占用 2 个字节（16），byte 相较 char 节省一半的内存空间。
JDK 官方就说了绝大部分字符串对象只包含 Latin-1 可表示的字符。
如果字符串中包含的汉字超过 Latin-1 可表示范围内的字符，byte 和 char 所占用的空间是一样的。
##### 字符串拼接用“+” 还是 StringBuilder?
不过，使用 “+” 进行字符串拼接会产生大量的临时对象的问题在 JDK9 中得到了解决。在 JDK9 当中，字符串相加 “+” 改为了用动态方法 makeConcatWithConstants() 来实现，而不是大量的 StringBuilder 了。这个改进是 JDK9 的 JEP 280提出的，这也意味着 JDK 9 之后，你可以放心使用“+” 进行字符串拼接了。
##### 字符串常量池的作用了解吗？
字符串常量池 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。
##### String s1 = new String("abc");这句话创建了几个字符串对象？
会创建 1 或 2 个字符串对象。
1、如果字符串常量池中不存在字符串对象“abc”的引用，那么它会在堆上创建两个字符串对象，其中一个字符串对象的引用会被保存在字符串常量池中。
2、如果字符串常量池中已存在字符串对象“abc”的引用，则只会在堆中创建 1 个字符串对象“abc”。
##### String#intern 方法有什么作用?
String.intern() 是一个 native（本地）方法，其作用是将指定的字符串对象的引用保存在字符串常量池中，可以简单分为两种情况：
- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回。
##### String 类型的变量和常量做“+”运算时发生了什么？
对于编译期可以确定值的字符串，也就是常量字符串 ，jvm 会将其存入字符串常量池。并且，字符串常量拼接得到的字符串常量在编译阶段就已经被存放字符串常量池，这个得益于编译器的优化。
在编译过程中，Javac 编译器（下文中统称为编译器）会进行一个叫做 常量折叠(Constant Folding) 的代码优化。
常量折叠会把常量表达式的值求出来作为常量嵌在最终生成的代码中，这是 Javac 编译器会对源代码做的极少量优化措施之一(代码优化几乎都在即时编译器中进行)。
引用的值在程序编译期是无法确定的，编译器无法对其进行优化。
### Java基础常见面试题总结（下）
#### 异常
##### Exception 和 Error 有什么区别？
在 Java 中，所有的异常都有一个共同的祖先 java.lang 包中的 Throwable 类。Throwable 类有两个重要的子类:
- Exception :程序本身可以处理的异常，可以通过 catch 来进行捕获。Exception 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。
- Error：Error 属于程序无法处理的错误 ，我们没办法通过 catch 来进行捕获不建议通过catch捕获 。例如 Java 虚拟机运行错误（Virtual MachineError）、虚拟机内存不够错误(OutOfMemoryError)、类定义错误（NoClassDefFoundError）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。
##### Checked Exception 和 Unchecked Exception 有什么区别？
Checked Exception 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 catch或者throws 关键字处理的话，就没办法通过编译。
除了RuntimeException
Unchecked Exception 即 不受检查异常 ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。
RuntimeException 及其子类都统称为非受检查异常，常见的有（建议记下来，日常开发中会经常用到）：
##### try-catch-finally 如何使用？
注意：不要在 finally 语句块中使用 return! 当 try 语句和 finally 语句中都有 return 语句时，try 语句块中的 return 语句会被忽略。这是因为 try 语句中的 return 返回值会先被暂存在一个本地变量中，当执行到 finally 语句中的 return 之后，这个本地变量的值就变为了 finally 语句中的 return 返回值。
##### finally 中的代码一定会执行吗？
在以下 2 种特殊情况下，finally 块的代码也不会被执行：
- 程序所在的线程死亡。
- 关闭 CPU。
##### 如何使用 try-with-resources 代替try-catch-finally？
```
try (BufferedInputStream bin = new BufferedInputStream(new FileInputStream(new File("test.txt")));
     BufferedOutputStream bout = new BufferedOutputStream(new FileOutputStream(new File("out.txt")))) {
    int b;
    while ((b = bin.read()) != -1) {
        bout.write(b);
    }
}
catch (IOException e) {
    e.printStackTrace();
}
```
##### 异常使用有哪些需要注意的地方？
- 要把异常定义为静态变量，因为这样会导致异常栈信息错乱。每次手动抛出异常，我们都需要手动 new 一个异常对象抛出。
- 避免重复记录日志：如果在捕获异常的地方已经记录了足够的信息（包括异常类型、错误信息和堆栈跟踪等），那么在业务代码中再次抛出这个异常时，就不应该再次记录相同的错误信息。重复记录日志会使得日志文件膨胀，并且可能会掩盖问题的实际原因，使得问题更难以追踪和解决。
#### 泛型
##### 泛型的使用方式有哪几种？
泛型一般有三种使用方式:泛型类、泛型接口、泛型方法
注意: public static < E > void printArray( E[] inputArray ) 一般被称为静态泛型方法;在 java 中泛型只是一个占位符，必须在传递类型后才能使用。类在实例化时才能真正的传递类型参数，由于静态方法的加载先于类的实例化，也就是说类中的泛型还没有传递真正的类型参数，静态的方法的加载就已经完成了，所以静态泛型方法是没有办法使用类上声明的泛型的。只能使用自己声明的 <E>
#### 反射
#### 注解
注解本质是一个继承了Annotation 的特殊接口。
```
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {

}

public interface Override extends Annotation{

}
```
##### 注解的解析方法有哪几种？
注解只有被解析之后才会生效，常见的解析方法有两种：
- 编译期直接扫描：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用@Override 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
- 运行期通过反射处理：像框架中自带的注解(比如 Spring 框架的 @Value、@Component)都是通过反射来进行处理的。
#### SPI
##### SPI 和 API 有什么区别？
当实现方提供了接口和实现，我们可以通过调用实现方的接口从而拥有实现方给我们提供的能力，这就是 API ，这种接口和实现都是放在实现方的。
当接口存在于调用方这边时，就是 SPI ，由接口调用方确定接口规则，然后由不同的厂商去根据这个规则对这个接口进行实现，从而提供服务。
##### SPI 的优缺点？
通过 SPI 机制能够大大地提高接口设计的灵活性，但是 SPI 机制也存在一些缺点，比如：
- 需要遍历加载所有的实现类，不能做到按需加载，这样效率还是相对较低的。
- 当多个 ServiceLoader 同时 load 时，会有并发问题。
#### 序列化和反序列化
面向对象中的对象序列化，并不概括之前原始对象所关系的函数。这种过程也称为对象编组（marshalling）。从一系列字节提取数据结构的反向操作，是反序列化（也称为解编组、deserialization、unmarshalling）。
##### 如果有些字段不想进行序列化怎么办？
transient 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 transient 修饰的变量值不会被持久化和恢复。
关于 transient 还有几点注意：
- transient 只能修饰变量，不能修饰类和方法。
- transient 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 int 类型，那么反序列后结果就是 0。
- static 变量因为不属于任何对象(Object)，所以无论有没有 transient 关键字修饰，均不会被序列化。
##### 常见序列化协议有哪些？
比较常用的序列化协议有 Hessian、Kryo、Protobuf、ProtoStuff，这些都是基于二进制的序列化协议。
像 JSON 和 XML 这种属于文本类序列化方式。虽然可读性比较好，但是性能较差，一般不会选择。
##### 为什么不推荐使用 JDK 自带的序列化？
- 不支持跨语言调用 : 如果调用的是其他语言开发的服务的时候就不支持了。
- 性能差：相比于其他序列化框架性能更低，主要原因是序列化之后的字节数组体积较大，导致传输成本加大。
- 存在安全问题：序列化和反序列化本身并不存在问题。但当输入的反序列化的数据可被用户控制，那么攻击者即可通过构造恶意输入，让反序列化产生非预期的对象，在此过程中执行构造的任意代码。
#### I/O
Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。
- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。
##### I/O流为什么要分为字节流和字符流呢?
问题本质想问：不管是文件读写还是网络发送接收，信息的最小存储单元都是字节，那为什么 I/O 流操作要分为字节流操作和字符流操作呢？
个人认为主要有两点原因：
- 字符流是由 Java 虚拟机将字节转换得到的，这个过程还算是比较耗时；
- 如果我们不知道编码类型的话，使用字节流的过程中很容易出现乱码问题。
#### 语法糖
不过，JVM 其实并不能识别语法糖，Java 语法糖要想被正确执行，需要先通过编译器进行解糖，也就是在程序编译阶段将其转换成 JVM 认识的基本语法。这也侧面说明，Java 中真正支持语法糖的是 Java 编译器而不是 JVM。如果你去看com.sun.tools.javac.main.JavaCompiler的源码，你会发现在compile()中有一个步骤就是调用desugar()，这个方法就是负责解语法糖的实现的。
### 重要知识点
#### Java值传递详解
#### Java序列化详解
##### JDK 自带的序列化方式
###### serialVersionUID 有什么作用？
序列化号 serialVersionUID 属于版本控制的作用。反序列化时，会检查 serialVersionUID 是否和当前类的 serialVersionUID 一致。如果 serialVersionUID 不一致则会抛出 InvalidClassException 异常。强烈推荐每个序列化类都手动指定其 serialVersionUID，如果不手动指定，那么编译器会动态生成默认的 serialVersionUID。
###### serialVersionUID 不是被 static 变量修饰了吗？为什么还会被“序列化”？
static 修饰的变量是静态变量，位于方法区，本身是不会被序列化的。但是，serialVersionUID 的序列化做了特殊处理，在序列化时，会将 serialVersionUID 序列化到二进制字节流中；在反序列化时，也会解析它并做一致性判断。
也就是说，serialVersionUID 只是用来被 JVM 识别，实际并没有被序列化。
##### Kryo
由于其变长存储特性并使用了字节码生成机制，拥有较高的运行速度和较小的字节码体积。
Kryo 已经是一种非常成熟的序列化实现了，已经在 Twitter、Groupon、Yahoo 以及多个著名开源项目（如 Hive、Storm）中广泛的使用。
```
/**
 * Kryo serialization class, Kryo serialization efficiency is very high, but only compatible with Java language
 *
 * @author shuang.kou
 * @createTime 2020年05月13日 19:29:00
 */
@Slf4j
public class KryoSerializer implements Serializer {

    /**
     * Because Kryo is not thread safe. So, use ThreadLocal to store Kryo objects
     */
    private final ThreadLocal<Kryo> kryoThreadLocal = ThreadLocal.withInitial(() -> {
        Kryo kryo = new Kryo();
        kryo.register(RpcResponse.class);
        kryo.register(RpcRequest.class);
        return kryo;
    });

    @Override
    public byte[] serialize(Object obj) {
        try (ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
             Output output = new Output(byteArrayOutputStream)) {
            Kryo kryo = kryoThreadLocal.get();
            // Object->byte:将对象序列化为byte数组
            kryo.writeObject(output, obj);
            kryoThreadLocal.remove();
            return output.toBytes();
        } catch (Exception e) {
            throw new SerializeException("Serialization failed");
        }
    }

    @Override
    public <T> T deserialize(byte[] bytes, Class<T> clazz) {
        try (ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(bytes);
             Input input = new Input(byteArrayInputStream)) {
            Kryo kryo = kryoThreadLocal.get();
            // byte->Object:从byte数组中反序列化出对象
            Object o = kryo.readObject(input, clazz);
            kryoThreadLocal.remove();
            return clazz.cast(o);
        } catch (Exception e) {
            throw new SerializeException("Deserialization failed");
        }
    }

}
```
##### Protobuf
Protobuf 出自于 Google，性能还比较优秀，也支持多种语言，同时还是跨平台的。就是在使用中过于繁琐，因为你需要自己定义 IDL 文件和生成对应的序列化代码。这样虽然不灵活，但是，另一方面导致 protobuf 没有序列化漏洞的风险。
##### ProtoStuff
##### Hessian
Hessian 是一个轻量级的，自定义描述的二进制 RPC 协议。Hessian 是一个比较老的序列化实现了，并且同样也是跨语言的。
Dubbo2.x 默认启用的序列化方式是 Hessian2 ,但是，Dubbo 对 Hessian2 进行了修改，不过大体结构还是差不多。
#### 范型&通配符详解
// TODO MISS
#### Java反射机制详解
##### 反射实战
###### 获取Class对象的四种方式
- 知道具体类的情况下可以使用
```
Class alunbarClass = TargetObject.class;
```
- 通过 Class.forName()传入类的全路径获取
```
Class alunbarClass1 = Class.forName("cn.javaguide.TargetObject");
```
- 通过对象实例instance.getClass()获取
```
TargetObject o = new TargetObject();
Class alunbarClass2 = o.getClass();
```
- 通过类加载器xxxClassLoader.loadClass()传入类路径获取
```
ClassLoader.getSystemClassLoader().loadClass("cn.javaguide.TargetObject");
```
通过类加载器获取 Class 对象不会进行初始化，意味着不进行包括初始化等一系列步骤，静态代码块和静态对象不会得到执行
###### 反射的一些基本操作
```
package cn.javaguide;

import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InstantiationException, InvocationTargetException, NoSuchFieldException {
        /**
         * 获取 TargetObject 类的 Class 对象并且创建 TargetObject 类实例
         */
        Class<?> targetClass = Class.forName("cn.javaguide.TargetObject");
        TargetObject targetObject = (TargetObject) targetClass.newInstance();
        /**
         * 获取 TargetObject 类中定义的所有方法
         */
        Method[] methods = targetClass.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println(method.getName());
        }

        /**
         * 获取指定方法并调用
         */
        Method publicMethod = targetClass.getDeclaredMethod("publicMethod",
                String.class);

        publicMethod.invoke(targetObject, "JavaGuide");

        /**
         * 获取指定参数并对参数进行修改
         */
        Field field = targetClass.getDeclaredField("value");
        //为了对类中的参数进行修改我们取消安全检查
        field.setAccessible(true);
        field.set(targetObject, "JavaGuide");

        /**
         * 调用 private 方法
         */
        Method privateMethod = targetClass.getDeclaredMethod("privateMethod");
        //为了调用private方法我们取消安全检查
        privateMethod.setAccessible(true);
        privateMethod.invoke(targetObject);
    }
}
```
#### Java代理模式详解
##### 静态代理
静态代理中，我们对目标对象的每个方法的增强都是手动完成的（后面会具体演示代码），非常不灵活（比如接口一旦新增加方法，目标对象和代理对象都要进行修改）且麻烦(需要对每个目标类都单独写一个代理类）。 实际应用场景非常非常少，日常开发几乎看不到使用静态代理的场景。
上面我们是从实现和应用角度来说的静态代理，从 JVM 层面来说， 静态代理在编译时就将接口、实现类、代理类这些都变成了一个个实际的 class 文件。
静态代理实现步骤:
- 定义一个接口及其实现类；
- 创建一个代理类同样实现这个接口
- 将目标对象注入进代理类，然后在代理类的对应方法调用目标类中的对应方法。这样的话，我们就可以通过代理类屏蔽对目标对象的访问，并且可以在目标方法执行前后做一些自己想做的事情。
```
public class SmsProxy implements SmsService {

    private final SmsService smsService;

    public SmsProxy(SmsService smsService) {
        this.smsService = smsService;
    }

    @Override
    public String send(String message) {
        //调用方法之前，我们可以添加自己的操作
        System.out.println("before method send()");
        smsService.send(message);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method send()");
        return null;
    }
}
```
- 实际使用
```
public class Main {
    public static void main(String[] args) {
        SmsService smsService = new SmsServiceImpl();
        SmsProxy smsProxy = new SmsProxy(smsService);
        smsProxy.send("java");
    }
}
```
##### 动态代理
从 JVM 角度来说，动态代理是在运行时动态生成类字节码，并加载到 JVM 中的。
Java 来说，动态代理的实现方式有很多种，比如 JDK 动态代理、CGLIB 动态代理等等。
###### JDK 动态代理机制
在 Java 动态代理机制中 InvocationHandler 接口和 Proxy 类是核心。
Proxy 类中使用频率最高的方法是：newProxyInstance() ，这个方法主要用来生成一个代理对象。
```
    public static Object newProxyInstance(ClassLoader loader,
                                          Class<?>[] interfaces,
                                          InvocationHandler h)
        throws IllegalArgumentException
    {
        ......
    }
```
这个方法一共有 3 个参数：
- loader :类加载器，用于加载代理对象。
- interfaces : 被代理类实现的一些接口；
- h : 实现了 InvocationHandler 接口的对象；
要实现动态代理的话，还必须需要实现InvocationHandler 来自定义处理逻辑。 当我们的动态代理对象调用一个方法时，这个方法的调用就会被转发到实现InvocationHandler 接口类的 invoke 方法来调用。
```
public interface InvocationHandler {

    /**
     * 当你使用代理对象调用方法的时候实际会调用到这个方法
     */
    public Object invoke(Object proxy, Method method, Object[] args)
        throws Throwable;
}
```
invoke() 方法有下面三个参数：
- proxy :动态生成的代理类
- method : 与代理类对象调用的方法相对应
- args : 当前 method 方法的参数
JDK 动态代理类使用步骤
- 定义一个接口及其实现类；
- 自定义 InvocationHandler 并重写invoke方法，在 invoke 方法中我们会调用原生方法（被代理类的方法）并自定义一些处理逻辑；
- 通过 Proxy.newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h) 方法创建代理对象；
```
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * @author shuang.kou
 * @createTime 2020年05月11日 11:23:00
 */
public class DebugInvocationHandler implements InvocationHandler {
    /**
     * 代理类中的真实对象
     */
    private final Object target;

    public DebugInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws InvocationTargetException, IllegalAccessException {
        //调用方法之前，我们可以添加自己的操作
        System.out.println("before method " + method.getName());
        Object result = method.invoke(target, args);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method " + method.getName());
        return result;
    }
}

public class JdkProxyFactory {
    public static Object getProxy(Object target) {
        return Proxy.newProxyInstance(
                target.getClass().getClassLoader(), // 目标类的类加载器
                target.getClass().getInterfaces(),  // 代理需要实现的接口，可指定多个
                new DebugInvocationHandler(target)   // 代理对象对应的自定义 InvocationHandler
        );
    }
}

SmsService smsService = (SmsService) JdkProxyFactory.getProxy(new SmsServiceImpl());
smsService.send("java");

```
###### CGLIB 动态代理机制
JDK 动态代理有一个最致命的问题是其只能代理实现了接口的类。
CGLIB(Code Generation Library)是一个基于ASM的字节码生成库，它允许我们在运行时对字节码进行修改和动态生成。CGLIB 通过继承方式实现代理。
在 CGLIB 动态代理机制中 MethodInterceptor 接口和 Enhancer 类是核心。
需要自定义 MethodInterceptor 并重写 intercept 方法，intercept 用于拦截增强被代理类的方法。
```
public interface MethodInterceptor
extends Callback{
    // 拦截被代理类中的方法
    public Object intercept(Object obj, java.lang.reflect.Method method, Object[] args,MethodProxy proxy) throws Throwable;
}
```
- obj : 被代理的对象（需要增强的对象）
- method : 被拦截的方法（需要增强的方法）
- args : 方法入参
- proxy : 用于调用原始方法
你可以通过 Enhancer类来动态获取被代理类，当代理类调用方法的时候，实际调用的是 MethodInterceptor 中的 intercept 方法。
CGLIB 动态代理类使用步骤
- 定义一个类；
- 自定义 MethodInterceptor 并重写 intercept 方法，intercept 用于拦截增强被代理类的方法，和 JDK 动态代理中的 invoke 方法类似；
- 通过 Enhancer 类的 create()创建代理类；
```
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

/**
 * 自定义MethodInterceptor
 */
public class DebugMethodInterceptor implements MethodInterceptor {


    /**
     * @param o           被代理的对象（需要增强的对象）
     * @param method      被拦截的方法（需要增强的方法）
     * @param args        方法入参
     * @param methodProxy 用于调用原始方法
     */
    @Override
    public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        //调用方法之前，我们可以添加自己的操作
        System.out.println("before method " + method.getName());
        Object object = methodProxy.invokeSuper(o, args);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method " + method.getName());
        return object;
    }

}

import net.sf.cglib.proxy.Enhancer;

public class CglibProxyFactory {

    public static Object getProxy(Class<?> clazz) {
        // 创建动态代理增强类
        Enhancer enhancer = new Enhancer();
        // 设置类加载器
        enhancer.setClassLoader(clazz.getClassLoader());
        // 设置被代理类
        enhancer.setSuperclass(clazz);
        // 设置方法拦截器
        enhancer.setCallback(new DebugMethodInterceptor());
        // 创建代理类
        return enhancer.create();
    }
}

AliSmsService aliSmsService = (AliSmsService) CglibProxyFactory.getProxy(AliSmsService.class);
aliSmsService.send("java");
```
###### JDK 动态代理和 CGLIB 动态代理对比
- JDK 动态代理只能代理实现了接口的类或者直接代理接口，而 CGLIB 可以代理未实现任何接口的类。 另外， CGLIB 动态代理是通过生成一个被代理类的子类来拦截被代理类的方法调用，因此不能代理声明为 final 类型的类和方法。
- 就二者的效率来说，大部分情况都是 JDK 动态代理更优秀，随着 JDK 版本的升级，这个优势更加明显。 
##### 静态代理和动态代理的对比
- 灵活性：动态代理更加灵活，不需要必须实现接口，可以直接代理实现类，并且可以不需要针对每个目标类都创建一个代理类。另外，静态代理中，接口一旦新增加方法，目标对象和代理对象都要进行修改，这是非常麻烦的！
- JVM 层面：静态代理在编译时就将接口、实现类、代理类这些都变成了一个个实际的 class 文件。而动态代理是在运行时动态生成类字节码，并加载到 JVM 中的。
#### BigDecimal 详解
##### BigDecimal 介绍
浮点数之间的等值判断，基本数据类型不能用 == 来比较，包装数据类型不能用 equals 来判断。
##### BigDecimal 常见方法
RoundingMode 不要选择 UNNECESSARY，否则很可能会遇到 ArithmeticException（无法除尽出现无限循环小数的时候）
##### BigDecimal 等值比较问题
equals() 方法不仅仅会比较值的大小（value）还会比较精度（scale），而 compareTo() 方法比较的时候会忽略精度。1.0 的 scale 是 1，1 的 scale 是 0，因此 a.equals(b) 的结果是 false。
建议对保留规则设置为 RoundingMode.HALF_EVEN,即四舍六入五成双。
#### Java 魔法类 Unsafe 详解
##### Unsafe 介绍
Unsafe 提供的这些功能的实现需要依赖本地方法（Native Method）。你可以将本地方法看作是 Java 中使用其他编程语言编写的方法。本地方法使用 native 关键字修饰，Java 代码中只是声明方法头，具体的实现则交给 本地代码。
###### 什么要使用本地方法呢？
- 需要用到 Java 中不具备的依赖于操作系统的特性，Java 在实现跨平台的同时要实现对底层的控制，需要借助其他语言发挥作用。
- 对于其他语言已经完成的一些现成功能，可以使用 Java 直接调用。程序对时间敏感或对性能要求非常高时，有必要使用更加底层的语言，例如 C/C++甚至是汇编。
在 JUC 包的很多并发工具类在实现并发机制时，都调用了本地方法，通过它们打破了 Java 运行时的界限，能够接触到操作系统底层的某些功能。对于同一本地方法，不同的操作系统可能会通过不同的方式来实现，但是对于使用者来说是透明的，最终都会得到相同的结果。
##### Unsafe 创建
Unsafe 类为一单例实现，提供静态方法 getUnsafe 获取 Unsafe实例。这个看上去貌似可以用来获取 Unsafe 实例。但是，当我们直接调用这个静态方法的时候，会抛出 SecurityException 异常
###### 为什么 public static 方法无法被直接调用呢？
这是因为在getUnsafe方法中，会对调用者的classLoader进行检查，判断当前类是否由Bootstrap classLoader加载，如果不是的话那么就会抛出一个SecurityException异常。也就是说，只有启动类加载器加载的类才能够调用 Unsafe 类中的方法，来防止这些方法在不可信的代码中被调用。
###### 如若想使用 Unsafe 这个类的话，应该如何获取其实例呢？
这里介绍两个可行的方案。
- 利用反射获得 Unsafe 类中已经实例化完成的单例对象 theUnsafe 。
```
private static Unsafe reflectGetUnsafe() {
    try {
      Field field = Unsafe.class.getDeclaredField("theUnsafe");
      field.setAccessible(true);
      return (Unsafe) field.get(null);
    } catch (Exception e) {
      log.error(e.getMessage(), e);
      return null;
    }
}
```
- 从getUnsafe方法的使用限制条件出发，通过 Java 命令行命令-Xbootclasspath/a把调用 Unsafe 相关方法的类 A 所在 jar 包路径追加到默认的 bootstrap 路径中，使得 A 被引导类加载器加载，从而通过Unsafe.getUnsafe方法安全的获取 Unsafe 实例。
java -Xbootclasspath/a: ${path}   // 其中path为调用Unsafe相关方法的类所在jar包路径
##### Unsafe 功能
概括的来说，Unsafe 类实现功能可以被分为下面 8 类：
- 内存操作
- 内存屏障
- 对象操作
- 数据操作
- CAS 操作
- 线程调度
- Class 操作
- 系统信息
###### 内存操作
需要注意，通过这种方式分配的内存属于 堆外内存 ，是无法进行垃圾回收的，需要我们把这些内存当做一种资源去手动调用freeMemory方法进行释放，否则会产生内存泄漏。通用的操作内存方式是在try中执行对内存的操作，最终在finally块中进行内存的释放。
####### 为什么要使用堆外内存？
- 对垃圾回收停顿的改善。由于堆外内存是直接受操作系统管理而不是 JVM，所以当我们使用堆外内存时，即可保持较小的堆内内存规模。从而在 GC 时减少回收停顿对于应用的影响。
- 提升程序 I/O 操作的性能。通常在 I/O 通信过程中，会存在堆内内存到堆外内存的数据拷贝操作，对于需要频繁进行内存间数据拷贝且生命周期较短的暂存数据，都建议存储到堆外内存。
####### 典型应用
DirectByteBuffer 是 Java 用于实现堆外内存的一个重要类，通常用在通信过程中做缓冲池，如在 Netty、MINA 等 NIO 框架中应用广泛。DirectByteBuffer 对于堆外内存的创建、使用、销毁等逻辑均由 Unsafe 提供的堆外内存 API 来实现。
###### 内存屏障
在硬件层面上，内存屏障是 CPU 为了防止代码进行重排序而提供的指令，不同的硬件平台上实现内存屏障的方法可能并不相同。在 Java8 中，引入了 3 个内存屏障的函数，它屏蔽了操作系统底层的差异，允许在代码中定义、并统一由 JVM 来生成内存屏障指令，来实现内存屏障的功能。
####### 典型应用
在 Java 8 中引入了一种锁的新机制——StampedLock，它可以看成是读写锁的一个改进版本。StampedLock 提供了一种乐观读锁的实现，这种乐观读锁类似于无锁的操作，完全不会阻塞写线程获取写锁，从而缓解读多写少时写线程“饥饿”现象。由于 StampedLock 提供的乐观读锁不阻塞写线程获取读锁，当线程共享变量从主内存 load 到线程工作内存时，会存在数据不一致问题。
为了解决这个问题，StampedLock 的 validate 方法会通过 Unsafe 的 loadFence 方法加入一个 load 内存屏障。
###### 对象操作
####### 对象属性
对象成员属性的内存偏移量获取，以及字段属性值的修改，在上面的例子中我们已经测试过了。除了前面的putInt、getInt方法外，Unsafe 提供了全部 8 种基础数据类型以及Object的put和get方法，并且所有的put方法都可以越过访问权限，直接修改内存中的数据。阅读 openJDK 源码中的注释发现，基础数据类型和Object的读写稍有不同，基础数据类型是直接操作的属性值（value），而Object的操作则是基于引用值（reference value）
相对于普通读写来说，volatile读写具有更高的成本，因为它需要保证可见性和有序性。
有序写入的成本相对volatile较低，因为它只保证写入时的有序性，而不保证可见性，也就是一个线程写入的值不能保证其他线程立即可见。
顺序写入与volatile写入的差别在于，在顺序写时加入的内存屏障类型为StoreStore类型，而在volatile写入时加入的内存屏障是StoreLoad类型。
在有序写入方法中，使用的是StoreStore屏障，该屏障确保Store1立刻刷新数据到内存，这一操作先于Store2以及后续的存储指令操作。而在volatile写入中，使用的是StoreLoad屏障，该屏障确保Store1立刻刷新数据到内存，这一操作先于Load2及后续的装载指令，并且，StoreLoad屏障会使该屏障之前的所有内存访问指令，包括存储指令和访问指令全部完成之后，才执行该屏障之后的内存访问指令。
####### 对象实例化
使用 Unsafe 的 allocateInstance 方法，允许我们使用非常规的方式进行对象的实例化，首先定义一个实体类，并且在构造函数中对其成员变量进行赋值操作。
通过allocateInstance方法创建对象过程中，不会调用类的构造方法。使用这种方式创建对象时，只用到了Class对象，所以说如果想要跳过对象的初始化阶段或者跳过构造器的安全检查，就可以使用这种方法。
如果将 A 类的构造函数改为private类型，将无法通过构造函数和反射创建对象（可以通过构造函数对象 setAccessible 后创建对象），但allocateInstance方法仍然有效。
###### 数组操作
###### CAS 操作
CAS 是一条 CPU 的原子指令（cmpxchg 指令），不会造成所谓的数据不一致问题，Unsafe 提供的 CAS 方法（如 compareAndSwapXXX）底层实现即为 CPU 指令 cmpxchg 。
在调用compareAndSwapInt方法后，会直接返回true或false的修改结果，因此需要我们在代码中手动添加自旋的逻辑。在AtomicInteger类的设计中，也是采用了将compareAndSwapInt的结果作为循环条件，直至修改成功才退出死循环的方式来实现的原子性的自增操作。
###### 线程调度
###### Class 操作
###### 系统信息
#### Java SPI 机制详解
为了实现在模块装配的时候不用在程序里面动态指明，这就需要一种服务发现机制。Java SPI 就是提供了这样一个机制：为某个接口寻找服务实现的机制。这有点类似 IoC 的思想，将装配的控制权移交到了程序之外。
##### SPI 介绍
###### 何谓 SPI？
SPI 即 Service Provider Interface ，字面意思就是：“服务提供者的接口”，我的理解是：专门提供给服务提供者或者扩展框架功能的开发者去使用的一个接口。
SPI 将服务接口和具体的服务实现分离开来，将服务调用方和服务实现者解耦，能够提升程序的扩展性、可维护性。修改或者替换服务实现并不需要修改调用方。
##### 实战演示
```
package edu.jiangxuan.up.spi;

import java.util.ArrayList;
import java.util.List;
import java.util.ServiceLoader;

public class LoggerService {
    private static final LoggerService SERVICE = new LoggerService();

    private final Logger logger;

    private final List<Logger> loggerList;

    private LoggerService() {
        ServiceLoader<Logger> loader = ServiceLoader.load(Logger.class);
        List<Logger> list = new ArrayList<>();
        for (Logger log : loader) {
            list.add(log);
        }
        // LoggerList 是所有 ServiceProvider
        loggerList = list;
        if (!list.isEmpty()) {
            // Logger 只取一个
            logger = list.get(0);
        } else {
            logger = null;
        }
    }

    public static LoggerService getService() {
        return SERVICE;
    }

    public void info(String msg) {
        if (logger == null) {
            System.out.println("info 中没有发现 Logger 服务提供者");
        } else {
            logger.info(msg);
        }
    }

    public void debug(String msg) {
        if (loggerList.isEmpty()) {
            System.out.println("debug 中没有发现 Logger 服务提供者");
        }
        loggerList.forEach(log -> log.debug(msg));
    }
}

```

实现 Logger 接口，在 src 目录下新建 META-INF/services 文件夹，然后新建文件 edu.jiangxuan.up.spi.Logger （SPI 的全类名），文件里面的内容是：edu.jiangxuan.up.spi.service.Logback （Logback 的全类名，即 SPI 的实现类的包名 + 类名）。
这是 JDK SPI 机制 ServiceLoader 约定好的标准。
这里先大概解释一下：Java 中的 SPI 机制就是在每次类加载的时候会先去找到 class 相对目录下的 META-INF 文件夹下的 services 文件夹下的文件，将这个文件夹下面的所有文件先加载到内存中，然后根据这些文件的文件名和里面的文件内容找到相应接口的具体实现类，找到实现类后就可以通过反射去生成对应的对象，保存在一个 list 列表里面，所以可以通过迭代或者遍历的方式拿到对应的实例对象，生成不同的实现。
所以会提出一些规范要求：文件名一定要是接口的全类名，然后里面的内容一定要是实现类的全类名，实现类可以有多个，直接换行就好了，多个实现类的时候，会一个一个的迭代加载。
##### ServiceLoader 
ServiceLoader 实现了 Iterable 接口的方法后，具有了迭代的能力，在这个 iterator 方法被调用时，首先会在 ServiceLoader 的 Provider 缓存中进行查找，如果缓存中没有命中那么则在 LazyIterator 中进行查找。
###### 自己实现一个 ServiceLoader
```
package edu.jiangxuan.up.service;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.lang.reflect.Constructor;
import java.net.URL;
import java.net.URLConnection;
import java.util.ArrayList;
import java.util.Enumeration;
import java.util.List;

public class MyServiceLoader<S> {

    // 对应的接口 Class 模板
    private final Class<S> service;

    // 对应实现类的 可以有多个，用 List 进行封装
    private final List<S> providers = new ArrayList<>();

    // 类加载器
    private final ClassLoader classLoader;

    // 暴露给外部使用的方法，通过调用这个方法可以开始加载自己定制的实现流程。
    public static <S> MyServiceLoader<S> load(Class<S> service) {
        return new MyServiceLoader<>(service);
    }

    // 构造方法私有化
    private MyServiceLoader(Class<S> service) {
        this.service = service;
        this.classLoader = Thread.currentThread().getContextClassLoader();
        doLoad();
    }

    // 关键方法，加载具体实现类的逻辑
    private void doLoad() {
        try {
            // 读取所有 jar 包里面 META-INF/services 包下面的文件，这个文件名就是接口名，然后文件里面的内容就是具体的实现类的路径加全类名
            Enumeration<URL> urls = classLoader.getResources("META-INF/services/" + service.getName());
            // 挨个遍历取到的文件
            while (urls.hasMoreElements()) {
                // 取出当前的文件
                URL url = urls.nextElement();
                System.out.println("File = " + url.getPath());
                // 建立链接
                URLConnection urlConnection = url.openConnection();
                urlConnection.setUseCaches(false);
                // 获取文件输入流
                InputStream inputStream = urlConnection.getInputStream();
                // 从文件输入流获取缓存
                BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
                // 从文件内容里面得到实现类的全类名
                String className = bufferedReader.readLine();

                while (className != null) {
                    // 通过反射拿到实现类的实例
                    Class<?> clazz = Class.forName(className, false, classLoader);
                    // 如果声明的接口跟这个具体的实现类是属于同一类型，（可以理解为Java的一种多态，接口跟实现类、父类和子类等等这种关系。）则构造实例
                    if (service.isAssignableFrom(clazz)) {
                        Constructor<? extends S> constructor = (Constructor<? extends S>) clazz.getConstructor();
                        S instance = constructor.newInstance();
                        // 把当前构造的实例对象添加到 Provider的列表里面
                        providers.add(instance);
                    }
                    // 继续读取下一行的实现类，可以有多个实现类，只需要换行就可以了。
                    className = bufferedReader.readLine();
                }
            }
        } catch (Exception e) {
            System.out.println("读取文件异常。。。");
        }
    }

    // 返回spi接口对应的具体实现类列表
    public List<S> getProviders() {
        return providers;
    }
}

```
#### Java 语法糖详解
##### switch 支持 String 与 枚举
在开始之前先科普下，Java 中的switch自身原本就支持基本类型。比如int、char等。对于int类型，直接进行数值的比较。对于char类型则是比较其 ascii 码。所以，对于编译器来说，switch中其实只能使用整型，任何类型的比较都要转换成整型。比如byte。short，char(ascii 码是整型)以及int。
看到这个代码，你知道原来 字符串的 switch 是通过equals()和hashCode()方法来实现的。 还好hashCode()方法返回的是int，而不是long。
仔细看下可以发现，进行switch的实际是哈希值，然后通过使用equals方法比较进行安全检查，这个检查是必要的，因为哈希可能会发生碰撞。因此它的性能是不如使用枚举进行 switch 或者使用纯整数常量，但这也不是很差。
##### 泛型
通常情况下，一个编译器处理泛型有两种方式：Code specialization和Code sharing。C++和 C#是使用Code specialization的处理机制，而 Java 使用的是Code sharing的机制。
Code sharing 方式为每个泛型类型创建唯一的字节码表示，并且将该泛型类型的实例都映射到这个唯一的字节码表示上。将多种泛型类形实例映射到唯一的字节码表示是通过类型擦除（type erasue）实现的。
也就是说，对于 Java 虚拟机来说，他根本不认识Map<String, String> map这样的语法。需要在编译阶段通过类型擦除的方式进行解语法糖。
类型擦除的主要过程如下：1.将所有的泛型参数用其最左边界（最顶级的父类型）类型替换。 2.移除所有的类型参数。
虚拟机中没有泛型，只有普通类和普通方法，所有泛型类的类型参数在编译时都会被擦除，泛型类并没有自己独有的Class类对象。比如并不存在List<String>.class或是List<Integer>.class，而只有List.class。
##### 自动装箱与拆箱
装箱过程是通过调用包装器的 valueOf 方法实现的，而拆箱过程是通过调用包装器的 xxxValue 方法实现的。
##### 可变长参数
##### 枚举
当我们使用enum来定义一个枚举类型的时候，编译器会自动帮我们创建一个final类型的类继承Enum类，所以枚举类型不能被继承。
##### 内部类
内部类之所以也是语法糖，是因为它仅仅是一个编译时的概念，outer.java里面定义了一个内部类inner，一旦编译成功，就会生成两个完全不同的.class文件了，分别是outer.class和outer$inner.class。所以内部类的名字完全可以和它的外部类名字相同。
当我们尝试对OutterClass.class文件进行反编译的时候，命令行会打印以下内容：Parsing OutterClass.class...Parsing inner class OutterClass$InnerClass.class... Generating OutterClass.jad 。他会把两个文件全部进行反编译，然后一起生成一个OutterClass.jad文件。
##### 条件编译
—般情况下，程序中的每一行代码都要参加编译。但有时候出于对程序代码优化的考虑，希望只对其中一部分内容进行编译，此时就需要在程序中加上条件，让编译器只对满足条件的代码进行编译，将不满足条件的代码舍弃，这就是条件编译。
如在 C 或 CPP 中，可以通过预处理语句来实现条件编译。其实在 Java 中也可实现条件编译。
Java 语法的条件编译，是通过判断条件为常量的 if 语句实现的。其原理也是 Java 语言的语法糖。根据 if 判断条件的真假，编译器直接把分支为 false 的代码块消除。通过该方式实现的条件编译，必须在方法体内实现，而无法在正整个 Java 类的结构或者类的属性上进行条件编译，这与 C/C++的条件编译相比，确实更有局限性。在 Java 语言设计之初并没有引入条件编译的功能，虽有局限，但是总比没有更强。
##### 断言
其实断言的底层实现就是 if 语言，如果断言结果为 true，则什么都不做，程序继续执行，如果断言结果为 false，则程序抛出 AssertError 来打断程序的执行。-enableassertions会设置$assertionsDisabled 字段的值。
##### 数值字面量
在 java 7 中，数值字面量，不管是整数还是浮点数，都允许在数字之间插入任意多个下划线。这些下划线不会对字面量的数值产生影响，目的就是方便阅读。
反编译后就是把_删除了。也就是说 编译器并不认识在数字字面量中的_，需要在编译阶段把他去掉。
##### for-each
or-each 的实现原理其实就是使用了普通的 for 循环和迭代器。
##### try-with-resource
##### Lambda 表达式
为啥说他并不是内部类的语法糖呢，前面讲内部类我们说过，内部类在编译之后会有两个 class 文件，但是，包含 lambda 表达式的类编译后只有一个文件。
lambda 表达式的实现其实是依赖了一些底层的 api，在编译阶段，编译器会把 lambda 表达式进行解糖，转换成调用内部 api 的方式。
##### 可能遇到的坑
###### 泛型
####### 当泛型遇到重载
####### 当泛型遇到 catch
####### 当泛型内包含静态变量
有些同学可能会误认为泛型类是不同的类，对应不同的字节码，其实 由于经过类型擦除，所有的泛型类实例都关联到同一份字节码上，泛型类的静态变量是共享的。上面例子里的GT<Integer>.var和GT<String>.var其实是一个变量。
###### 自动装箱与拆箱
####### 对象相等比较
在 Java 5 中，在 Integer 的操作上引入了一个新功能来节省内存和提高性能。整型对象通过使用相同的对象引用实现了缓存和重用。
（适用于整数值区间-128 至 +127。只适用于自动装箱。使用构造函数创建对象不适用。）
###### 增强 for 循环
Iterator 是工作在一个独立的线程中，并且拥有一个 mutex 锁。 Iterator 被创建之后会建立一个指向原来对象的单链索引表，当原来的对象数量发生变化时，这个索引表的内容不会同步改变，所以当索引指针往后移动的时候就找不到要迭代的对象，所以按照 fail-fast 原则 Iterator 会马上抛出java.util.ConcurrentModificationException异常。
所以 Iterator 在工作的时候是不允许被迭代的对象被改变的。但你可以使用 Iterator 本身的方法remove()来删除对象，Iterator.remove() 方法会在删除当前迭代对象的同时维护索引的一致性。
## 集合
### Java 集合常见面试题总结(上)
#### 集合概述
##### Java 集合概览
1. Collection
1.1 List
1.1.1 ArrayList: Object[]
1.1.2 Vector: Object[]
1.1.3 LinkedList: 双向链表
1.2 Set
1.2.1 HashSet(无序，唯一): 基于HashMap实现的
1.2.2 LinkedHashSet: 通过LinkedHashMap实现的
1.2.3 TreeSet(有序，唯一): 红黑树（自平衡排序二叉树）
1.3 Queue
1.3.1 PriorityQueue: Object[] 小顶堆
1.3.2 DelayQueue: PriorityQueue
1.3.3 ArrayDeque: 可扩容动态双向数组
2. Map
2.1 HashMap: JDK1.8 之前 HashMap 由数组+链表组成的。JDK1.8 以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。
2.2 LinkedHashMap: LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。
2.3 HashTable: 数组+链表组成的，数组是 Hashtable 的主体，链表则是主要为了解决哈希冲突而存在的。
2.4 TreeMap: 红黑树（自平衡的排序二叉树）。
##### List
###### ArrayList 和 Array（数组）的区别？
ArrayList 内部基于动态数组实现，比 Array（静态数组） 使用起来更加灵活:
- ArrayList会根据实际存储的元素动态地扩容或缩容，而 Array 被创建之后就不能改变它的长度了。
- ArrayList 允许你使用泛型来确保类型安全，Array 则不可以。
- ArrayList 中只能存储对象。对于基本类型数据，需要使用其对应的包装类（如 Integer、Double 等）。Array 可以直接存储基本类型数据，也可以存储对象。
- ArrayList 支持插入、删除、遍历等常见操作，并且提供了丰富的 API 操作方法，比如 add()、remove()等。Array 只是一个固定长度的数组，只能按照下标访问其中的元素，不具备动态添加、删除元素的能力。ArrayList创建时不需要指定大小，而Array创建时必须指定大小。
###### ArrayList 和 Vector 的区别?（了解即可）
- ArrayList 是 List 的主要实现类，底层使用 Object[]存储，适用于频繁的查找工作，线程不安全 。
- Vector 是 List 的古老实现类，底层使用Object[] 存储，线程安全。
###### Vector 和 Stack 的区别?（了解即可）
- Vector 和 Stack 两者都是线程安全的，都是使用 synchronized 关键字进行同步处理。
- Stack 继承自 Vector，是一个后进先出的栈，而 Vector 是一个列表。
###### ArrayList 与 LinkedList 区别?
- 是否保证线程安全： ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全；
- 底层数据结构： ArrayList 底层使用的是 Object 数组；LinkedList 底层使用的是 双向链表 数据结构（JDK1.6 之前为循环链表，JDK1.7 取消了循环。
- 内存空间占用： ArrayList 的空间浪费主要体现在在 list 列表的结尾会预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）。
###### RandomAccess 接口
RandomAccess 接口不过是一个标识罢了,标识实现这个接口的类具有随机访问功能。
在 binarySearch() 方法中，它要判断传入的 list 是否 RandomAccess 的实例，如果是，调用indexedBinarySearch()方法，如果不是，那么调用iteratorBinarySearch()方法。
RandomAccess 接口只是标识，并不是说 ArrayList 实现 RandomAccess 接口才具有快速随机访问功能的！
##### Set
###### Comparable 和 Comparator 的区别
- Comparable 接口实际上是出自 java.lang 包，它有一个 compareTo(Object obj)方法用来排序
- Comparator 接口实际上是出自 java.util 包，它有一个compare(Object obj1, Object obj2)方法用来排序
###### 无序性和不可重复性的含义是什么
- 无序性不等于随机性 ，无序性是指存储的数据在底层数组中并非按照数组索引的顺序添加 ，而是根据数据的哈希值决定的。
- 不可重复性是指添加的元素按照 equals() 判断时 ，返回 false，需要同时重写 equals() 方法和 hashCode() 方法。
###### 比较 HashSet、LinkedHashSet 和 TreeSet 三者的异同
- HashSet、LinkedHashSet 和 TreeSet 的主要区别在于底层数据结构不同。HashSet 的底层数据结构是哈希表（基于 HashMap 实现）。LinkedHashSet 的底层数据结构是链表和哈希表，元素的插入和取出顺序满足 FIFO。TreeSet 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
- 底层数据结构不同又导致这三者的应用场景不同。HashSet 用于不需要保证元素插入和取出顺序的场景，LinkedHashSet 用于保证元素的插入和取出顺序满足 FIFO 的场景，TreeSet 用于支持对元素自定义排序规则的场景。
##### Queue
###### Queue 与 Deque 的区别
Queue 是单端队列，只能从一端插入元素，另一端删除元素，实现上一般遵循 先进先出（FIFO） 规则。
Queue 扩展了 Collection 的接口，根据 因为容量问题而导致操作失败后处理方式的不同 可以分为两类方法: 
一种在操作失败后会抛出异常（插入 add、删除 remove、查看 element），另一种则会返回特殊值（插入 offer、删除 poll、查看 peek）。
Deque 是双端队列，在队列的两端均可以插入或删除元素。
Deque 扩展了 Queue 的接口, 增加了在队首和队尾进行插入和删除的方法，同样根据失败后处理方式的不同分为两类。
事实上，Deque 还提供有 push() 和 pop() 等其他方法，可用于模拟栈。
###### ArrayDeque 与 LinkedList 的区别
ArrayDeque 和 LinkedList 都实现了 Deque 接口，两者都具有队列的功能，但两者有什么区别呢？
- ArrayDeque 是基于可变长的数组和双指针来实现，而 LinkedList 则通过链表来实现。
- ArrayDeque 不支持存储 NULL 数据，但 LinkedList 支持。
- ArrayDeque 是在 JDK1.6 才被引入的，而LinkedList 早在 JDK1.2 时就已经存在。
- ArrayDeque 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 LinkedList 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。
从性能的角度上，选用 ArrayDeque 来实现队列要比 LinkedList 更好。此外，ArrayDeque 也可以用于实现栈。
###### PriorityQueue
PriorityQueue 是在 JDK1.5 中被引入的, 其与 Queue 的区别在于元素出队顺序是与优先级相关的，即总是优先级最高的元素先出队。这里列举其相关的一些要点：
- PriorityQueue 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据
- PriorityQueue 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素。
- PriorityQueue 是非线程安全的，且不支持存储 NULL 和 non-comparable 的对象。
- PriorityQueue 默认是小顶堆，但可以接收一个 Comparator 作为构造参数，从而来自定义元素优先级的先后。
PriorityQueue 在面试中可能更多的会出现在手撕算法的时候，典型例题包括堆排序、求第 K 大的数、带权图的遍历等，所以需要会熟练使用才行。
###### BlockingQueue
BlockingQueue阻塞的原因是其支持当队列没有元素时一直阻塞，直到有元素；还支持如果队列已满，一直等到队列可以放入新元素时再放入。
BlockingQueue 常用于生产者-消费者模型中，生产者线程会向队列中添加数据，而消费者线程会从队列中取出数据进行处理。
####### BlockingQueue 的实现类有哪些？
- ArrayBlockingQueue：使用数组实现的有界阻塞队列。在创建时需要指定容量大小，并支持公平和非公平两种方式的锁访问机制。
- LinkedBlockingQueue：使用单向链表实现的可选有界阻塞队列。在创建时可以指定容量大小，如果不指定则默认为Integer.MAX_VALUE。和ArrayBlockingQueue不同的是， 它仅支持非公平的锁访问机制。
- PriorityBlockingQueue：支持优先级排序的无界阻塞队列。元素必须实现Comparable接口或者在构造函数中传入Comparator对象，并且不能插入 null 元素。
- SynchronousQueue：同步队列，是一种不存储元素的阻塞队列。每个插入操作都必须等待对应的删除操作，反之删除操作也必须等待插入操作。因此，SynchronousQueue通常用于线程之间的直接传递数据。
- DelayQueue：延迟队列，其中的元素只有到了其指定的延迟时间，才能够从队列中出队。
####### ArrayBlockingQueue 和 LinkedBlockingQueue 有什么区别？
- 底层实现：ArrayBlockingQueue 基于数组实现，而 LinkedBlockingQueue 基于链表实现。
- 是否有界：ArrayBlockingQueue 是有界队列，必须在创建时指定容量大小。LinkedBlockingQueue 创建时可以不指定容量大小，默认是Integer.MAX_VALUE，也就是无界的。但也可以指定队列大小，从而成为有界的。
- 锁是否分离： ArrayBlockingQueue中的锁是没有分离的，即生产和消费用的是同一个锁；LinkedBlockingQueue中的锁是分离的，即生产用的是putLock，消费是takeLock，这样可以防止生产者和消费者线程之间的锁争夺。
- 内存占用：ArrayBlockingQueue 需要提前分配数组内存，而 LinkedBlockingQueue 则是动态分配链表节点内存。这意味着，ArrayBlockingQueue 在创建时就会占用一定的内存空间，且往往申请的内存比实际所用的内存更大，而LinkedBlockingQueue 则是根据元素的增加而逐渐占用内存空间。
### Java 集合常见面试题总结(下)
#### Map (重要)
##### HashMap 和 HashTable的区别
- 线程是否安全： HashMap 是非线程安全的，Hashtable 是线程安全的,因为 Hashtable 内部的方法基本都经过synchronized 修饰。（如果你要保证线程安全的话就使用 ConcurrentHashMap 吧！）；
- 效率： 因为线程安全的问题，HashMap 要比 Hashtable 效率高一点。另外，Hashtable 基本被淘汰，不要在代码中使用它；
- 对 Null key 和 Null value 的支持： HashMap 可以存储 null 的 key 和 value，但 null 作为键只能有一个，null 作为值可以有多个；Hashtable 不允许有 null 键和 null 值，否则会抛出 NullPointerException。
- 初始容量大小和每次扩充容量大小的不同： 
① 创建时如果不指定容量初始值，Hashtable 默认的初始大小为 11，之后每次扩充，容量变为原来的 2n+1。HashMap 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。
② 创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为 2 的幂次方大小（HashMap 中的tableSizeFor()方法保证，下面给出了源代码）。也就是说 HashMap 总是使用 2 的幂作为哈希表的大小。
- 底层数据结构： JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）时，将链表转化为红黑树（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树），以减少搜索时间。Hashtable 没有这样的机制。
##### HashMap 和 HashSet 区别
- HashMap 使用键（Key）计算 hashcode
- HashSet 使用成员对象来计算 hashcode 值，对于两个对象来说 hashcode 可能相同，所以equals()方法用来判断对象的相等性
##### HashMap 和 TreeMap 区别
TreeMap 和HashMap 都继承自AbstractMap ，但是需要注意的是TreeMap它还实现了NavigableMap接口和SortedMap 接口。
实现 NavigableMap 接口让 TreeMap 有了对集合内元素的搜索的能力。
NavigableMap 接口提供了丰富的方法来探索和操作键值对:
- 定向搜索: ceilingEntry(), floorEntry(), higherEntry()和 lowerEntry() 等方法可以用于定位大于、小于、大于等于、小于等于给定键的最接近的键值对。
- 子集操作: subMap(), headMap()和 tailMap() 方法可以高效地创建原集合的子集视图，而无需复制整个集合。
- 逆序视图:descendingMap() 方法返回一个逆序的 NavigableMap 视图，使得可以反向迭代整个 TreeMap。
- 边界操作: firstEntry(), lastEntry(), pollFirstEntry()和 pollLastEntry() 等方法可以方便地访问和移除元素。
这些方法都是基于红黑树数据结构的属性实现的，红黑树保持平衡状态，从而保证了搜索操作的时间复杂度为 O(log n)，这让 TreeMap 成为了处理有序集合搜索问题的强大工具。
实现SortedMap接口让 TreeMap 有了对集合中的元素根据键排序的能力。默认是按 key 的升序排序，不过我们也可以指定排序的比较器。
```
Comparator 接口
@Override
public int compare(Person person1, Person person2) {
    int num = person1.getAge() - person2.getAge();
    return Integer.compare(num, 0);
}
```
##### HashSet 如何检查重复?
在 JDK1.8 中，HashSet的add()方法只是简单的调用了HashMap的put()方法，并且判断了一下返回值以确保是否有重复元素。
##### HashMap 的底层实现
###### JDK 1.8 之前
HashMap 通过 key 的 hashcode 经过扰动函数处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置。
所谓扰动函数指的就是 HashMap 的 hash 方法。使用 hash 方法也就是扰动函数是为了防止一些实现比较差的 hashCode() 方法 换句话说使用扰动函数之后可以减少碰撞。
###### JDK 1.8 之后
JDK1.8 之后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。
##### HashMap 的长度为什么是 2 的幂次方
这个散列值是不能直接拿来用的。用之前还要先做对数组的长度取模运算，得到的余数才能用来要存放的位置也就是对应的数组下标。这个数组下标的计算方法是“ (n - 1) & hash”。（n 代表数组长度）。取余(%)操作中如果除数是 2 的幂次则等价于与其除数减一的与(&)操作（也就是说 hash%length==hash&(length-1)的前提是 length 是 2 的 n 次方。这也就解释了 HashMap 的长度为什么是 2 的幂次方。
##### HashMap 多线程操作导致死循环问题
JDK1.7 及之前版本的 HashMap 在多线程环境下扩容操作可能存在死循环问题，这是由于当一个桶位中有多个元素需要进行扩容时，多个线程同时对链表进行操作，头插法可能会导致链表中的节点指向错误的位置，从而形成一个环形链表，进而使得查询元素的操作陷入死循环无法结束。
为了解决这个问题，JDK1.8 版本的 HashMap 采用了尾插法而不是头插法来避免链表倒置，使得插入的节点永远都是放在链表的末尾，避免了链表中的环形结构。但是还是不建议在多线程下使用 HashMap，因为多线程下使用 HashMap 还是会存在数据覆盖的问题。并发环境下，推荐使用 ConcurrentHashMap 。
##### HashMap 为什么线程不安全？
- 元素被覆盖
- Map长度被覆盖
##### HashMap 常见的遍历方式?
存在阻塞时 parallelStream 性能最高, 非阻塞时 parallelStream 性能最低, lambda最高。
https://mp.weixin.qq.com/s/zQBN3UvJDhRTKP6SzcZFKw
##### ConcurrentHashMap 和 Hashtable 的区别
ConcurrentHashMap 和 Hashtable 的区别主要体现在实现线程安全的方式上不同。
- 底层数据结构： JDK1.7 的 ConcurrentHashMap 底层采用 分段的数组+链表 实现，JDK1.8 采用的数据结构跟 HashMap1.8 的结构一样，数组+链表/红黑二叉树。Hashtable 和 JDK1.8 之前的 HashMap 的底层数据结构类似都是采用 数组+链表 的形式，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的；
- 实现线程安全的方式（重要）：
1. 在 JDK1.7 的时候，ConcurrentHashMap 对整个桶数组进行了分割分段(Segment，分段锁)，每一把锁只锁容器其中一部分数据（下面有示意图），多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。
2. 到了 JDK1.8 的时候，ConcurrentHashMap 已经摒弃了 Segment 的概念，而是直接用 Node 数组+链表+红黑树的数据结构来实现，并发控制使用 synchronized 和 CAS 来操作。（JDK1.6 以后 synchronized 锁做了很多优化） 整个看起来就像是优化过且线程安全的 HashMap，虽然在 JDK1.8 中还能看到 Segment 的数据结构，但是已经简化了属性，只是为了兼容旧版本；
3. Hashtable(同一把锁) :使用 synchronized 来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用 put 添加元素，另一个线程不能使用 put 添加元素，也不能使用 get，竞争会越来越激烈效率越低。
JDK1.8 的 ConcurrentHashMap 不再是 Segment 数组 + HashEntry 数组 + 链表，而是 Node 数组 + 链表 / 红黑树。不过，Node 只能用于链表的情况，红黑树的情况需要使用 TreeNode。当冲突链表达到一定长度时，链表会转换成红黑树。
TreeNode是存储红黑树节点，被TreeBin包装。TreeBin通过root属性维护红黑树的根结点，因为红黑树在旋转的时候，根结点可能会被它原来的子节点替换掉，在这个时间点，如果有其他线程要写这棵红黑树就会发生线程不安全问题，所以在 ConcurrentHashMap 中TreeBin通过waiter属性维护当前使用这棵红黑树的线程，来防止其他线程的进入。
##### ConcurrentHashMap 线程安全的具体实现方式/底层具体实现
JDK1.8 之前
ConcurrentHashMap 是由 Segment 数组结构和 HashEntry 数组结构组成。Segment 继承了 ReentrantLock,所以 Segment 是一种可重入锁，扮演锁的角色。HashEntry 用于存储键值对数据。
一个 ConcurrentHashMap 里包含一个 Segment 数组，Segment 的个数一旦初始化就不能改变。 Segment 数组的大小默认是 16，也就是说默认可以同时支持 16 个线程并发写。
对同一 Segment 的并发写入会被阻塞，不同 Segment 的写入是可以并发执行的。
JDK1.8 之后
Java 8 中，锁粒度更细，synchronized 只锁定当前链表或红黑二叉树的首节点，这样只要 hash 不冲突，就不会产生并发，就不会影响其他 Node 的读写，效率大幅提升。
##### JDK 1.7 和 JDK 1.8 的 ConcurrentHashMap 实现有什么不同？
并发度：JDK 1.7 最大并发度是 Segment 的个数，默认是 16。JDK 1.8 最大并发度是 Node 数组的大小，并发度更大。
##### ConcurrentHashMap 能保证复合操作的原子性吗？
ConcurrentHashMap 是线程安全的，意味着它可以保证多个线程同时对它进行读写操作时，不会出现数据不一致的情况，也不会导致 JDK1.7 及之前版本的 HashMap 多线程操作导致死循环问题。但是，这并不意味着它可以保证所有的复合操作都是原子性的，一定不要搞混了！
复合操作是指由多个基本操作(如put、get、remove、containsKey等)组成的操作，例如先判断某个键是否存在containsKey(key)，然后根据结果进行插入或更新put(key, value)。这种操作在执行过程中可能会被其他线程打断，导致结果不符合预期。
ConcurrentHashMap 提供了一些原子性的复合操作，如 putIfAbsent、compute、computeIfAbsent 、computeIfPresent、merge等。这些方法都可以接受一个函数作为参数，根据给定的 key 和 value 来计算一个新的 value，并且将其更新到 map 中。
不建议使用加锁的同步机制，违背了使用 ConcurrentHashMap 的初衷。在使用 ConcurrentHashMap 的时候，尽量使用这些原子性的复合操作方法来保证原子性。
#### Collections 工具类 （不重要）
Collections 工具类常用方法:
- 排序
```
void reverse(List list)//反转
void shuffle(List list)//随机排序
void sort(List list)//按自然排序的升序排序
void sort(List list, Comparator c)//定制排序，由Comparator控制排序逻辑
void swap(List list, int i , int j)//交换两个索引位置的元素
void rotate(List list, int distance)//旋转。当distance为正数时，将list后distance个元素整体移到前面。当distance为负数时，将 list的前distance个元素整体移到后面
```
- 查找,替换操作
```
int binarySearch(List list, Object key)//对List进行二分查找，返回索引，注意List必须是有序的
int max(Collection coll)//根据元素的自然顺序，返回最大的元素。 类比int min(Collection coll)
int max(Collection coll, Comparator c)//根据定制排序，返回最大元素，排序规则由Comparatator类控制。类比int min(Collection coll, Comparator c)
void fill(List list, Object obj)//用指定的元素代替指定list中的所有元素
int frequency(Collection c, Object o)//统计元素出现次数
int indexOfSubList(List list, List target)//统计target在list中第一次出现的索引，找不到则返回-1，类比int lastIndexOfSubList(List source, list target)
boolean replaceAll(List list, Object oldVal, Object newVal)//用新元素替换旧元素
```
- 同步控制(不推荐，需要线程安全的集合类型时请考虑使用 JUC 包下的并发集合)
```
synchronizedCollection(Collection<T>  c) //返回指定 collection 支持的同步（线程安全的）collection。
synchronizedList(List<T> list)//返回指定列表支持的同步（线程安全的）List。
synchronizedMap(Map<K,V> m) //返回由指定映射支持的同步（线程安全的）Map。
synchronizedSet(Set<T> s) //返回指定 set 支持的同步（线程安全的）set。
```
### Java 集合使用注意事项总结
#### 集合转Map
在使用 java.util.stream.Collectors 类的 toMap() 方法转为 Map 集合时，一定要注意当 value 为 null 时会抛 NPE 异常。
#### 集合遍历
Java8 开始，可以使用 Collection#removeIf()方法删除满足特定条件的
#### 集合转数组
使用集合转数组的方法，必须使用集合的 toArray(T[] array)，传入的是类型完全一致、长度为 0 的空数组。
```
由于 JVM 优化，new String[0]作为Collection.toArray()方法的参数现在使用更好，new String[0]就是起一个模板的作用，指定了返回数组的类型，0 是为了节省空间，因为它只是为了说明返回的类型。
```
#### 数组转集合
1、Arrays.asList()是泛型方法，传递的数组必须是对象数组，而不是基本类型。
2、使用集合的修改方法: add()、remove()、clear()会抛出异常。
Arrays.asList() 方法返回的并不是 java.util.ArrayList ，而是 java.util.Arrays 的一个内部类,这个内部类并没有实现集合的修改方法或者说并没有重写这些方法。
那我们如何正确的将数组转换为 ArrayList ?
1、手动实现工具类
2、最简便的方法
```
List list = new ArrayList<>(Arrays.asList("a", "b", "c"))
```
3、使用 Java8 的 Stream(推荐)
```
Integer [] myArray = { 1, 2, 3 };
List myList = Arrays.stream(myArray).collect(Collectors.toList());
//基本类型也可以实现转换（依赖boxed的装箱操作）
int [] myArray2 = { 1, 2, 3 };
List myList = Arrays.stream(myArray2).boxed().collect(Collectors.toList());
```
4、使用 Guava
```
List<String> il = ImmutableList.of("string", "elements");  // from varargs
List<String> il = ImmutableList.copyOf(aStringArray);      // from array

List<String> l1 = Lists.newArrayList(anotherListOrCollection);    // from collection
List<String> l2 = Lists.newArrayList(aStringArray);               // from array
List<String> l3 = Lists.newArrayList("or", "string", "elements"); // from varargs
```
5、使用 Apache Commons Collections
```
List<String> list = new ArrayList<String>();
CollectionUtils.addAll(list, str);
```
6、使用 Java9 的 List.of()方法
```
Integer[] array = {1, 2, 3};
List<Integer> list = List.of(array);
``` 
### 源码分析
#### ArrayList 源码分析
##### ArrayList 简介
在添加大量元素前，应用程序可以使用ensureCapacity操作来增加 ArrayList 实例的容量。这可以减少递增式再分配的数量。
##### ArrayList 扩容机制分析
以无参数构造方法创建 ArrayList 时，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为 10。 
ArrayList 每次扩容之后容量都会变为原来的 1.5 倍左右。
- Java 中的 length属性是针对数组说的,比如说你声明了一个数组,想知道这个数组的长度则用到了 length 这个属性.
- Java 中的 length() 方法是针对字符串说的,如果想看这个字符串的长度则用到 length() 这个方法.
- Java 中的 size() 方法是针对泛型集合说的,如果想看这个泛型有多少个元素,就调用此方法来查看!
###### hugeCapacity() 方法
如果新容量大于 MAX_ARRAY_SIZE,进入(执行) hugeCapacity() 方法来比较 minCapacity 和 MAX_ARRAY_SIZE，如果 minCapacity 大于最大容量，则新容量则为Integer.MAX_VALUE，否则，新容量大小则为 MAX_ARRAY_SIZE 即为 Integer.MAX_VALUE - 8。
###### System.arraycopy() 和 Arrays.copyOf()方法
arraycopy() 需要目标数组，将原数组拷贝到你自己定义的数组里或者原数组，而且可以选择拷贝的起点和长度以及放入新数组中的位置 
copyOf() 是系统自动在内部新建一个数组，并返回该数组。Arrays.copyOf()方法主要是为了给原有数组扩容
#### LinkedList 源码分析
##### 获取元素
get(int index) 或 remove（int index）
通过比较索引值与链表 size 的一半大小来确定从链表头还是尾开始遍历。如果索引值小于 size 的一半，就从链表头开始遍历，反之从链表尾开始遍历。这样可以在较短的时间内找到目标节点，充分利用了双向链表的特性来提高效率。
#### HashMap 源码分析
##### 底层数据结构分析
- loadFactor 负载因子
loadFactor 太大导致查找元素效率低，太小导致数组的利用率低，存放的数据会很分散。loadFactor 的默认值为 0.75f 是官方给出的一个比较好的临界值。
给定的默认容量为 16，负载因子为 0.75。Map 在使用过程中不断的往里面存放数据，当数量超过了 16 * 0.75 = 12 就需要将当前 16 的容量进行扩容，而扩容这个过程涉及到 rehash、复制数据等操作，所以非常消耗性能。
- threshold
threshold = capacity * loadFactor，当 Size>threshold的时候，那么就要考虑对数组的扩增了，也就是说，这个的意思就是 衡量数组是否需要扩增的一个标准。
###### resize 方法
进行扩容，会伴随着一次重新 hash 分配，并且会遍历 hash 表中所有的元素，是非常耗时的。在编写程序中，要尽量避免 resize。resize 方法实际上是将 table 初始化和 table 扩容 进行了整合，底层的行为都是给 table 赋值一个新的数组。
#### ConcurrentHashMap 源码分析
##### ConcurrentHashMap 1.7
- 校验并发级别 concurrencyLevel 大小，如果大于最大值，重置为最大值。无参构造默认值是 16.
- 寻找并发级别 concurrencyLevel 之上最近的 2 的幂次方值，作为初始化容量大小，默认是 16。
- 记录 segmentShift 偏移量，这个值为【容量 = 2 的 N 次方】中的 N，在后面 Put 时计算位置时会用到。默认是 32 - sshift = 28.
- 记录 segmentMask，默认是 ssize - 1 = 16 -1 = 15.
- 初始化 segments[0]，默认大小为 2，负载因子 0.75，扩容阀值是 2*0.75=1.5，插入第二个值时才会进行扩容。
##### ConcurrentHashMap 1.8
###### put
- 即为当前 key 定位出的 Node，如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。
- 如果当前位置的 hashcode == MOVED == -1,则需要进行扩容。
- 如果都不满足，则利用 synchronized 锁写入数据。
- 如果数量大于 TREEIFY_THRESHOLD 则要执行树化方法，在 treeifyBin 中会首先判断当前数组长度 ≥64 时才会将链表转换为红黑树。
#### LinkedHashMap 源码分析
##### LinkedHashMap 简介
因为内部使用双向链表维护各个节点，使得原本散列在不同 bucket 上的节点、链表、红黑树有序关联起来。所以遍历时的效率和元素个数成正比，相较于和容量成正比的 HashMap 来说，迭代效率会高很多。
##### LinkedHashMap 使用示例
###### 访问顺序遍历
为了实现访问顺序遍历，我们可以使用传入 accessOrder 属性的 LinkedHashMap 构造方法，并将 accessOrder 设置为 true，表示其具备访问有序性。默认为 false。构造方法中指定 accessOrder 为 true ，这样在访问元素时就会把该元素移动到链表尾部，链表首元素就是最近最少被访问的元素；重写removeEldestEntry 方法，该方法会返回一个 boolean 值，告知 LinkedHashMap 是否需要移除链表首元素（缓存容量有限）。
##### LinkedHashMap 源码解析
###### Node 的设计
者们认为在良好的 hashCode 算法时，HashMap 转红黑树的概率不大。就算转为红黑树变为树节点，也可能会因为移除或者扩容将 TreeNode 变为 Node，所以 TreeNode 的使用概率不算很大，对于这一点资源空间的浪费是可以接受的。
###### remove 方法后置操作——afterNodeRemoval
LinkedHashMap 并没有对 remove 方法进行重写，而是直接继承 HashMap 的 remove 方法，为了保证键值对移除后双向链表中的节点也会同步被移除，LinkedHashMap 重写了 HashMap 的空实现方法 afterNodeRemoval。
##### LinkedHashMap 和 HashMap 遍历性能比较
LinkedHashMap 维护了一个双向链表来记录数据插入的顺序，因此在迭代遍历生成的迭代器的时候，是按照双向链表的路径进行遍历的。这一点相比于 HashMap 那种遍历整个 bucket 的方式来说，高效需多。
#### CopyOnWriteArrayList 源码分析
##### CopyOnWriteArrayList 简介
###### CopyOnWriteArrayList 到底有什么厉害之处？
对于大部分业务场景来说，读取操作往往是远大于写入操作的。由于读取操作不会对原有数据进行修改，因此，对于每次读取都进行加锁其实是一种资源浪费。
这种思路与 ReentrantReadWriteLock 读写锁的设计思想非常类似，即读读不互斥、读写互斥、写写互斥（只有读读不互斥）。
CopyOnWriteArrayList 中的读取操作是完全无需加锁的。更加厉害的是，写入操作也不会阻塞读取操作，只有写写才会互斥。
###### Copy-On-Write 的思想是什么？
如果有多个调用者（callers）同时请求相同资源（如内存或磁盘上的数据存储），他们会共同获取相同的指针指向相同的资源，直到某个调用者试图修改资源的内容时，系统才会真正复制一份专用副本（private copy）给该调用者，而其他调用者所见到的最初的资源仍然保持不变。
###### CopyOnWriteArrayList 源码分析
####### 插入元素
锁被修饰保证了锁的内存地址肯定不会被修改，并且，释放锁的逻辑放在 finally 中，可以保证锁能被释放。
Arrays.copyOf 由于底层调用了系统级别的拷贝指令，因此在实际应用中这个方法的性能表现比较优秀，但是也需要注意控制复制的数据量，避免出现内存占用过高的情况。
####### 获取列表中元素的个数
CopyOnWriteArrayList中的array数组每次复制都刚好能够容纳下所有元素，并不像ArrayList那样会预留一定的空间。因此，CopyOnWriteArrayList中并没有size属性CopyOnWriteArrayList的底层数组的长度就是元素个数，因此size()方法只要返回数组长度就可以了。
#### ArrayBlockingQueue 源码分析
##### 阻塞队列简介
ArrayBlockingQueue 是有界队列，即添加的元素达到上限之后，再次添加就会被阻塞或者抛出异常。
##### ArrayBlockingQueue 常见方法及测试
某些场景下，我们希望能够一次性将阻塞队列的结果存到列表中再进行批量操作，我们就可以使用阻塞队列的 drainTo 方法，这个方法会一次性将队列中所有元素存放到列表。
##### 阻塞式获取和新增元素
更新 putIndex 到下一个位置，如果 putIndex 等于队列长度，则说明 putIndex 已经到达数组末尾了，下一次插入则需要 0 开始。(ArrayBlockingQueue 用到了循环队列的思想，即从头到尾循环复用一个数组)
##### ArrayBlockingQueue 相关面试题
ArrayBlockingQueue 的并发控制采用可重入锁 ReentrantLock ，不管是插入操作还是读取操作，都需要获取到锁才能进行操作。并且，它还支持公平和非公平两种方式的锁访问机制，默认是非公平锁。
ArrayBlockingQueue中的锁是没有分离的，即生产和消费用的是同一个锁；LinkedBlockingQueue中的锁是分离的，即生产用的是putLock，消费是takeLock，这样可以防止生产者和消费者线程之间的锁争夺。
ArrayBlockingQueue 支持阻塞和非阻塞两种获取和新增元素的方式（一般只会使用前者）， ConcurrentLinkedQueue 是无界的，仅支持非阻塞式获取和新增元素。
#### PriorityQueue 源码分析
TODO
#### DelayQueue 源码分析
##### DelayQueue 简介
DelayQueue 会按照到期时间升序编排任务。只有当元素过期时（getDelay()方法返回值小于等于 0），才能从队列中取出。
##### DelayQueue 常见使用场景示例
对此我们可以使用 DelayQueue 来实现,所以我们首先需要继承 Delayed 实现 DelayedTask，实现 getDelay 方法以及优先级比较 compareTo。
##### DelayQueue 常见面试题
###### DelayQueue 和 Timer/TimerTask 的区别是什么？
DelayQueue 和 Timer/TimerTask 都可以用于实现定时任务调度，但是它们的实现方式不同。DelayQueue 是基于优先级队列和堆排序算法实现的，可以实现多个任务按照时间先后顺序执行；而 Timer/TimerTask 是基于单线程实现的，只能按照任务的执行顺序依次执行，如果某个任务执行时间过长，会影响其他任务的执行。另外，DelayQueue 还支持动态添加和移除任务，而 Timer/TimerTask 只能在创建时指定任务。
## 并发编程
### Java 并发常见面试题总结(上)
#### Java 线程和操作系统的线程有啥区别？
在 Windows 和 Linux 等主流操作系统中，Java 线程采用的是一对一的线程模型，也就是一个 Java 线程对应一个系统内核线程。Solaris 系统是一个特例（Solaris 系统本身就支持多对多的线程模型），HotSpot VM 在 Solaris 上支持多对多和一对一。
#### 请简要描述线程与进程的关系,区别及优缺点？
##### 程序计数器为什么是私有的?
需要注意的是，如果执行的是 native 方法，那么程序计数器记录的是 undefined 地址，只有执行的是 Java 代码时程序计数器记录的才是下一条指令的地址。
所以，程序计数器私有主要是为了线程切换后能恢复到正确的执行位置。
##### 虚拟机栈和本地方法栈为什么是私有的?
为了保证线程中的局部变量不被别的线程访问到，虚拟机栈和本地方法栈是线程私有的。
#### sleep() 方法和 wait() 方法对比
- sleep() 方法没有释放锁，而 wait() 方法释放了锁 。
- wait() 通常被用于线程间交互/通信，sleep()通常被用于暂停执行。
- wait() 方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 notify()或者 notifyAll() 方法。sleep()方法执行完成后，线程会自动苏醒，或者也可以使用 wait(long timeout) 超时后线程会自动苏醒。
- sleep() 是 Thread 类的静态本地方法，wait() 则是 Object 类的本地方法。
#### 为什么 wait() 方法不定义在 Thread 中？
wait() 是让获得对象锁的线程实现等待，会自动释放当前线程占有的对象锁。每个对象（Object）都拥有对象锁，既然要释放当前线程占有的对象锁并让其进入 WAITING 状态，自然是要操作对应的对象（Object）而非当前的线程（Thread）。
#### 可以直接调用 Thread 类的 run 方法吗？
调用 start() 方法方可启动线程并使线程进入就绪状态，直接执行 run() 方法的话不会以多线程的方式执行。
### Java 并发常见面试题总结(中)
#### volatile 关键字
##### 如何禁止指令重排序？
uniqueInstance 采用 volatile 关键字修饰也是很有必要的， uniqueInstance = new Singleton(); 这段代码其实是分为三步执行：
- 为 uniqueInstance 分配内存空间
- 初始化 uniqueInstance
- 将 uniqueInstance 指向分配的内存地址
#### 乐观锁和悲观锁
高并发的场景下，乐观锁相比悲观锁来说，不存在锁竞争造成线程阻塞，也不会有死锁的问题，在性能上往往会更胜一筹。但是，如果冲突频繁发生（写占比非常多的情况），会频繁失败和重试，这样同样会非常影响性能，导致 CPU 飙升。
悲观锁通常多用于写比较多的情况（多写场景，竞争激烈），这样可以避免频繁失败和重试影响性能，悲观锁的开销是固定的。不过，如果乐观锁解决了频繁失败和重试这个问题的话（比如LongAdder），也是可以考虑使用乐观锁的，要视实际情况而定。
##### CAS 算法
###### CAS 算法存在哪些问题？
只能保证一个共享变量的原子操作
CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效。但是从 JDK 1.5 开始，提供了AtomicReference类来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行 CAS 操作.所以我们可以使用锁或者利用AtomicReference类把多个共享变量合并成一个共享变量来操作。
#### synchronized 关键字
在 Java 早期版本中，synchronized 属于 重量级锁，效率低下。这是因为监视器锁（monitor）是依赖于底层的操作系统的 Mutex Lock 来实现的，Java 的线程是映射到操作系统的原生线程之上的。如果要挂起或者唤醒一个线程，都需要操作系统帮忙完成，而操作系统实现线程之间的切换时需要从用户态转换到内核态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高。
不过，在 Java 6 之后， synchronized 引入了大量的优化如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销，这些优化让 synchronized 锁的效率提升了很多。因此， synchronized 还是可以在实际项目中使用的，像 JDK 源码、很多开源框架都大量使用了 synchronized 。关于偏向锁多补充一点：由于偏向锁增加了 JVM 的复杂性，同时也并没有为所有应用都带来性能提升。在 JDK18 中，偏向锁已经被彻底废弃（无法通过命令行打开）。
##### 如何使用 synchronized？
静态 synchronized 方法和非静态 synchronized 方法之间的调用互斥么？不互斥！
- synchronized(object) 表示进入同步代码库前要获得 给定对象的锁。
- synchronized(类.class) 表示进入同步代码前要获得 给定 Class 的锁
总结
- synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指明同步代码块的结束位置。
- synchronized 修饰的方法并没有 monitorenter 指令和 monitorexit 指令，取得代之的确实是 ACC_SYNCHRONIZED 标识，该标识指明了该方法是一个同步方法。
不过两者的本质都是对对象监视器 monitor 的获取。
##### JDK1.6 之后的 synchronized 底层做了哪些优化？锁升级原理了解吗？
锁主要存在四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。
##### synchronized 和 volatile 有什么区别？
synchronized 关键字和 volatile 关键字是两个互补的存在，而不是对立的存在！
volatile关键字主要用于解决变量在多个线程之间的可见性，而 synchronized 关键字解决的是多个线程之间访问资源的同步性。
#### ReentrantLock
ReentrantLock 默认使用非公平锁，也可以通过构造器来显式的指定使用公平锁。
##### synchronized 和 ReentrantLock 有什么区别？
- synchronized 依赖于 JVM 而 ReentrantLock 依赖于 API
- ReentrantLock 比 synchronized 增加了一些高级功能
1. 等待可中断 lock.lockInterruptibly()
2. 可实现公平锁 
3. 可实现选择性通知（锁可以绑定多个条件） synchronized关键字与wait()和notify()/notifyAll()方法相结合可以实现等待/通知机制。ReentrantLock类当然也可以实现，但是需要借助于Condition接口与newCondition()方法。
#### ReentrantReadWriteLock
ReentrantReadWriteLock 在实际项目中使用的并不多，面试中也问的比较少，简单了解即可。JDK 1.8 引入了性能更好的读写锁 StampedLock 。
##### 线程持有读锁还能获取写锁吗？
- 在线程持有读锁的情况下，该线程不能取得写锁(因为获取写锁的时候，如果发现当前的读锁被占用，就马上获取失败，不管读锁是不是被当前线程持有)。
- 在线程持有写锁的情况下，该线程可以继续获取读锁（获取读锁时如果发现写锁被占用，只有写锁没有被当前线程占用的情况才会获取失败）。
##### 读锁为什么不能升级为写锁？
写锁可以降级为读锁，但是读锁却不能升级为写锁。这是因为读锁升级为写锁会引起线程的争夺，毕竟写锁属于是独占锁，这样的话，会影响性能。
另外，还可能会有死锁问题发生。举个例子：假设两个线程的读锁都想升级写锁，则需要对方都释放自己锁，而双方都不释放，就会产生死锁。
#### StampedLock
##### StampedLock 是什么
StampedLock 是 JDK 1.8 引入的性能更好的读写锁，不可重入且不支持条件变量 Condition。
StampedLock 并不是直接实现 Lock或 ReadWriteLock接口，而是基于 CLH 锁 独立实现的（AQS 也是基于这玩意。
StampedLock 提供了三种模式的读写控制模式：读锁、写锁和乐观读。
StampedLock 还支持这三种锁在一定条件下进行相互转换。
##### StampedLock 的性能为什么更好？
StampedLock 的乐观读允许一个写线程获取写锁，所以不会导致所有写线程阻塞，也就是当读多写少的时候，写线程有机会获取写锁，减少了线程饥饿的问题，吞吐量大大提高。
### Java 并发常见面试题总结(下)
#### ThreadLocal
##### ThreadLocal 有什么用？
ThreadLocal类主要解决的就是让每个线程绑定自己的值，可以将ThreadLocal类形象的比喻成存放数据的盒子，盒子中可以存储每个线程的私有数据。
##### 如何使用 ThreadLocal？
```
public class ThreadLocalExample implements Runnable{

     // SimpleDateFormat 不是线程安全的，所以每个线程都要有自己独立的副本
    private static final ThreadLocal<SimpleDateFormat> formatter = ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyyMMdd HHmm"));

    @Override
    public void run() {
        System.out.println("default Formatter = "+formatter.get().toPattern());
        //formatter pattern is changed here by thread, but it won't reflect to other threads
        formatter.set(new SimpleDateFormat());
        System.out.println("formatter = "+formatter.get().toPattern());
    }

}

```
##### ThreadLocal 原理了解吗？
ThreadLocalMap 理解为ThreadLocal 类实现的定制化的 HashMap。默认情况下这两个变量都是 null，只有当前线程调用 ThreadLocal 类的 set或get方法时才创建它们，实际上调用这两个方法的时候，我们调用的是ThreadLocalMap类对应的 get()、set()方法。
每个Thread中都具备一个ThreadLocalMap，而ThreadLocalMap可以存储以ThreadLocal为 key ，Object 对象为 value 的键值对。
我们在同一个线程中声明了两个 ThreadLocal 对象的话， Thread内部都是使用仅有的那个ThreadLocalMap 存放数据的，ThreadLocalMap的 key 就是 ThreadLocal对象，value 就是 ThreadLocal 对象调用set方法设置的值。
##### ThreadLocal 内存泄露问题是怎么导致的？
ThreadLocalMap 中使用的 key 为 ThreadLocal 的弱引用，而 value 是强引用。所以，如果 ThreadLocal 没有被外部强引用的情况下，在垃圾回收的时候，key 会被清理掉，而 value 不会被清理掉。
#### 线程池
##### 如何创建线程池？
1. 通过ThreadPoolExecutor构造函数来创建（推荐）。
ThreadPoolExecutor 3 个最重要的参数：
- corePoolSize : 任务队列未达到队列容量时，最大可以同时运行的线程数量。
- maximumPoolSize : 任务队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。
- workQueue: 新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。
ThreadPoolExecutor其他常见参数 :
- keepAliveTime:线程池中的线程数量大于 corePoolSize 的时候，如果这时没有新的任务提交，核心线程外的线程不会立即销毁，而是会等待，直到等待的时间超过了 keepAliveTime才会被回收销毁。
- unit : keepAliveTime 参数的时间单位。
- threadFactory :executor 创建新线程的时候会用到。
- handler :饱和策略（后面会单独详细介绍一下）。
2. 通过 Executor 框架的工具类 Executors 来创建。
可以看出，通过Executors工具类可以创建多种类型的线程池，包括
- FixedThreadPool：固定线程数量的线程池。该线程池中的线程数量始终不变。当有一个新的任务提交时，线程池中若有空闲线程，则立即执行。若没有，则新的任务会被暂存在一个任务队列中，待有线程空闲时，便处理在任务队列中的任务。
- SingleThreadExecutor： 只有一个线程的线程池。若多余一个任务被提交到该线程池，任务会被保存在一个任务队列中，待线程空闲，按先入先出的顺序执行队列中的任务。
- CachedThreadPool： 可根据实际情况调整线程数量的线程池。线程池的线程数量不确定，但若有空闲线程可以复用，则会优先使用可复用的线程。若所有线程均在工作，又有新的任务提交，则会创建新的线程处理任务。所有线程在当前任务执行完毕后，将返回线程池进行复用。
- ScheduledThreadPool：给定的延迟后运行任务或者定期执行任务的线程池。
Executors 返回线程池对象的弊端如下：
- FixedThreadPool 和 SingleThreadExecutor:使用的是无界的 LinkedBlockingQueue，任务队列最大长度为 Integer.MAX_VALUE,可能堆积大量的请求，从而导致 OOM。
- CachedThreadPool:使用的是同步队列 SynchronousQueue, 允许创建的线程数量为 Integer.MAX_VALUE ，如果任务数量过多且执行速度较慢，可能会创建大量的线程，从而导致 OOM。
- ScheduledThreadPool 和 SingleThreadScheduledExecutor:使用的无界的延迟阻塞队列DelayedWorkQueue，任务队列最大长度为 Integer.MAX_VALUE,可能堆积大量的请求，从而导致 OOM。
##### 线程池的饱和策略有哪些？
- ThreadPoolExecutor.AbortPolicy：抛出 RejectedExecutionException来拒绝新任务的处理。
- ThreadPoolExecutor.CallerRunsPolicy：调用执行自己的线程运行任务，也就是直接在调用execute方法的线程中运行(run)被拒绝的任务，如果执行程序已关闭，则会丢弃该任务。因此这种策略会降低对于新任务提交速度，影响程序的整体性能。
- ThreadPoolExecutor.DiscardPolicy：不处理新任务，直接丢弃掉。
- ThreadPoolExecutor.DiscardOldestPolicy：此策略将丢弃最早的未处理的任务请求。
##### 如何给线程池命名？
- 利用 guava 的 ThreadFactoryBuilder
```
ThreadFactory threadFactory = new ThreadFactoryBuilder()
                        .setNameFormat(threadNamePrefix + "-%d")
                        .setDaemon(true).build();
ExecutorService threadPool = new ThreadPoolExecutor(corePoolSize, maximumPoolSize, keepAliveTime, TimeUnit.MINUTES, workQueue, threadFactory);
```
- 自己实现 ThreadFactory
```
import java.util.concurrent.ThreadFactory;
import java.util.concurrent.atomic.AtomicInteger;

/**
 * 线程工厂，它设置线程名称，有利于我们定位问题。
 */
public final class NamingThreadFactory implements ThreadFactory {

    private final AtomicInteger threadNum = new AtomicInteger();
    private final String name;

    /**
     * 创建一个带名字的线程池生产工厂
     */
    public NamingThreadFactory(String name) {
        this.name = name;
    }

    @Override
    public Thread newThread(Runnable r) {
        Thread t = new Thread(r);
        t.setName(name + " [#" + threadNum.incrementAndGet() + "]");
        return t;
    }
}
```
##### 如何设定线程池的大小？
- CPU 密集型任务(N+1)： 这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1。比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。
- I/O 密集型任务(2N)： 这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是 2N。
线程数更严谨的计算的方法应该是：最佳线程数 = N（CPU 核心数）∗（1+WT（线程等待时间）/ST（线程计算时间）），其中 WT（线程等待时间）=线程运行总时间 - ST（线程计算时间）。线程等待时间所占比例越高，需要越多线程。线程计算时间所占比例越高，需要越少线程。我们可以通过 JDK 自带的工具 VisualVM 来查看 WT/ST 比例。CPU 密集型任务的 WT/ST 接近或者等于 0，因此， 线程数可以设置为 N（CPU 核心数）∗（1+0）= N，和我们上面说的 N（CPU 核心数）+1 差不多。IO 密集型任务下，几乎全是线程等待时间，从理论上来说，你就可以将线程数设置为 2N（按道理来说，WT/ST 的结果应该比较大，这里选择 2N 的原因应该是为了避免创建过多线程吧）。
#### Future
##### Callable 和 Future 有什么关系？
FutureTask 提供了 Future 接口的基本实现，常用来封装 Callable 和 Runnable。传入 Runnable 对象也会在方法内部转换为Callable 对象。
##### CompletableFuture 类有什么用？
Future 在实际使用过程中存在一些局限性比如不支持异步任务的编排组合、获取计算结果的 get() 方法为阻塞调用。
Java 8 才被引入CompletableFuture 类可以解决Future 的这些缺陷。CompletableFuture 除了提供了更为好用和强大的 Future 特性之外，还提供了函数式编程、异步任务编排组合（可以将多个异步任务串联起来，组成一个完整的链式调用）等能力。
CompletionStage 接口描述了一个异步计算的阶段。很多计算可以分成多个阶段或步骤，此时可以通过它将所有步骤组合起来，形成异步计算的流水线。
CompletionStage 接口中的方法比较多，CompletableFuture 的函数式能力就是这个接口赋予的。从这个接口的方法参数你就可以发现其大量使用了 Java8 引入的函数式编程。
```
//文件夹位置
List<String> filePaths = Arrays.asList(...)
// 异步处理所有文件
List<CompletableFuture<String>> fileFutures = filePaths.stream()
    .map(filePath -> doSomeThing(filePath))
    .collect(Collectors.toList());
// 将他们合并起来
CompletableFuture<Void> allFutures = CompletableFuture.allOf(
    fileFutures.toArray(new CompletableFuture[fileFutures.size()])
);
```
#### AQS
##### AQS 的原理是什么？
AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用 CLH 队列锁 实现的，即将暂时获取不到锁的线程加入到队列中。
CLH(Craig,Landin,and Hagersten) 队列是一个虚拟的双向队列（虚拟的双向队列即不存在队列实例，仅存在结点之间的关联关系）。AQS 是将每条请求共享资源的线程封装成一个 CLH 锁队列的一个结点（Node）来实现锁的分配。在 CLH 同步队列中，一个节点表示一个线程，它保存着线程的引用（thread）、 当前节点在队列中的状态（waitStatus）、前驱节点（prev）、后继节点（next）。
AQS 使用 int 成员变量 state 表示同步状态，通过内置的 线程等待队列 来完成获取资源线程的排队工作。state 变量由 volatile 修饰，用于展示当前临界资源的获锁情况。
- 以 ReentrantLock 为例，state 初始值为 0，表示未锁定状态。A 线程 lock() 时，会调用 tryAcquire() 独占该锁并将 state+1 。此后，其他线程再 tryAcquire() 时就会失败，直到 A 线程 unlock() 到 state=0（即释放锁）为止，其它线程才有机会获取该锁。当然，释放锁之前，A 线程自己是可以重复获取此锁的（state 会累加），这就是可重入的概念。但要注意，获取多少次就要释放多少次，这样才能保证 state 是能回到零态的。
##### Semaphore 有什么用？
synchronized 和 ReentrantLock 都是一次只允许一个线程访问某个资源，而Semaphore(信号量)可以用来控制同时访问特定资源的线程数量。
Semaphore 通常用于那些资源有明确访问数量限制的场景比如限流（仅限于单机模式，实际项目中推荐使用 Redis +Lua 来做限流）。
##### CountDownLatch 有什么用？
CountDownLatch 是一次性的，计数器的值只能在构造方法中初始化一次，之后没有任何机制再次对其设置值，当 CountDownLatch 使用完毕后，它不能再次被使用。
##### 用过 CountDownLatch 么？什么场景下用的？
我们要读取处理 6 个文件，这 6 个任务都是没有执行顺序依赖的任务，但是我们需要返回给用户的时候将这几个文件的处理的结果进行统计整理。
为此我们定义了一个线程池和 count 为 6 的CountDownLatch对象 。使用线程池处理读取任务，每一个线程处理完之后就将 count-1，调用CountDownLatch对象的 await()方法，直到所有文件读取完之后，才会接着执行后面的逻辑。
##### CyclicBarrier 有什么用？
CountDownLatch 的实现是基于 AQS 的，而 CycliBarrier 是基于 ReentrantLock(ReentrantLock 也属于 AQS 同步器)和 Condition 的。
### 重要知识点
#### JMM(Java 内存模型)详解
##### 从 CPU 缓存模型说起
CPU 为了解决内存缓存不一致性问题可以通过制定缓存一致协议（比如 MESI 协议）或者其他手段来解决。 这个缓存一致性协议指的是在 CPU 高速缓存与主内存交互的时候需要遵守的原则和规范。不同的 CPU 中，使用的缓存一致性协议通常也会有所不同。
##### 指令重排序
- 编译器优化重排
- 指令并行重排： 现代处理器采用了指令级并行技术(Instruction-Level Parallelism，ILP)来将多条指令重叠执行。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序。
##### 什么是 JMM？为什么需要 JMM？
一般来说，编程语言也可以直接复用操作系统层面的内存模型。不过，不同的操作系统内存模型不同。如果直接复用操作系统层面的内存模型，就可能会导致同样一套代码换了一个操作系统就无法执行了。Java 语言是跨平台的，它需要自己提供一套内存模型以屏蔽系统差异。
你可以把 JMM 看作是 Java 定义的并发编程相关的一组规范，除了抽象了线程和主内存之间的关系之外，其还规定了从 Java 源代码到 CPU 可执行指令的这个转化过程要遵守哪些和并发相关的原则和规范，其主要目的是为了简化多线程编程，增强程序可移植性的。
##### JMM 是如何抽象线程和主内存之间的关系？
从工作内存同步到主内存之间的实现细节，Java 内存模型定义来以下八种同步操作（了解即可，无需死记硬背）：
- 锁定（lock）: 作用于主内存中的变量，将他标记为一个线程独享变量。
- 解锁（unlock）: 作用于主内存中的变量，解除变量的锁定状态，被解除锁定状态的变量才能被其他线程锁定。
- read（读取）：作用于主内存的变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的 load 动作使用。
- load(载入)：把 read 操作从主内存中得到的变量值放入工作内存的变量的副本中。
- use(使用)：把工作内存中的一个变量的值传给执行引擎，每当虚拟机遇到一个使用到变量的指令时都会使用该指令。
- assign（赋值）：作用于工作内存的变量，它把一个从执行引擎接收到的值赋给工作内存的变量，每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。
- store（存储）：作用于工作内存的变量，它把工作内存中一个变量的值传送到主内存中，以便随后的 write 操作使用。
- write（写入）：作用于主内存的变量，它把 store 操作从工作内存中得到的变量的值放入主内存的变量中。
##### happens-before 常见规则有哪些？谈谈你的理解？
- 程序顺序规则：一个线程内，按照代码顺序，书写在前面的操作 happens-before 于书写在后面的操作；
- 解锁规则：解锁 happens-before 于加锁；
- volatile 变量规则：对一个 volatile 变量的写操作 happens-before 于后面对这个 volatile 变量的读操作。说白了就是对 volatile 变量的写操作的结果对于发生于其后的任何操作都是可见的。
- 传递规则：如果 A happens-before B，且 B happens-before C，那么 A happens-before C；
- 线程启动规则：Thread 对象的 start()方法 happens-before 于此线程的每一个动作。
#### Java 线程池详解
##### Executor 框架介绍
用线程池实现，节约开销，更有助于避免 this 逃逸问题。
##### 几个常见的对比
###### Runnable vs Callable
Runnable 接口不会返回结果或抛出检查异常，但是 Callable 接口可以。所以，如果任务不需要返回结果或抛出异常推荐使用 Runnable 接口，这样代码看起来会更加简洁。 
###### execute() vs submit()
- execute()方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；
- submit()方法用于提交需要返回值的任务。线程池会返回一个 Future 类型的对象，通过这个 Future 对象可以判断任务是否执行成功，并且可以通过 Future 的 get()方法来获取返回值，get()方法会阻塞当前线程直到任务完成，而使用 get（long timeout，TimeUnit unit）方法的话，如果在 timeout 时间内任务还没有执行完，就会抛出 java.util.concurrent.TimeoutException。
###### shutdown()VSshutdownNow()
- shutdown（） :关闭线程池，线程池的状态变为 SHUTDOWN。线程池不再接受新任务了，但是队列里的任务得执行完毕。
-     shutdownNow（） :关闭线程池，线程池的状态变为 STOP。线程池会终止当前正在运行的任务，并停止处理排队的任务并返回正在等待执行的 List。
###### isTerminated() VS isShutdown()
- isShutDown 当调用 shutdown() 方法后返回为 true。
- isTerminated 当调用 shutdown() 方法后，并且所有提交的任务完成后返回为 true
##### 几种常见的内置线程池
###### FixedThreadPool
maximumPoolSize 的值比 corePoolSize 大，也至多只会创建 corePoolSize 个线程。这是因为FixedThreadPool 使用的是容量为 Integer.MAX_VALUE 的 LinkedBlockingQueue（无界队列），队列永远不会被放满。
###### CachedThreadPool
CachedThreadPool 的corePoolSize 被设置为空（0），maximumPoolSize被设置为 Integer.MAX.VALUE，即它是无界的，这也就意味着如果主线程提交任务的速度高于 maximumPool 中线程处理任务的速度时，CachedThreadPool 会不断创建新的线程。极端情况下，这样会导致耗尽 cpu 和内存资源。
流程如下：
1. 首先执行 SynchronousQueue.offer(Runnable task) 提交任务到任务队列。如果当前 maximumPool 中有闲线程正在执行 SynchronousQueue.poll(keepAliveTime,TimeUnit.NANOSECONDS)，那么主线程执行 offer 操作与空闲线程执行的 poll 操作配对成功，主线程把任务交给空闲线程执行，execute()方法执行完成，否则执行下面的步骤 2。
2. 当初始 maximumPool 为空，或者 maximumPool 中没有空闲线程时，将没有线程执行 SynchronousQueue.poll(keepAliveTime,TimeUnit.NANOSECONDS)。这种情况下，步骤 1 将失败，此时 CachedThreadPool 会创建新线程执行任务，execute 方法执行完成。
###### ScheduledThreadPool
这个在实际项目中基本不会被用到，也不推荐使用。
####### ScheduledThreadPoolExecutor 和 Timer 对比
- Timer 对系统时钟的变化敏感，ScheduledThreadPoolExecutor不是；
- Timer 只有一个执行线程，因此长时间运行的任务可以延迟其他任务。 ScheduledThreadPoolExecutor 可以配置任意数量的线程。 此外，如果你想（通过提供 ThreadFactory），你可以完全控制创建的线程;
- 在TimerTask 中抛出的运行时异常会杀死一个线程，从而导致 Timer 死机即计划任务将不再运行。ScheduledThreadExecutor 不仅捕获运行时异常，还允许您在需要时处理它们（通过重写 afterExecute 方法ThreadPoolExecutor）。抛出异常的任务将被取消，但其他任务将继续运行。
#### Java 线程池最佳实践
##### 正确声明线程池
线程池必须手动通过 ThreadPoolExecutor 的构造函数来声明，避免使用Executors 类创建线程池，会有 OOM 风险。
##### 建议不同类别的业务用不同的线程池
父子任务用同一个线程池导致死锁。
##### 别忘记给线程池命名
##### 别忘记关闭线程池
调用完 shutdownNow 和 shuwdown 方法后，并不代表线程池已经完成关闭操作，它只是异步的通知线程池进行关闭处理。如果要同步等待线程池彻底关闭后才继续往下执行，需要调用awaitTermination方法进行同步等待。
在调用 awaitTermination() 方法时，应该设置合理的超时时间，以避免程序长时间阻塞而导致性能问题。另外。由于线程池中的任务可能会被取消或抛出异常，因此在使用 awaitTermination() 方法时还需要进行异常处理。awaitTermination() 方法会抛出 InterruptedException 异常，需要捕获并处理该异常，以避免程序崩溃或者无法正常退出。
```
// ...
// 关闭线程池
executor.shutdown();
try {
    // 等待线程池关闭，最多等待5分钟
    if (!executor.awaitTermination(5, TimeUnit.MINUTES)) {
        // 如果等待超时，则打印日志
        System.err.println("线程池未能在5分钟内完全关闭");
    }
} catch (InterruptedException e) {
    // 异常处理
}
```
##### 线程池尽量不要放耗时任务
##### 不要频繁创建线程池，复用线程池。
##### 使用 Spring 内部线程池时，一定要手动自定义线程池，配置合理的参数，不然会出现生产问题（一个请求创建一个线程）
##### 线程池和 ThreadLocal共用，可能会导致线程从ThreadLocal获取到的是旧值/脏数据。
解决上述问题比较建议的办法是使用阿里巴巴开源的 TransmittableThreadLocal(TTL)。TransmittableThreadLocal类继承并加强了 JDK 内置的InheritableThreadLocal类，在使用线程池等会池化复用线程的执行组件情况下，提供ThreadLocal值的传递功能，解决异步执行时上下文传递的问题。
#### Java 常见并发容器总结
##### CopyOnWriteArrayList
线程安全的 List，在读多写少的场合性能非常好，远远好于 Vector。
##### ConcurrentLinkedQueue
高效的并发队列，使用链表实现。可以看做一个线程安全的 LinkedList，这是一个非阻塞队列。
适合在对性能要求相对较高，同时对队列的读写存在多个线程同时进行的场景，即如果对队列加锁的成本较高则适合使用无锁的 ConcurrentLinkedQueue 来替代。
##### BlockingQueue
这是一个接口，JDK 内部通过链表、数组等方式实现了这个接口。表示阻塞队列，非常适合用于作为数据共享的通道。
##### ConcurrentSkipListMap
跳表的实现。这是一个 Map，使用跳表的数据结构进行快速查找。
哈希并不会保存元素的顺序，而跳表内所有的元素都是排序的。因此在对跳表进行遍历时，你会得到一个有序的结果。
#### AQS详解
##### AQS 资源共享方式
AQS 定义两种资源共享方式：Exclusive（独占，只有一个线程能执行，如ReentrantLock，tryAcquire-tryRelease）和Share（共享，多个线程可同时执行，如Semaphore/CountDownLatch，tryAcquireShared-tryReleaseShared）。
##### CountDownLatch （倒计时器）
###### 实战
- 某一线程在开始运行前等待 n 个线程执行完毕 : 将 CountDownLatch 的计数器初始化为 n （new CountDownLatch(n)），每当一个任务线程执行完毕，就将计数器减 1 （countdownlatch.countDown()），当计数器的值变为 0 时，在 CountDownLatch 上 await() 的线程就会被唤醒。一个典型应用场景就是启动一个服务时，主线程需要等待多个组件加载完毕，之后再继续执行。
- 实现多个线程开始执行任务的最大并行性：注意是并行性，不是并发，强调的是多个线程在某一时刻同时开始执行。类似于赛跑，将多个线程放到起点，等待发令枪响，然后同时开跑。做法是初始化一个共享的 CountDownLatch 对象，将其计数器初始化为 1 （new CountDownLatch(1)），多个线程在开始执行任务前首先 coundownlatch.await()，当主线程调用 countDown() 时，计数器变为 0，多个线程同时被唤醒。
#### Atomic原子类总结
##### 引用类型原子类
- AtomicReference：引用类型原子类
- AtomicStampedReference：原子更新带有版本号的引用类型。该类将整数值与引用关联起来，可用于解决原子的更新数据和数据的版本号，可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。
- AtomicMarkableReference：原子更新带有标记的引用类型。该类将 boolean 标记与引用关联起来。
##### 对象的属性修改类型原子类
要想原子地更新对象的属性需要两步。第一步，因为对象的属性修改类型原子类都是抽象类，所以每次使用都必须使用静态方法 newUpdater()创建一个更新器，并且需要设置想要更新的类和属性。第二步，更新的对象属性必须使用 public volatile 修饰符。
#### ThreadLocal 详解
ThreadLocalMap有点类似HashMap的结构，只是HashMap是由数组+链表实现的，而ThreadLocalMap中并没有链表结构。
##### GC 之后 key 是否为 null？
因为题目说的是在做 ThreadLocal.get() 操作，证明其实还是有强引用存在的，所以 key 并不为 null。如果我们的强引用不存在的话，那么 key 就会被回收，也就是会出现我们 value 没被回收，key 被回收，导致 value 永远存在，出现内存泄漏。
##### ThreadLocalMap Hash 算法
每当创建一个ThreadLocal对象，这个ThreadLocal.nextHashCode 这个值就会增长 0x61c88647 。
这个值很特殊，它是斐波那契数 也叫 黄金分割数。hash增量为 这个数字，带来的好处就是 hash 分布非常均匀。
而 ThreadLocalMap 中并没有链表结构，所以这里不能使用 HashMap 解决冲突的方式了。采用线性探测。
##### ThreadLocalMap扩容机制
如果执行完启发式清理工作后，未清理到任何数据，且当前散列数组中Entry的数量已经达到了列表的扩容阈值(len*2/3)，就开始执行rehash()逻辑
##### InheritableThreadLocal
一般我们做异步化处理都是使用的线程池，而InheritableThreadLocal是在new Thread中的init()方法给赋值的，而线程池是线程复用的逻辑，所以这里会存在问题。阿里巴巴开源了一个TransmittableThreadLocal组件就可以解决这个问题。
#### CompletableFuture 详解
##### CompletableFuture 常见操作
###### 创建 CompletableFuture
- 通过 new 关键字。
假设在未来的某个时刻，我们得到了最终的结果。这时，我们可以调用 complete() 方法为其传入结果，这表示 resultFuture 已经被完成了。
如果你已经知道计算的结果的话，可以使用静态方法 completedFuture() 来创建 CompletableFuture 。
- 基于 CompletableFuture 自带的静态工厂方法：runAsync()、supplyAsync() 。
runAsync() 方法接受的参数是 Runnable ，这是一个函数式接口，不允许返回值。当你需要异步操作且不关心返回结果的时候可以使用 runAsync() 方法。
supplyAsync() 方法接受的参数是 Supplier<U> ，这也是一个函数式接口，U 是返回结果值的类型。
###### 处理异步结算的结果
- thenApply() 方法接受一个 Function 实例，用它来处理结果。
```
CompletableFuture<String> future = CompletableFuture.completedFuture("hello!")
        .thenApply(s -> s + "world!");
assertEquals("hello!world!", future.get());
// 这次调用将被忽略。
future.thenApply(s -> s + "nice!");
assertEquals("hello!world!", future.get());

CompletableFuture<String> future = CompletableFuture.completedFuture("hello!")
        .thenApply(s -> s + "world!").thenApply(s -> s + "nice!");
assertEquals("hello!world!nice!", future.get());
```
如果你不需要从回调函数中获取返回结果，可以使用 thenAccept() 或者 thenRun()。这两个方法的区别在于 thenRun() 不能访问异步计算的结果。
###### 异常处理
你可以通过 handle() 方法来处理任务执行过程中可能出现的抛出异常的情况。
你还可以通过 exceptionally() 方法来处理异常情况。
###### 组合 CompletableFuture
你可以使用 thenCompose() 按顺序链接两个 CompletableFuture 对象，实现异步的任务链。它的作用是将前一个任务的返回结果作为下一个任务的输入参数，从而形成一个依赖关系。
也可以使用 thenCombine() 会在两个任务都执行完成后，把两个任务的结果合并。两个任务是并行执行的，它们之间并没有先后依赖顺序。
如果我们想要实现 task1 和 task2 中的任意一个任务执行完后就执行 task3 的话，可以使用 acceptEither()。
###### 并行运行多个 CompletableFuture
allOf() 方法会等到所有的 CompletableFuture 都运行完成之后再返回。join() 可以让程序等future1 和 future2 都运行完了之后再继续执行。
anyOf() 方法不会等待所有的 CompletableFuture 都运行完成之后再返回，只要有一个执行完成即可！
##### CompletableFuture 使用建议
- 使用自定义线程池，可以提高并发度和灵活性。
- 尽量避免使用 get()，get()方法是阻塞的，尽量避免使用。如果必须要使用的话，需要添加超时时间。
#### 虚拟线程极简入门
##### 缺点
- 不适用于计算密集型任务： 虚拟线程适用于 I/O 密集型任务，但不适用于计算密集型任务，因为密集型计算始终需要 CPU 资源作为支持。
- 依赖于语言或库的支持： 协程需要编程语言或库提供支持。不是所有编程语言都原生支持协程。比如 Java 实现的虚拟线程。
##### 四种创建虚拟线程的方法
- 使用 Thread.startVirtualThread() 创建
- 使用 Thread.ofVirtual().unstarted() 创建
- 使用 Thread.ofVirtual().factory().newThread() 创建
- 使用 Executors.newVirtualThreadPerTaskExecutor().submit()创建
## IO
### Java IO 基础知识总结
#### IO 流简介
Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。 
- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。
##### 为什么 I/O 流操作要分为字节流操作和字符流操作呢？
- 字符流是由 Java 虚拟机将字节转换得到的，这个过程还算是比较耗时。
- 如果我们不知道编码类型就很容易出现乱码问题。
字符流默认采用的是 Unicode 编码，我们可以通过构造方法自定义编码。
常用字符编码所占字节数？utf8 :英文占 1 字节，中文占 3 字节，unicode：任何字符都占 2 个字节，gbk：英文占 1 字节，中文占 2 字节。
#### 随机访问流
RandomAccessFile 的 seek(long pos) 方法来设置文件指针的偏移量（距文件开头 pos 个字节处）。
RandomAccessFile 的 write 方法在写入对象的时候如果对应的位置已经有数据的话，会将其覆盖掉。
### Java IO 设计模式总结
#### 装饰器模式
#### 适配器模式
#### 工厂模式
#### 观察者模式
### Java IO 模型详解
IO 多路复用模型中，线程首先发起 select 调用，询问内核数据是否准备就绪，等内核把数据准备好了，用户线程再发起 read 调用。read 调用的过程（数据从内核空间 -> 用户空间）还是阻塞的。
目前支持 IO 多路复用的系统调用，有 select，epoll 等等。select 系统调用，目前几乎在所有的操作系统上都有支持。
- select 调用：内核提供的系统调用，它支持一次查询多个系统调用的可用状态。几乎所有的操作系统都支持。
- epoll 调用：linux 2.6 内核，属于 select 调用的增强版本，优化了 IO 的执行效率。
Java 中的 NIO ，有一个非常重要的选择器 ( Selector ) 的概念，也可以被称为 多路复用器。通过它，只需要一个线程便可以管理多个客户端连接。当客户端数据到了之后，才会为其服务。
### Java NIO 核心知识总结
需要注意：使用 NIO 并不一定意味着高性能，它的性能优势主要体现在高并发和高延迟的网络环境下。当连接数较少、并发程度较低或者网络传输速度较快时，NIO 的性能并不一定优于传统的 BIO 。
#### NIO 核心组件
- Buffer（缓冲区）：NIO 读写数据都是通过缓冲区进行操作的。读操作的时候将 Channel 中的数据填充到 Buffer 中，而写操作时将 Buffer 中的数据写入到 Channel 中。
1. 容量（capacity）：Buffer可以存储的最大数据量，Buffer创建时设置且不可改变。
2. 界限（limit）：Buffer 中可以读/写数据的边界。写模式下，limit 代表最多能写入的数据，一般等于 capacity（可以通过limit(int newLimit)方法设置）；读模式下，limit 等于 Buffer 中实际写入的数据大小。
3. 位置（position）：下一个可以被读写的数据的位置（索引）。从写操作模式到读操作模式切换的时候（flip），position 都会归零，这样就可以从头开始读写了。如果要再次切换回写模式，可以调用 clear() 或者 compact() 方法。
4. 标记（mark）：Buffer允许将位置直接定位到该标记处，这是一个可选属性。
- Channel（通道）：Channel 是一个双向的、可读可写的数据传输通道，NIO 通过 Channel 来实现数据的输入输出。通道是一个抽象的概念，它可以代表文件、套接字或者其他数据源之间的连接。
BIO 中的流是单向的，分为各种 InputStream（输入流）和 OutputStream（输出流），数据只是在一个方向上传输。通道与流的不同之处在于通道是双向的，它可以用于读、写或者同时用于读写。
另外，因为 Channel 是全双工的，所以它可以比流更好地映射底层操作系统的 API。特别是在 UNIX 网络编程模型中，底层操作系统的通道都是全双工的，同时支持读写操作。
- Selector（选择器）：允许一个线程处理多个 Channel，基于事件驱动的 I/O 多路复用模型。所有的 Channel 都可以注册到 Selector 上，由 Selector 来分配线程来处理事件。
一个多路复用器 Selector 可以同时轮询多个 Channel，由于 JDK 使用了 epoll() 代替传统的 select 实现，所以它并没有最大连接句柄 1024/2048 的限制。这也就意味着只需要一个线程负责 Selector 的轮询，就可以接入成千上万的客户端。
Selector 可以监听以下四种事件类型：
1. SelectionKey.OP_ACCEPT：表示通道接受连接的事件，这通常用于 ServerSocketChannel。
2. SelectionKey.OP_CONNECT：表示通道完成连接的事件，这通常用于 SocketChannel。
3. SelectionKey.OP_READ：表示通道准备好进行读取的事件，即有数据可读。
4. SelectionKey.OP_WRITE：表示通道准备好进行写入的事件，即可以写入数据。
使用Selector 实现网络读写的简单示例
```
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;

public class NioSelectorExample {

  public static void main(String[] args) {
    try {
      ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
      serverSocketChannel.configureBlocking(false);
      serverSocketChannel.socket().bind(new InetSocketAddress(8080));

      Selector selector = Selector.open();
      // 将 ServerSocketChannel 注册到 Selector 并监听 OP_ACCEPT 事件
      serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

      while (true) {
        int readyChannels = selector.select();

        if (readyChannels == 0) {
          continue;
        }

        Set<SelectionKey> selectedKeys = selector.selectedKeys();
        Iterator<SelectionKey> keyIterator = selectedKeys.iterator();

        while (keyIterator.hasNext()) {
          SelectionKey key = keyIterator.next();

          if (key.isAcceptable()) {
            // 处理连接事件
            ServerSocketChannel server = (ServerSocketChannel) key.channel();
            SocketChannel client = server.accept();
            client.configureBlocking(false);

            // 将客户端通道注册到 Selector 并监听 OP_READ 事件
            client.register(selector, SelectionKey.OP_READ);
          } else if (key.isReadable()) {
            // 处理读事件
            SocketChannel client = (SocketChannel) key.channel();
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            int bytesRead = client.read(buffer);

            if (bytesRead > 0) {
              buffer.flip();
              System.out.println("收到数据：" +new String(buffer.array(), 0, bytesRead));
              // 将客户端通道注册到 Selector 并监听 OP_WRITE 事件
              client.register(selector, SelectionKey.OP_WRITE);
            } else if (bytesRead < 0) {
              // 客户端断开连接
              client.close();
            }
          } else if (key.isWritable()) {
            // 处理写事件
            SocketChannel client = (SocketChannel) key.channel();
            ByteBuffer buffer = ByteBuffer.wrap("Hello, Client!".getBytes());
            client.write(buffer);

            // 将客户端通道注册到 Selector 并监听 OP_READ 事件
            client.register(selector, SelectionKey.OP_READ);
          }

          keyIterator.remove();
        }
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```
#### NIO 零拷贝
零拷贝的常见实现技术有： mmap+write、sendfile和 sendfile + DMA gather copy 。
可以看出，无论是传统的 I/O 方式，还是引入了零拷贝之后，2 次 DMA(Direct Memory Access) 拷贝是都少不了的。因为两次 DMA 都是依赖硬件完成的。零拷贝主要是减少了 CPU 拷贝及上下文的切换。
Java 对零拷贝的支持：
- MappedByteBuffer 是 NIO 基于内存映射（mmap）这种零拷⻉⽅式的提供的⼀种实现，底层实际是调用了 Linux 内核的 mmap 系统调用。它可以将一个文件或者文件的一部分映射到内存中，形成一个虚拟内存文件，这样就可以直接操作内存中的数据，而不需要通过系统调用来读写文件。
- FileChannel 的transferTo()/transferFrom()是 NIO 基于发送文件（sendfile）这种零拷贝方式的提供的一种实现，底层实际是调用了 Linux 内核的 sendfile系统调用。它可以直接将文件数据从磁盘发送到网络，而不需要经过用户空间的缓冲区。
## JVM
### Java 内存区域详解（重点）
#### 运行时数据区域
Java 虚拟机规范对于运行时数据区域的规定是相当宽松的。以堆为例：堆可以是连续空间，也可以不连续。堆的大小可以固定，也可以在运行时按需扩展 。虚拟机实现者可以使用任何垃圾回收算法管理堆，甚至完全不进行垃圾收集也是可以的。
#### Java 虚拟机栈
栈绝对算的上是 JVM 运行时数据区域的一个核心，除了一些 Native 方法调用是通过本地方法栈实现的，其他所有的 Java 方法调用都是通过栈来实现的（也需要和其他运行时数据区域比如程序计数器配合）。
动态链接 主要服务一个方法需要调用其他方法的场景。Class 文件的常量池里保存有大量的符号引用比如方法引用的符号引用。当一个方法要调用其他方法，需要将常量池中指向方法的符号引用转化为其在内存地址中的直接引用。动态链接的作用就是为了将符号引用转换为调用方法的直接引用，这个过程也被称为 动态连接 。
#### 本地方法栈
虚拟机栈为虚拟机执行 Java 方法 （也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。
#### 堆
Java 世界中“几乎”所有的对象都在堆中分配，但是，随着 JIT 编译器的发展与逃逸分析技术逐渐成熟，栈上分配、标量替换优化技术将会导致一些微妙的变化，所有的对象都分配到堆上也渐渐变得不那么“绝对”了。从 JDK 1.7 开始已经默认开启逃逸分析，如果某些方法中的对象引用没有被返回或者未被外面使用（也就是未逃逸出去），那么对象可以直接在栈上分配内存。
#### 方法区
方法区属于是 JVM 运行时数据区域的一块逻辑区域，是各个线程共享的内存区域。方法区会存储已被虚拟机加载的 类信息、字段信息、方法信息、常量、静态变量、即时编译器编译后的代码缓存等数据。
##### 为什么要将永久代 (PermGen) 替换为元空间 (MetaSpace) 呢?
1. 整个永久代有一个 JVM 本身设置的固定大小上限，无法进行调整（也就是受到 JVM 内存的限制），而元空间使用的是本地内存，受本机可用内存的限制，虽然元空间仍旧可能溢出，但是比原来出现的几率会更小。
2. 元空间里面存放的是类的元数据，这样加载多少类的元数据就不由 MaxPermSize 控制了, 而由系统的实际可用空间来控制，这样能加载的类就更多了。
3. 在 JDK8，合并 HotSpot 和 JRockit 的代码时, JRockit 从来没有一个叫永久代的东西, 合并之后就没有必要额外的设置这么一个永久代的地方了。
4. 永久代会为 GC 带来不必要的复杂度，并且回收效率偏低。
与永久代很大的不同就是，如果不指定大小的话，随着更多类的创建，虚拟机会耗尽所有可用的系统内存。
#### 运行时常量池
#### 字符串常量池
##### JDK 1.7 为什么要将字符串常量池移动到堆中？
主要是因为永久代（方法区实现）的 GC 回收效率太低，只有在整堆收集 (Full GC)的时候才会被执行 GC。Java 程序中通常会有大量的被创建的字符串等待回收，将字符串常量池放到堆中，能够更高效及时地回收字符串内存。
#### 直接内存
直接内存是一种特殊的内存缓冲区，并不在 Java 堆或方法区中分配的，而是通过 JNI 的方式在本地内存上分配的。
#### HotSpot 虚拟机对象探秘
##### 对象的创建
1. 类加载检查
2. 分配内存
- 指针碰撞（使用该分配方式的 GC 收集器：Serial, ParNew）压缩内存的方式
- 空闲列表 (使用该分配方式的 GC 收集器：CMS) 
内存分配并发问题
- CAS+失败重试
- TLAB
3. 初始化零值
4. 设置对象头
虚拟机要对对象进行必要的设置，例如这个对象是哪个类的实例、如何才能找到类的元数据信息、对象的哈希码、对象的 GC 分代年龄等信息。 这些信息存放在对象头中。
5. 执行 init 方法
##### 对象的内存布局
- 对象头
Hotspot 虚拟机的对象头包括两部分信息，第一部分用于存储对象自身的运行时数据（哈希码、GC 分代年龄、锁状态标志等等），另一部分是类型指针，即对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例。
- 实例数据
实例数据部分是对象真正存储的有效信息
- 对齐填充
Hotspot 虚拟机的自动内存管理系统要求对象起始地址必须是 8 字节的整数倍
##### 对象的访问定位
对象的访问方式由虚拟机实现而定，目前主流的访问方式有：使用句柄、直接指针。
###### 句柄
Java 堆中将会划分出一块内存来作为句柄池，reference 中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与对象类型数据各自的具体地址信息。
###### 直接指针
reference 中存储的直接就是对象的地址。
使用句柄来访问的最大好处是 reference 中存储的是稳定的句柄地址，在对象被移动时只会改变句柄中的实例数据指针，而 reference 本身不需要修改。使用直接指针访问方式最大的好处就是速度快，它节省了一次指针定位的时间开销。
### JVM 垃圾回收详解（重点）
#### 死亡对象判断方法
- 引用计数法
这个方法实现简单，效率高，但是目前主流的虚拟机中并没有选择这个算法来管理内存，其最主要的原因是它很难解决对象之间循环引用的问题。
- 可达性分析算法
这个算法的基本思想就是通过一系列的称为 “GC Roots” 的对象作为起点，从这些节点开始向下搜索，节点所走过的路径称为引用链，当一个对象到 GC Roots 没有任何引用链相连的话，则证明此对象是不可用的，需要被回收。
##### 哪些对象可以作为 GC Roots 呢？
- 虚拟机栈(栈帧中的局部变量表)中引用的对象
- 本地方法栈(Native 方法)中引用的对象
- 方法区中类静态属性引用的对象
- 方法区中常量引用的对象
- 所有被同步锁持有的对象
- JNI（Java Native Interface）引用的对象
##### 对象可以被回收，就代表一定会被回收吗？
要真正宣告一个对象死亡，至少要经历两次标记过程；
筛选的条件是此对象是否有必要执行 finalize 方法。当对象没有覆盖 finalize 方法，或 finalize 方法已经被虚拟机调用过时，虚拟机将这两种情况视为没有必要执行。JDK9 版本及后续版本中各个类中的 finalize 方法会被逐渐弃用移除。
#### 引用类型总结
软引用（SoftReference）软引用可用来实现内存敏感的高速缓存。软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，JAVA 虚拟机就会把这个软引用加入到与之关联的引用队列中。
在程序设计中一般很少使用弱引用与虚引用，使用软引用的情况较多，这是因为软引用可以加速 JVM 对垃圾内存的回收速度，可以维护系统的运行安全，防止内存溢出（OutOfMemory）等问题的产生。
##### 如何判断一个类是无用的类？
- 该类所有的实例都已经被回收，也就是 Java 堆中不存在该类的任何实例。
- 加载该类的 ClassLoader 已经被回收。
- 该类对应的 java.lang.Class 对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。
#### 垃圾收集算法
- 标记-清除算法(效率低)
- 复制算法(不适合老年代)
- 标记-整理算法(效率低，适合老年代)
#### 垃圾收集器
- Serial 收集器
简单而高效, 运行在 Client 模式下的虚拟机来说是个不错的选择
- ParNew 收集器
它是许多运行在 Server 模式下的虚拟机的首要选择
- Parallel Scavenge 收集器
关注点是吞吐量（高效率的利用 CPU）
JDK1.8 默认收集器
- Serial Old 收集器
- Parallel Old 收集器
重吞吐量以及 CPU 资源的场合，都可以优先考虑 Parallel Scavenge 收集器和 Parallel Old 收集器。
- CMS 收集器
是一种以获取最短回收停顿时间为目标的收集器。它非常符合在注重用户体验的应用上使用。
是 HotSpot 虚拟机第一款真正意义上的并发收集器，它第一次实现了让垃圾收集线程与用户线程（基本上）同时工作。
1. 初始标记
2. 并发标记
同时开启 GC 和用户线程
3. 重新标记
停顿时间一般会比初始标记阶段的时间稍长，远远比并发标记阶段时间短
4. 并发清除
开启用户线程，同时 GC 线程开始对未标记的区域做清扫
缺点
1. 对 CPU 资源敏感
2. 无法处理浮动垃圾
3. 它使用的回收算法-“标记-清除”算法会导致收集结束时会有大量空间碎片产生
从 JDK9 开始，CMS 收集器已被弃用。
- G1 收集器
可预测的停顿：这是 G1 相对于 CMS 的另一个大优势，
从 JDK9 开始，G1 垃圾收集器成为了默认的垃圾收集器。
- ZGC 收集器
ZGC 在 Java15 已经可以正式使用了。
在 Java21 中，引入了分代 ZGC，暂停时间可以缩短到 1 毫秒以内。
### 类文件结构详解（重点）
JVM 可以理解的代码就叫做字节码（即扩展名为 .class 的文件），它不面向任何特定的处理器，只面向虚拟机。
### 类加载过程详解
系统加载 Class 类型的文件主要三步：加载->连接->初始化。连接过程又可分为三步：验证->准备->解析。
#### 加载
- 通过全类名获取定义此类的二进制字节流
- 将字节流所代表的静态存储结构转换为方法区的运行时数据结构
- 在内存中生成一个代表该类的 Class 对象，作为方法区这些数据的访问入口
每个 Java 类都有一个引用指向加载它的 ClassLoader。不过，数组类不是通过 ClassLoader 创建的，而是 JVM 在需要的时候自动创建的，数组类通过getClassLoader()方法获取 ClassLoader 的时候和该数组的元素类型的 ClassLoader 是一致的。
加载阶段与连接阶段的部分动作(如一部分字节码文件格式验证动作)是交叉进行的，加载阶段尚未结束，连接阶段可能就已经开始了。
#### 验证
验证阶段也不是必须要执行的阶段。在生产环境的实施阶段就可以考虑使用 -Xverify:none 参数来关闭大部分的类验证措施，以缩短虚拟机类加载的时间。
验证阶段主要由四个检验阶段组成：
- 文件格式验证（Class 文件格式检查）
- 元数据验证（字节码语义检查）
- 字节码验证（程序语义检查）
- 符号引用验证（类的正确性检查）
文件格式验证这一阶段是基于该类的二进制字节流进行的，其余三个验证阶段都是基于方法区的存储结构上进行的，不会再直接读取、操作字节流了。
#### 准备
准备阶段是正式为类变量分配内存并设置类变量初始值的阶段
#### 解析
解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程
#### 初始化
初始化阶段是执行初始化方法 <clinit> ()方法的过程，是类加载的最后一步，这一步 JVM 才开始真正执行类中定义的 Java 程序代码(字节码)。<clinit> ()方法是编译之后自动生成的。
虚拟机会自己确保其在多线程环境中的安全性。因为 <clinit> () 方法是带锁线程安全，所以在多线程环境下进行类初始化的话可能会引起多个线程阻塞，并且这种阻塞很难被发现。
只有 6 种情况下，必须对类进行初始化(只有主动去使用类才会初始化类)：
- 当遇到 new、 getstatic、putstatic 或 invokestatic 这 4 条字节码指令时，比如 new 一个类，读取一个静态字段(未被 final 修饰)、或调用一个类的静态方法时。
- 使用 java.lang.reflect 包的方法对类进行反射调用时如 Class.forName("..."), newInstance() 等等。
- 初始化一个类，如果其父类还未初始化，则先触发该父类的初始化。
- 当虚拟机启动时，用户需要定义一个要执行的主类 (包含 main 方法的那个类)，虚拟机会先初始化这个类
- MethodHandle 和 VarHandle 可以看作是轻量级的反射调用机制，而要想使用这 2 个调用，就必须先使用 findStaticVarHandle 来初始化要调用的类。
- 当一个接口中定义了 JDK8 新加入的默认方法（被 default 关键字修饰的接口方法）时，如果有这个接口的实现类发生了初始化，那该接口要在其之前被初始化。
### 类加载器详情（重点）
#### 类加载器加载规则
JVM 启动的时候，并不会一次性加载所有的类，而是根据需要去动态加载。
对于一个类加载器来说，相同二进制名称的类只会被加载一次。
#### 类加载器总结
- BootstrapClassLoader
由 C++实现，通常表示为 null，并且没有父级，主要用来加载 JDK 内部的核心类库（ %JAVA_HOME%/lib目录下的 rt.jar、resources.jar、charsets.jar等 jar 包和类）以及被 -Xbootclasspath参数指定的路径下的所有类。
- ExtensionClassLoader
主要负责加载 %JRE_HOME%/lib/ext 目录下的 jar 包和类以及被 java.ext.dirs 系统变量所指定的路径下的所有类。
- AppClassLoader
Java 9 引入了模块系统，并且略微更改了上述的类加载器。扩展类加载器被改名为平台类加载器（platform class loader）。Java SE 中除了少数几个关键模块，比如说 java.base 是由启动类加载器加载之外，其他的模块均由平台类加载器所加载。
##### 自定义类加载器
如果我们不想打破双亲委派模型，就重写 ClassLoader 类中的 findClass() 方法即可，无法被父类加载器加载的类最终会通过这个方法被加载。但是，如果想打破双亲委派模型则需要重写 loadClass() 方法。
JVM 判定两个 Java 类是否相同的具体规则：JVM 不仅要看类的全名是否相同，还要看加载此类的类加载器是否一样。只有两者都相同的情况，才认为两个类是相同的。即使两个类来源于同一个 Class 文件，被同一个虚拟机加载，只要加载它们的类加载器不同，那这两个类就必定不相同。
### 最重要的JVM参数总结
#### 堆内存相关
##### 显式指定堆内存–Xms和-Xmx
##### 显式新生代内存(Young Generation)
- 通过-XX:NewSize和-XX:MaxNewSize指定
- 通过-Xmn<young size>[unit]指定
- 通过-XX:NewRatio=1
##### 显式指定永久代/元空间的大小
-XX:MetaspaceSize=N 
-XX:MaxMetaspaceSize=N #设置 Metaspace 的最大大小
无论 -XX:MetaspaceSize 配置什么值，对于 64 位 JVM 来说，Metaspace 的初始容量都是 21807104（约 20.8m）。
MetaspaceSize 表示 Metaspace 使用过程中触发 Full GC 的阈值，只对触发起作用。
#### GC 日志记录
```
# 必选
# 打印基本 GC 信息
-XX:+PrintGCDetails
-XX:+PrintGCDateStamps
# 打印对象分布
-XX:+PrintTenuringDistribution
# 打印堆数据
-XX:+PrintHeapAtGC
# 打印Reference处理信息
# 强引用/弱引用/软引用/虚引用/finalize 相关的方法
-XX:+PrintReferenceGC
# 打印STW时间
-XX:+PrintGCApplicationStoppedTime

# 可选
# 打印safepoint信息，进入 STW 阶段之前，需要要找到一个合适的 safepoint
-XX:+PrintSafepointStatistics
-XX:PrintSafepointStatisticsCount=1

# GC日志输出的文件路径
-Xloggc:/path/to/gc-%t.log
# 开启日志文件分割
-XX:+UseGCLogFileRotation
# 最多分割几个文件，超过之后从头文件开始写
-XX:NumberOfGCLogFiles=14
# 每个文件上限大小，超过就触发分割
-XX:GCLogFileSize=50M
```
#### 处理 OOM
```
-XX:+HeapDumpOnOutOfMemoryError
-XX:HeapDumpPath=./java_pid<pid>.hprof
-XX:OnOutOfMemoryError="< cmd args >;< cmd args >"
-XX:+UseGCOverheadLimit
```
#### 其他
-XX:+UseStringDeduplication : Java 8u20 引入了这个 JVM 参数，通过创建太多相同 String 的实例来减少不必要的内存使用; 这通过将重复 String 值减少为单个全局 char [] 数组来优化堆内存。
-XX:+UseStringCache : 启用 String 池中可用的常用分配字符串的缓存。
-XX:+UseCompressedStrings : 对 String 对象使用 byte [] 类型，该类型可以用纯 ASCII 格式表示。-XX:+OptimizeStringConcat : 它尽可能优化字符串串联操作。
### JDK监控和故障处理工具总结
#### JDK 命令行工具
- jps (JVM Process Status）: 类似 UNIX 的 ps 命令。用于查看所有 Java 进程的启动类、传入参数和 Java 虚拟机参数等信息；
1. -l:输出主类的全名，如果进程执行的是 Jar 包，输出 Jar 路径
2. -v：输出虚拟机进程启动时 JVM 参数
3. -m：输出传递给 Java 进程 main() 函数的参数
- jstat（JVM Statistics Monitoring Tool）: 用于收集 HotSpot 虚拟机各方面的运行数据;
1. jstat -class vmid：显示 ClassLoader 的相关信息；
2. jstat -compiler vmid：显示 JIT 编译的相关信息；
3. jstat -gc vmid：显示与 GC 相关的堆信息；
4. jstat -gccapacity vmid：显示各个代的容量及使用情况；
5. jstat -gcnew vmid：显示新生代信息；
6. jstat -gcnewcapcacity vmid：显示新生代大小与使用情况；
7. jstat -gcold vmid：显示老年代和永久代的行为统计，从 jdk1.8 开始,该选项仅表示老年代，因为永久代被移除了；
8. jstat -gcoldcapacity vmid：显示老年代的大小；
9. jstat -gcpermcapacity vmid：显示永久代大小，从 jdk1.8 开始,该选项不存在了，因为永久代被移除了；
10. jstat -gcutil vmid：显示垃圾收集信息；
- jinfo (Configuration Info for Java) : Configuration Info for Java,显示虚拟机配置信息;\
jinfo -flag [+|-]name vmid 开启或者关闭对应名称的参数
- jmap (Memory Map for Java) : 生成堆转储快照;
jmap -dump:format=b,file=C:\Users\SnailClimb\Desktop\heap.hprof 17340
- jhat (JVM Heap Dump Browser) : 用于分析 heapdump 文件，它会建立一个 HTTP/HTML 服务器，让用户可以在浏览器上查看分析结果;
jhat C:\Users\SnailClimb\Desktop\heap.hprof
- jstack (Stack Trace for Java) : 生成虚拟机当前时刻的线程快照，线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合。
jstack 命令已经帮我们找到发生死锁的线程的具体信息。
## 新特性
### Java 8
#### Interface
为了解决接口的修改与现有的实现不兼容的问题。新 interface 的方法可以用default 或 static修饰，这样就可以有方法体，实现类也不必重写此方法。
- default修饰的方法，是普通实例方法，可以用this调用，可以被子类继承、重写。
- static修饰的方法，使用上和一般类静态方法一样。但它不能被子类继承，只能用Interface调用。
#### functional interface 函数式接口
@FunctionalInterface 注解，提供函数式编程
#### Lambda 表达式
##### 方法的引用
Java 8 允许使用 :: 关键字来传递方法或者构造函数引用，无论如何，表达式返回的类型必须是 functional-interface。
```
//1.调用静态函数，返回类型必须是functional-interface
LambdaInterface t = LambdaClass::staticF;

//2.实例方法调用
LambdaClass lambdaClass = new LambdaClass();
LambdaInterface lambdaInterface = lambdaClass::f;

//3.超类上的方法调用
LambdaInterface superf = super::sf;

//4. 构造方法调用
LambdaInterface tt = LambdaClassSuper::new;
```
#### Stream
在 Java 9 中，Stream 中增加了新的方法 ofNullable()、dropWhile()、takeWhile() 以及 iterate() 方法的重载方法。
ofNullable() 方 法允许我们创建一个单元素的 Stream，可以包含一个非空元素，也可以创建一个空 Stream。
takeWhile() 方法可以从 Stream 中依次获取满足条件的元素，直到不满足条件为止结束获取。
iterate() 方法的新重载方法提供了一个 Predicate 参数 (判断条件)来决定什么时候结束迭代
```
//对数组的统计，比如用
List<Integer> number = Arrays.asList(1, 2, 5, 4);

IntSummaryStatistics statistics = number.stream().mapToInt((x) -> x).summaryStatistics();
System.out.println("列表中最大的数 : "+statistics.getMax());
System.out.println("列表中最小的数 : "+statistics.getMin());
System.out.println("平均数 : "+statistics.getAverage());
System.out.println("所有数之和 : "+statistics.getSum());

//concat 合并流
List<String> strings2 = Arrays.asList("xyz", "jqx");
Stream.concat(strings2.stream(),strings.stream()).count();

//注意 一个Stream只能操作一次，不能断开，否则会报错。
Stream stream = strings.stream();
//第一次使用
stream.limit(2);
//第二次使用
stream.forEach(System.out::println);
//报错 java.lang.IllegalStateException: stream has already been operated upon or closed
}

```
##### 延迟执行
filter 中的方法并没有立刻执行，而是等调用count()方法后才执行。
##### Match(匹配)
- anyMatch
- allMatch
- noneMatch
##### Reduce
```
// 字符串连接，concat = "ABCD"
String concat = Stream.of("A", "B", "C", "D").reduce("", String::concat);
// 求最小值，minValue = -3.0
double minValue = Stream.of(-1.5, 1.0, -3.0, -2.0).reduce(Double.MAX_VALUE, Double::min);
// 求和，sumValue = 10, 有起始值
int sumValue = Stream.of(1, 2, 3, 4).reduce(0, Integer::sum);
// 求和，sumValue = 10, 无起始值
sumValue = Stream.of(1, 2, 3, 4).reduce(Integer::sum).get();
// 过滤，字符串连接，concat = "ace"
concat = Stream.of("a", "B", "c", "D", "e", "F").
 filter(x -> x.compareTo("Z") > 0).
 reduce("", String::concat);
```
##### Map
```
map.computeIfPresent(3, (num, val) -> val + num);
map.getOrDefault(42, "not found");  // not found
map.merge(9, "val9", (value, newValue) -> value.concat(newValue));
map.get(9);             // val9
map.merge(9, "concat", (value, newValue) -> value.concat(newValue));
map.get(9);             // val9concat
```
Merge 做的事情是如果键名不存在则插入，否则对原键对应的值做合并操作并重新插入到 map 中。

#### Optional 
在 Java 9 中，Optional 类中新增了 ifPresentOrElse()、or() 和 stream() 等方法
ifPresentOrElse() 方法接受两个参数 Consumer 和 Runnable ，如果 Optional 不为空调用 Consumer 参数，为空则调用 Runnable 参数。
or() 方法接受一个 Supplier 参数 ，如果 Optional 为空则返回 Supplier 参数指定的 Optional 值。
在 Java 10 中，Optional 新增了orElseThrow()方法来在没有值时抛出指定的异常。
在 Java 11 中，新增了isEmpty()方法来判断指定的 Optional 对象是否为空。
注意NPE场景
- return 包装数据类型的对象
- 数据库的查询结果
- 集合里的元素即使 isNotEmpty，取出的数据元素也可能为 null
- 远程调用返回对象时，一律要求进行空指针判断
- 对于 Session 中获取的数据
- 级联调用 obj.getA().getB().getC()；一连串调用
```
Optional.ofNullable(zoo).map(o -> o.getDog()).map(d -> d.getAge()).ifPresent(age ->
    System.out.println(age)
);
```
##### 如何创建一个 Optional
ofNullable 方法和of方法唯一区别就是当 value 为 null 时，ofNullable 返回的是EMPTY，of 会抛出 NullPointerException 异常。如果需要把 NullPointerException 暴漏出来就用 of，否则就用 ofNullable。
##### map() 和 flatMap() 有什么区别的？
map 和 flatMap 都是将一个函数应用于集合中的每个元素，但不同的是map返回一个新的集合，flatMap是将每个元素都映射为一个集合，最后再将这个集合展平。
在实际应用场景中，如果map返回的是数组，那么最后得到的是一个二维数组，使用flatMap就是为了将这个二维数组展平变成一个一维数组。
```
List<String[]> mapResult = listOfArrays.stream()
        .map(array -> Arrays.stream(array).map(String::toUpperCase).toArray(String[]::new))
        .collect(Collectors.toList());

List<String> flatMapResult = listOfArrays.stream()
        .flatMap(array -> Arrays.stream(array).map(String::toUpperCase))
        .collect(Collectors.toList());
}
```
在Optional里面，当使用map()时，如果映射函数返回的是一个普通值，它会将这个值包装在一个新的Optional中。而使用flatMap时，如果映射函数返回的是一个Optional，它会将这个返回的Optional展平，不再包装成嵌套的Optional。
```
// 使用flatMap的代码
String cityUsingFlatMap = getUserById(userId)
        .flatMap(OptionalExample::getAddressByUser)
        .map(Address::getCity)
        .orElse("Unknown");

// 不使用flatMap的代码
Optional<Optional<Address>> optionalAddress = getUserById(userId)
        .map(OptionalExample::getAddressByUser);

String cityWithoutFlatMap;
if (optionalAddress.isPresent()) {
    Optional<Address> addressOptional = optionalAddress.get();
    if (addressOptional.isPresent()) {
        Address address = addressOptional.get();
        cityWithoutFlatMap = address.getCity();
    } else {
        cityWithoutFlatMap = "Unknown";
    }
} else {
    cityWithoutFlatMap = "Unknown";
}
```
在Stream和Optional中正确使用flatMap可以减少很多不必要的代码。
##### 获取 value
```
/**
* 如果value != null 返回value，否则返回T
*/
public T orElse(T other) {
    return value != null ? value : other;
}
```
##### 小结
看完 Optional 源码，Optional 的方法真的非常简单，值得注意的是如果坚决不想看见 NPE，就不要用 of()、 get()、flatMap(..)。最后再综合用一下 Optional 的高频方法。
```
Optional.ofNullable(zoo).map(o -> o.getDog()).map(d -> d.getAge()).filter(v->v==1).orElse(3);
```
#### Date-Time API
这是对java.util.Date强有力的补充，解决了 Date 类的大部分痛点：
- 非线程安全
- 时区处理麻烦
- 各种格式化、和时间计算繁琐
- 设计有缺陷，Date 类同时包含日期和时间；还有一个 java.sql.Date，容易混淆。
##### 格式化
Java 8 之前:
```
Date now = new Date();
//format yyyy-MM-dd HH:mm:ss
SimpleDateFormat sdfdt = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String datetime = sdfdt.format(now);
System.out.println(String.format("dateTime format : %s", datetime));
```
Java 8 之后:
```
//format yyyy-MM-dd HH:mm:ss
LocalDateTime dateTime = LocalDateTime.now();
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String dateTimeStr = dateTime.format(dateTimeFormatter);
System.out.println(String.format("dateTime format : %s", dateTimeStr));
```
##### 日期计算
Java 8 之前:
```
Calendar ca = Calendar.getInstance();
ca.add(Calendar.DATE, 7);
Date d = ca.getTime();
```
Java 8 之后:
```
//方法1
LocalDate after = localDate.plus(1, ChronoUnit.WEEKS);
//方法2
LocalDate after2 = localDate.plusWeeks(1);
```
##### 获取指定日期
Java 8 之前:
```
//获取当前月最后一天
Calendar ca = Calendar.getInstance();
ca.set(Calendar.DAY_OF_MONTH, ca.getActualMaximum(Calendar.DAY_OF_MONTH));
String last = format.format(ca.getTime());

//当年最后一天
Calendar currCal = Calendar.getInstance();
Calendar calendar = Calendar.getInstance();
calendar.clear();
calendar.set(Calendar.YEAR, currCal.get(Calendar.YEAR));
calendar.roll(Calendar.DAY_OF_YEAR, -1);
Date time = calendar.getTime();
```
Java 8 之后:
```
LocalDate today = LocalDate.now();
//获取当前月第一天：
LocalDate firstDayOfThisMonth = today.with(TemporalAdjusters.firstDayOfMonth());
// 取本月最后一天
LocalDate lastDayOfThisMonth = today.with(TemporalAdjusters.lastDayOfMonth());
//取下一天：
LocalDate nextDay = lastDayOfThisMonth.plusDays(1);
//当年最后一天
LocalDate lastday = today.with(TemporalAdjusters.lastDayOfYear());
//2021年最后一个周日，如果用Calendar是不得烦死。
LocalDate lastMondayOf2021 = LocalDate.parse("2021-12-31").with(TemporalAdjusters.lastInMonth(DayOfWeek.SUNDAY));   
```
java.time.temporal.TemporalAdjusters 里面还有很多便捷的算法，这里就不带大家看 Api 了，都很简单，看了秒懂。
##### 时区
java.util.Date 本身并不支持国际化，需要借助 TimeZone。
```
Date date = new Date();
SimpleDateFormat bjSdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
//上海时区
bjSdf.setTimeZone(TimeZone.getTimeZone("Asia/Shanghai"));
System.out.println("毫秒数:" + date.getTime() + ", 上海时间:" + bjSdf.format(date));
```
在新特性中引入了 java.time.ZonedDateTime 来表示带时区的时间。它可以看成是 LocalDateTime + ZoneId。
```
//当前时区时间
ZonedDateTime zonedDateTime = ZonedDateTime.now();
System.out.println("当前时区时间: " + zonedDateTime);

//东京时间
ZoneId zoneId = ZoneId.of(ZoneId.SHORT_IDS.get("JST"));
ZonedDateTime tokyoTime = zonedDateTime.withZoneSameInstant(zoneId);
System.out.println("东京时间: " + tokyoTime);

//输出所有区域标识符
System.out.println(ZoneId.getAvailableZoneIds());
```
#### Annotations(注解)
在 Java 8 中支持多重注解了, Java 8 允许我们把同一个类型的注解使用多次，只需要给该注解标注一下@Repeatable即可。
```
@Retention(RetentionPolicy.RUNTIME)
@interface Hints {
    Hint[] value();
}
@Repeatable(Hints.class)
@interface Hint {
    String value();
}
```
老方法:
```
@Hints({@Hint("hint1"), @Hint("hint2")})
class Person {}
```
新方法:
```
@Hint("hint1")
@Hint("hint2")
class Person {}
```
### Java 9
#### JShell
##### JShell 的代码和普通的可编译代码，有什么不一样？
- 一旦语句输入完成，JShell 立即就能返回执行的结果，而不再需要编辑器、编译器、解释器。
- JShell 支持变量的重复声明，后面声明的会覆盖前面声明的。
- JShell 支持独立的表达式比如普通的加法运算 1 + 1。
#### 模块化系统
在引入了模块系统之后，JDK 被重新组织成 94 个模块。Java 应用可以通过新增的 jlink 工具 (Jlink 是随 Java 9 一起发布的新命令行工具。它允许开发人员为基于模块的 Java 应用程序创建自己的轻量级、定制的 JRE)，创建出只包含所依赖的 JDK 模块的自定义运行时镜像。这样可以极大的减少 Java 运行时环境的大小。
我们可以通过 exports 关键词精准控制哪些类可以对外开放使用，哪些类只能内部使用。
```
module my.module {
    //exports 公开指定包的所有公共成员
    exports com.my.package.name;
}

module my.module {
     //exports…to 限制访问的成员范围
    export com.my.package.name to com.specific.package;
}
```
#### 快速创建不可变集合
增加了List.of()、Set.of()、Map.of() 和 Map.ofEntries()等工厂方法来创建不可变集合（有点参考 Guava 的味道）：
```
List.of("Java", "C++");
Set.of("Java", "C++");
Map.of("Java", 1, "C++", 2);
```
#### String 存储结构优化
Java 8 及之前的版本，String 一直是用 char[] 存储。在 Java 9 之后，String 的实现改用 byte[] 数组存储字符串，节省了空间。
#### 接口私有方法
#### try-with-resources 增强
在 Java 9 之后，在 try-with-resources 语句中可以使用 effectively-final 变量。
什么是 effectively-final 变量？ 简单来说就是没有被 final 修饰但是值在初始化后从未更改的变量。
#### 进程 API
Java 9 增加了 java.lang.ProcessHandle 接口来实现对原生进程进行管理，尤其适合于管理长时间运行的进程。
#### 响应式流 （ Reactive Streams ）
在 Java 9 中的 java.util.concurrent.Flow 类中新增了反应式流规范的核心接口 。
#### 变量句柄
VarHandle 的出现替代了 java.util.concurrent.atomic 和 sun.misc.Unsafe 的部分操作。并且提供了一系列标准的内存屏障操作，用于更加细粒度的控制内存排序。在安全性、可用性、性能上都要优于现有的 API。
#### 其他
- 平台日志 API 改进：Java 9 允许为 JDK 和应用配置同样的日志实现。新增了 System.LoggerFinder 用来管理 JDK 使 用的日志记录器实现。JVM 在运行时只有一个系统范围的 LoggerFinder 实例。我们可以通过添加自己的 System.LoggerFinder 实现来让 JDK 和应用使用 SLF4J 等其他日志记录框架。
- CompletableFuture类增强：新增了几个新的方法（completeAsync ，orTimeout 等）。
- Nashorn 引擎的增强：Nashorn 是从 Java8 开始引入的 JavaScript 引擎，Java9 对 Nashorn 做了些增强，实现了一些 ES6 的新特性（Java 11 中已经被弃用）。
- I/O 流的新特性：增加了新的方法来读取和复制 InputStream 中包含的数据。
- 改进应用的安全性能：Java 9 新增了 4 个 SHA- 3 哈希算法，SHA3-224、SHA3-256、SHA3-384 和 SHA3-512。
- 改进方法句柄（Method Handle）：方法句柄从 Java7 开始引入，Java9 在类java.lang.invoke.MethodHandles 中新增了更多的静态方法来创建不同类型的方法句柄。
### Java 10
#### 局部变量类型推断(var)
由于太多 Java 开发者希望 Java 中引入局部变量推断，于是 Java 10 的时候它来了，也算是众望所归了！
var 关键字只能用于带有构造器的局部变量和 for 循环中。
 ```
var count=null; //❌编译不通过，不能声明为 null
var r = () -> Math.random();//❌编译不通过,不能声明为 Lambda表达式
var array = {1,2,3};//❌编译不通过,不能声明数组
 ```
var 并不会改变 Java 是一门静态类型语言的事实，编译器负责推断出类型。
#### 垃圾回收器接口
#### G1 并行 Full GC
Java9 的 G1 的 FullGC 依然是使用单线程去完成标记清除算法,这可能会导致垃圾回收期在无法回收内存的时候触发 Full GC。从 Java10 开始，G1 的 FullGC 改为并行的标记清除算法，同时会使用与年轻代回收和混合回收相同的并行工作线程数量，从而减少了 Full GC 的发生，以带来更好的性能提升、更大的吞吐量。
#### 集合增强
List，Set，Map 提供了静态方法copyOf()返回入参集合的一个不可变拷贝。
并且，java.util.stream.Collectors 中新增了静态方法，用于将流中的元素收集为不可变的集合。
```
var list = new ArrayList<>();
list.stream().collect(Collectors.toUnmodifiableList());
list.stream().collect(Collectors.toUnmodifiableSet());
```
#### 应用程序类数据共享(扩展 CDS 功能)
在 Java 5 中就已经引入了类数据共享机制 (Class Data Sharing，简称 CDS)，允许将一组类预处理为共享归档文件，以便在运行时能够进行内存映射以减少 Java 程序的启动时间，当多个 Java 虚拟机（JVM）共享相同的归档文件时，还可以减少动态内存的占用量，同时减少多个虚拟机在同一个物理或虚拟的机器上运行时的资源占用。
Java 10 在现有的 CDS 功能基础上再次拓展，以允许应用类放置在共享存档中。CDS 特性在原来的 bootstrap 类基础之上，扩展加入了应用类的 CDS 为 (Application Class-Data Sharing，AppCDS) 支持，大大加大了 CDS 的适用范围。其原理为：在启动时记录加载类的过程，写入到文本文件中，再次启动时直接读取此启动文本并加载。设想如果应用环境没有大的变化，启动速度就会得到提升。
#### 实验性的基于 Java 的 JIT 编译器
Graal 是一个基于 Java 语言编写的 JIT 编译器，是 JDK 9 中引入的实验性 Ahead-of-Time (AOT) 编译器的基础。
Oracle 的 HotSpot VM 便附带两个用 C++ 实现的 JIT compiler：C1 及 C2。在 Java 10 (Linux/x64, macOS/x64) 中，默认情况下 HotSpot 仍使用 C2，但通过向 java 命令添加 -XX:+UnlockExperimentalVMOptions -XX:+UseJVMCICompiler 参数便可将 C2 替换成 Graal。
#### 其他
- 线程-局部管控：Java 10 中线程管控引入 JVM 安全点的概念，将允许在不运行全局 JVM 安全点的情况下实现线程回调，由线程本身或者 JVM 线程来执行，同时保持线程处于阻塞状态，这种方式使得停止单个线程变成可能，而不是只能启用或停止所有线程
- 备用存储装置上的堆分配：Java 10 中将使得 JVM 能够使用适用于不同类型的存储机制的堆，在可选内存设备上进行堆内存分配
### Java 11
#### HTTP Client 标准化
Java 11 中，Http Client 通过 CompleteableFuture 提供非阻塞请求和响应语义。使用起来也很简单。
```
var request = HttpRequest.newBuilder()
    .uri(URI.create("https://javastack.cn"))
    .GET()
    .build();
var client = HttpClient.newHttpClient();

// 同步
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());

// 异步
client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
    .thenApply(HttpResponse::body)
    .thenAccept(System.out::println);
```
#### String 增强
#### ZGC(可伸缩低延迟垃圾收集器)
#### Lambda 参数的局部变量语法
Java11 开始允许开发者在 Lambda 表达式中使用 var 进行参数声明。
#### 启动单文件源代码程序
允许使用 Java 解释器直接执行 Java 源代码。源代码在内存中编译，然后由解释器执行，不需要在磁盘上生成 .class 文件了。唯一的约束在于所有相关的类必须定义在同一个 Java 文件中。
一定能程度上增强了使用 Java 来写脚本程序的能力。
#### 其他新特性
- 新的垃圾回收器 Epsilon：一个完全消极的 GC 实现，分配有限的内存资源，最大限度的降低内存占用和内存吞吐延迟时间
- 低开销的 Heap Profiling：Java 11 中提供一种低开销的 Java 堆分配采样方法，能够得到堆分配的 Java 对象信息，并且能够通过 JVMTI 访问堆信息
- TLS1.3 协议：Java 11 中包含了传输层安全性（TLS）1.3 规范（RFC 8446）的实现，替换了之前版本中包含的 TLS，包括 TLS 1.2，同时还改进了其他 TLS 功能，例如 OCSP 装订扩展（RFC 6066，RFC 6961），以及会话散列和扩展主密钥扩展（RFC 7627），在安全性和性能方面也做了很多提升
- 飞行记录器(Java Flight Recorder)：飞行记录器之前是商业版 JDK 的一项分析工具，但在 Java 11 中，其代码被包含到公开代码库中，这样所有人都能使用该功能了。
### Java 12
#### String 增强
indent() 方法可以实现字符串缩进。
transform() 方法可以用来转变指定字符串。
#### Files 增强（文件比较）
mismatch() 方法用于比较两个文件，并返回第一个不匹配字符的位置，如果文件相同则返回 -1L。
#### 数字格式化工具类
```
NumberFormat fmt = NumberFormat.getCompactNumberInstance(Locale.US, NumberFormat.Style.SHORT);
String result = fmt.format(1000);
System.out.println(result); // 1K
```
#### Shenandoah GC
和 Java11 开源的 ZGC 相比（需要升级到 JDK11 才能使用），Shenandoah GC 有稳定的 JDK8u 版本，在 Java8 占据主要市场份额的今天有更大的可落地性。
#### G1 收集器优化
Java12 为默认的垃圾收集器 G1 带来了两项更新:
- 可中止的混合收集集合
- 及时返回未使用的已分配内存
### Java 13
#### SocketAPI 重构
NioSocketImpl 是对 PlainSocketImpl 的直接替代，它使用 java.util.concurrent 包下的锁而不是同步方法。
#### FileSystems
#### 动态 CDS 存档
允许在 Java 应用程序执行结束时动态进行类归档，具体能够被归档的类包括所有已被加载，但不属于默认基层 CDS 的应用程序类和引用类库中的类。
### Java 14
#### 空指针异常精准提示
JVM 参数中添加-XX:+ShowCodeDetailsInExceptionMessages
#### 其他
- 移除了 CMS(Concurrent Mark Sweep) 垃圾收集器
### Java 15
#### CharSequence
CharSequence 接口添加了一个默认方法 isEmpty() 来判断字符序列为空，如果是则返回 true。
#### TreeMap
TreeMap 新引入了下面这些方法:
putIfAbsent()，computeIfAbsent()，computeIfPresent()，compute()，merge()
#### ZGC
在 Java 11 中实验性引入的 ZGC 在实际的使用中存在未能主动将未使用的内存释放给操作系统的问题。
在 Java 13 中，ZGC 将向操作系统返回被标识为长时间未使用的页面，这样它们将可以被其他进程重用。
ZGC 在 Java 15 已经可以正式使用了！默认的垃圾回收器依然是 G1。你可以通过下面的参数启动 ZGC
java -XX:+UseZGC className
#### EdDSA(数字签名算法)
虽然其性能优于现有的 ECDSA 实现，不过，它并不会完全取代 JDK 中现有的椭圆曲线数字签名算法( ECDSA)。
```
KeyPairGenerator kpg = KeyPairGenerator.getInstance("Ed25519");
KeyPair kp = kpg.generateKeyPair();

byte[] msg = "test_string".getBytes(StandardCharsets.UTF_8);

Signature sig = Signature.getInstance("Ed25519");
sig.initSign(kp.getPrivate());
sig.update(msg);
byte[] s = sig.sign();

String encodedString = Base64.getEncoder().encodeToString(s);
System.out.println(encodedString);
```
#### 文本块
Java 13 支持两个 """ 符号中间的任何内容都会被解释为字符串的一部分，包括换行符。
Java14 中，文本块依然是预览特性，不过，其引入了两个新的转义字符
- \ : 表示行尾，不引入换行符
- \s: 表示单个空格
#### 隐藏类(Hidden Classes)
隐藏类是为框架（frameworks）所设计的，隐藏类不能直接被其他类的字节码使用，只能在运行时生成类并通过反射间接使用它们。
#### 其他
- Nashorn JavaScript 引擎彻底移除
- DatagramSocket API 重构
- 禁用和废弃偏向锁。-XX:+UseBiasedLocking 启用偏向锁定
### Java 16
#### 启用 C++ 14 语言特性
Java 16 允许在 JDK 的 C++ 源代码中使用 C++14 语言特性。在 Java 15 中，JDK 中 C++ 代码使用的语言特性仅限于 C++98/03 语言标准。
#### ZGC 并发线程堆栈处理
#### 弹性元空间
弹性元空间这个特性可将未使用的 HotSpot 类元数据（即元空间，metaspace）内存更快速地返回到操作系统，从而减少元空间的占用空间。
#### 对基于值的类发出警告
早在 Java9 版本时，Java 的设计者们就对 @Deprecated 注解进行了一次升级，增加了 since 和 forRemoval 等 2 个新元素。
- 建议使用Integer a = 10;或者Integer.valueOf()函数
- 不建议在 synchronized 同步块中使用值类型。因为每次的count++操作，所产生的 hashcode 均不同，简而言之，每次加锁都锁在了不同的对象上。因此，如果希望在实际的开发过程中保证其原子性，应该使用 AtomicInteger。
#### 打包工具
在 Java 14 中，JEP 343 引入了打包工具，命令是 jpackage。在 Java 15 中，继续孵化，现在在 Java 16 中，终于成为了正式功能。
jpackage 工具，标配将应用打成 jar 包外，还支持不同平台的特性包，比如 linux 下的deb和rpm，window 平台下的msi和exe，macOS 上的 pkg 和 dmg。它还允许在打包时指定启动时参数，并且可以从命令行直接调用，也可以通过 ToolProvider API 以编程方式调用。注意 jpackage 模块名称从 jdk.incubator.jpackage 更改为 jdk.jpackage。这将改善最终用户在安装应用程序时的体验，并简化了“应用商店”模型的部署。
#### 外部内存访问 API(第三次孵化)
引入外部内存访问 API 的目的如下：
- 通用：单个 API 应该能够对各种外部内存（如本机内存、持久内存、堆内存等）进行操作。
- 安全：无论操作何种内存，API 都不应该破坏 JVM 的安全性。
- 控制：可以自由的选择如何释放内存（显式、隐式等）。
- 可用：如果需要访问外部内存，API 应该是 sun.misc.Unsafe。
#### 外部链接器 API(孵化)： 该孵化器 API 提供了静态类型、纯 Java 访问原生代码的特性，该 API 将大大简化绑定原生库的原本复杂且容易出错的过程。Java 1.1 就已通过 Java 原生接口（JNI）支持了原生方法调用，但并不好用。Java 开发人员应该能够为特定任务绑定特定的原生库。它还提供了外来函数支持，而无需任何中间的 JNI 粘合代码。
#### instanceof 模式匹配
Java 12 的instanceof 可以在判断是否属于具体的类型同时完成转换。
#### 记录类型
Java 14 record 关键字可以简化 数据类（一个 Java 类一旦实例化就不能再修改）的定义方式，使用 record 代替 class 定义的类，只需要声明属性，就可以在获得属性的访问方法。
在 Java 20 中，可以嵌套记录模式和类型模式结合使用，以实现强大的、声明性的和可组合的数据导航和处理形式。
记录模式不能单独使用，而是要与 instanceof 或 switch 模式匹配一同使用。
```
Shape circle = new Shape("Circle", 10);
if (circle instanceof Shape(String type, long unit)) {
  System.out.println("Area of " + type + " is : " + Math.PI * Math.pow(unit, 2));
}
```
非静态内部类可以定义非常量的静态成员。
在 Java 20 中，
#### 默认强封装 JDK 内部元素
此特性会默认强封装 JDK 的所有内部元素，但关键内部 API（例如 sun.misc.Unsafe）除外。
强封装由 JDK 9 的启动器选项–illegal-access 控制，到 JDK 15 默认改为 warning，从 JDK 16 开始默认为 deny。（目前）仍然可以使用单个命令行选项放宽对所有软件包的封装，将来只有使用–add-opens 打开特定的软件包才行。
#### 其他优化与改进
##### Unix-Domain 套接字通道
Unix-domain 套接字用于同一主机上的进程间通信（IPC）。它们在很大程度上类似于 TCP/IP，区别在于套接字是通过文件系统路径名而不是 Internet 协议（IP）地址和端口号寻址的。对于本地进程间通信，Unix-domain 套接字比 TCP/IP 环回连接更安全、更有效
##### 从 Mercurial 迁移到 Git：在此之前，OpenJDK 源代码是使用版本管理工具 Mercurial 进行管理，现在迁移到了 Git。
##### 移植 Alpine Linux：Alpine Linux 是一个独立的、非商业的 Linux 发行版，它十分的小，一个容器需要不超过 8MB 的空间，最小安装到磁盘只需要大约 130MB 存储空间，并且十分的简单，同时兼顾了安全性。
##### Windows/AArch64 移植：这些 JEP 的重点不是移植工作本身，而是将它们集成到 JDK 主线存储库中；
### Java 17（重要）
#### 增强的伪随机数生成器
JDK 17 之前，我们可以借助 Random、ThreadLocalRandom和SplittableRandom来生成随机数。不过，这 3 个类都各有缺陷，且缺少常见的伪随机算法支持。
Java 17 为伪随机数生成器 （pseudorandom number generator，PRNG（需要种子），又称为确定性随机位生成器）增加了新的接口类型和实现，使得开发者更容易在应用程序中互换使用各种 PRNG 算法。
```
RandomGeneratorFactory<RandomGenerator> l128X256MixRandom = RandomGeneratorFactory.of("L128X256MixRandom");
// 使用时间戳作为随机数种子
RandomGenerator randomGenerator = l128X256MixRandom.create(System.currentTimeMillis());
// 生成随机数
randomGenerator.nextInt(10);
```
#### 弃用 Applet API 以进行删除
#### 删除远程方法调用激活机制
#### 密封类
在 Java 15 中，密封类可以对继承或者实现它们的类进行限制，这样这个类就只能被指定的类继承。
另外，任何扩展密封类的类本身都必须声明为 sealed、non-sealed 或 final。
如果允许扩展的子类和封闭类在同一个源代码文件里，封闭类可以不使用 permits 语句。
```
// 抽象类 Person 只允许 Employee 和 Manager 继承。
public abstract sealed class Person
    permits Employee, Manager {

    //...
}
```
#### 删除实验性的 AOT 和 JIT 编译器
Java 17，删除实验性的提前 (AOT) 和即时 (JIT) 编译器，因为该编译器自推出以来很少使用，维护它所需的工作量很大。保留实验性的 Java 级 JVM 编译器接口 (JVMCI)，以便开发人员可以继续使用外部构建的编译器版本进行 JIT 编译。
#### 弃用安全管理器以进行删除
### Java 18
#### 默认字符集为 UTF-8
#### 简易的 Web 服务器
Java 18 之后，你可以使用 jwebserver 命令启动一个简易的静态 Web 服务器。这个服务器不支持 CGI 和 Servlet，只限于静态文件。
#### 优化 Java API 文档中的代码片段
@snippet 这种方式生成的效果更好且使用起来更方便一些。
#### 使用方法句柄重新实现反射核心
Java 18 改进了 java.lang.reflect.Method、Constructor 的实现逻辑，使之性能更好，速度更快。这项改动不会改动相关 API ，这意味着开发中不需要改动反射相关代码，就可以体验到性能更好反射。
#### 向量 API（第三次孵化）
在 Java 16 中，孵化器 API 提供了一个 API 的初始迭代以表达一些向量计算，这些计算在运行时可靠地编译为支持的 CPU 架构上的最佳向量硬件指令，从而获得优于同等标量计算的性能，充分利用单指令多数据（SIMD）技术（大多数现代 CPU 上都可以使用的一种指令）。尽管 HotSpot 支持自动向量化，但是可转换的标量操作集有限且易受代码更改的影响。该 API 将使开发人员能够轻松地用 Java 编写可移植的高性能向量算法。
```
void scalarComputation(float[] a, float[] b, float[] c) {
   for (int i = 0; i < a.length; i++) {
        c[i] = (a[i] * a[i] + b[i] * b[i]) * -1.0f;
   }
}
// 使用 Vector API 进行的等效向量计算
static final VectorSpecies<Float> SPECIES = FloatVector.SPECIES_PREFERRED;
void vectorComputation(float[] a, float[] b, float[] c) {
    int i = 0;
    int upperBound = SPECIES.loopBound(a.length);
    for (; i < upperBound; i += SPECIES.length()) {
        // FloatVector va, vb, vc;
        var va = FloatVector.fromArray(SPECIES, a, i);
        var vb = FloatVector.fromArray(SPECIES, b, i);
        var vc = va.mul(va)
                   .add(vb.mul(vb))
                   .neg();
        vc.intoArray(c, i);
    }
}
```
#### 互联网地址解析 SPI
Java 18 定义了一个全新的 SPI（service-provider interface），用于主要名称和地址的解析，以便 java.net.InetAddress 可以使用平台之外的第三方解析器。
### Java 19
#### 外部函数和内存 API（预览）
Java 程序可以通过该 API 与 Java 运行时之外的代码和数据进行互操作。通过高效地调用外部函数（即 JVM 之外的代码）和安全地访问外部内存（即不受 JVM 管理的内存），该 API 使 Java 程序能够调用本机库并处理本机数据，而不会像 JNI 那样危险和脆弱。
在没有外部函数和内存 API 之前：
- Java 通过 sun.misc.Unsafe 提供一些执行低级别、不安全操作的方法（如直接访问系统内存资源、自主管理内存资源等）
- Java 1.1 就已通过 Java 原生接口（JNI）支持了原生方法调用，但并不好用。JNI 实现起来过于复杂，步骤繁琐。不受 JVM 的语言安全机制控制，影响 Java 语言的跨平台特性。并且，JNI 的性能也不行，因为 JNI 方法调用不能从许多常见的 JIT 优化(如内联)中受益。
Foreign Function & Memory API (FFM API) 定义了类和接口：
- 分配外部内存：MemorySegment、、MemoryAddress和SegmentAllocator）
- 操作和访问结构化的外部内存：MemoryLayout, VarHandle
- 控制外部内存的分配和释放：MemorySession
- 调用外部函数：Linker、FunctionDescriptor和SymbolLookup
#### 虚拟线程（预览）
虚拟线程在其他多线程语言中已经被证实是十分有用的，比如 Go 中的 Goroutine、Erlang 中的进程。
虚拟线程避免了上下文切换的额外耗费，兼顾了多线程的优点，简化了高并发程序的复杂，可以有效减少编写、维护和观察高吞吐量并发应用程序的工作量。
#### 结构化并发(孵化)
结构化并发的基本 API 是StructuredTaskScope。StructuredTaskScope 支持将任务拆分为多个并发子任务，在它们自己的线程中执行，并且子任务必须在主任务继续之前完成。
在 Java 20 中，进行第二次孵化，更新为支持在任务范围内创建的线程StructuredTaskScope继承范围值 这简化了跨线程共享不可变数据
### Java 20
#### 作用域值（第一次孵化）
作用域值（Scoped Values）它可以在线程内和线程间共享不可变的数据，优于线程局部变量，尤其是在使用大量虚拟线程时。
```
final static ScopedValue<...> V = new ScopedValue<>();

// In some method
ScopedValue.where(V, <value>)
           .run(() -> { ... V.get() ... call methods ... });

// In a method called directly or indirectly from the lambda expression
... V.get() ...
```
### Java 21（重要）
#### 字符串模板（预览）
Java 目前支持三种模板处理器：
- STR：自动执行字符串插值，即将模板中的每个嵌入式表达式替换为其值（转换为字符串）。
- FMT：和 STR 类似，但是它还可以接受格式说明符，这些格式说明符出现在嵌入式表达式的左边，用来控制输出的样式
- RAW：不会像 STR 和 FMT 模板处理器那样自动处理字符串模板，而是返回一个 StringTemplate 对象，这个对象包含了模板中的文本和表达式的信息
除了 JDK 自带的三种模板处理器外，你还可以实现 StringTemplate.Processor 接口来创建自己的模板处理器。
我们可以使用局部变量、静态/非静态字段甚至方法作为嵌入表达式。
还可以在表达式中执行计算并打印结果。
```
String name = "Lokesh";

//STR
String message = STR."Greetings \{name}.";

//FMT
String message = STR."Greetings %-12s\{name}.";

//RAW
StringTemplate st = RAW."Greetings \{name}.";
String message = STR.process(st);

//variable
message = STR."Greetings \{name}!";

//method
message = STR."Greetings \{getName()}!";

//field
message = STR."Greetings \{this.name}!";

int x = 10, y = 20;
String s = STR."\{x} + \{y} = \{x + y}";  //"10 + 20 = 30"
```
除了 JDK 自带的三种模板处理器外，你还可以实现 StringTemplate.Processor 接口来创建自己的模板处理器。
#### 序列化集合
Sequenced Collections（序列化集合，也叫有序集合），这是一种具有确定出现顺序（encounter order）的集合（无论我们遍历这样的集合多少次，元素的出现顺序始终是固定的）。序列化集合提供了处理集合的第一个和最后一个元素以及反向视图（与原始集合相反的顺序）的简单方法。
#### 分代 ZGC
默认是关闭的，需要通过配置打开
```
// 启用分代ZGC
java -XX:+UseZGC -XX:+ZGenerational ...
```
#### switch 的模式匹配
在 Java 12 中，增强了 switch 表达式，使用类似 lambda 语法条件匹配成功后的执行块，不需要多写 break 。
```
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY                -> System.out.println(7);
    case THURSDAY, SATURDAY     -> System.out.println(8);
    case WEDNESDAY              -> System.out.println(9);
}
```
在 Java 13 中，增强 Switch(引入 yield 关键字到 Switch 中)
yield和 return 的区别在于：return 会直接跳出当前循环或者方法，而 yield 只会跳出当前 Switch 块，同时在使用 yield 时，需要有 default 条件
在 Java 14 中，以上功能全部转正。
在 Java 17 中，支持null
```
// New code
static String formatterPatternSwitch(Object o) {
    return switch (o) {
        case null      -> "Oops";
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> o.toString();
    };
}
```
在 Java 21 中，以上功能全部转正。
#### 未命名模式和变量（预览）
未命名模式和变量使得我们可以使用下划线 _ 表示未命名的变量以及模式匹配时不使用的组件，旨在提高代码的可读性和可维护性。
未命名变量的典型场景是 try-with-resources 语句、 catch 子句中的异常变量和for循环。当变量不需要使用的时候就可以使用下划线 _代替，这样清晰标识未被使用的变量。
#### 未命名类和实例 main 方法 （预览）
```
void main() {
   System.out.println("Hello, World!");
}
```
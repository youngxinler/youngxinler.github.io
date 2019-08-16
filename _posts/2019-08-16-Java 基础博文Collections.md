---
title: Java 基础博文Collections
tags: [Java,Collections]
---
## 面向对象  
#### 什么是面向对象  
[面向对象、面向过程](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/object-oriented-vs-procedure-oriented.md)

[面向对象的三大基本特征](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/characteristics.md)

[五大基本原则](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/principle.md)

#### 平台无关性  
[Java如何实现的平台无关性的](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/platform-independent.md)

[JVM还支持哪些语言（Kotlin、Groovy、JRuby、Jython、Scala）](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/jvm-language.md)

#### 值传递  
[值传递、引用传递](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/java-pass-by.md)

[为什么说Java中只有值传递](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/java-pass-by.md)

#### 封装、继承、多态  
[什么是多态](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/polymorphism.md)

[方法重写与重载](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/overloading-vs-overriding.md)

Java的继承与实现

[Java的继承与组合](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/inheritance-composition.md)

[构造函数与默认构造函数](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/constructor.md)

[类变量、成员变量和局部变量](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/variable.md)

[成员变量和方法作用域](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/scope.md)

## Java基础知识  
#### 基本数据类型
[7种基本数据类型：整型、浮点型、布尔型、字符型](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/basic-data-types.md)

[整型中byte、short、int、long的取值范围](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/integer-scope.md)

[什么是浮点型？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/float.md)

[什么是单精度和双精度？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/single-double-float.md)

[为什么不能用浮点型表示金额？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/float-amount.md)
#### 自动拆装箱
[什么是包装类型、什么是基本类型、什么是自动拆装箱](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/boxing-unboxing.md)

[Integer的缓存机制](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/integer-cache.md)
#### String
[字符串的不可变性](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/final-string.md)

[JDK 6和JDK 7中substring的原理及区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/substring.md)

replaceFirst、replaceAll、replace区别、

[String对“+”的重载](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/string-append.md)

[字符串拼接的几种方式和区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/string-concat.md)

[String.valueOf和Integer.toString的区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/value-of-vs-to-string.md)

[switch对String的支持](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/switch-string.md)

字符串池、常量池（运行时常量池、Class常量池）、intern
#### 熟悉Java中各种关键字
transient

instanceof

volatile

synchronized

final

static

const
#### 集合类
常用集合类的使用

[ArrayList和LinkedList和Vector的区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/arraylist-vs-linkedlist-vs-vector.md)

[SynchronizedList和Vector的区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/synchronizedlist-vector.md)

[HashMap、HashTable、ConcurrentHashMap区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/HashMap-HashTable-ConcurrentHashMap.md)

[Set和List区别？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/set-vs-list.md)

[Set如何保证元素不重复?](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/set-repetition.md)

[Java 8中stream相关用法、](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/stream.md)

apache集合处理工具类的使用、

不同版本的JDK中HashMap的实现的区别以及原因

[Collection和Collections区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/Collection-vs-Collections.md)

[Arrays.asList获得的List使用时需要注意什么](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/Arrays-asList.md)

[Enumeration和Iterator区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/Enumeration-vs-Iterator.md)

[fail-fast 和 fail-safe](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/fail-fast-vs-fail-safe.md)

[CopyOnWriteArrayList](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/CopyOnWriteArrayList.md)

[ConcurrentSkipListMap](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/ConcurrentSkipListMap.md)
#### 枚举
[枚举的用法](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-usage.md)

[枚举的实现](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-impl.md)

[枚举与单例](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-singleton.md)

Enum类

[Java枚举如何比较](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-compare.md)

[switch对枚举的支持](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-switch.md)

[枚举的序列化如何实现](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-serializable.md)

[枚举的线程安全性问题](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-thread-safe.md)
#### IO
[字符流、字节流](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/byte-stream-vs-character-stream.md)

[输入流、输出流](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/input-stream-vs-output-stream.md)

[同步、异步、阻塞、非阻塞、Linux 5种IO模型](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/synchronized-vs-asynchronization.md)

[BIO、NIO和AIO的区别、三种IO的用法与原理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/bio-vs-nio-vs-aio.md)

netty
#### 反射
[反射与工厂模式](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/reflection.md)

[反射有什么作用](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/usage-of-reflection.md)

[Class类](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/Class.md)

java.lang.reflect.*
#### 动态代理
[静态代理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/static-proxy.md)

[动态代理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/dynamic-proxy.md)

[动态代理和反射的关系](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/dynamic-proxy-vs-reflection.md)

[动态代理的几种实现方式](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/dynamic-proxy-implementation.md)

[AOP](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/aop-vs-proxy.md)
#### 序列化
[什么是序列化与反序列化](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/serialize.md)

为什么序列化

[序列化底层原理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/serialize-principle.md)

[序列化与单例模式](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/serialize-singleton.md)

protobuf

为什么说序列化并不安全
#### 注解
[元注解](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/meta-annotation.md)

[自定义注解](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/custom-annotation.md)

Java中常用注解使用

注解与反射的结合

[如何自定义一个注解？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/create-annotation.md)

[Spring常用注解](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/annotation-in-spring.md)
#### JMS (Java消息服务（Java Message Service）)
[什么是Java消息服务](https://zh.wikipedia.org/wiki/Java消息服务)

JMS消息传送模型
#### JMX
[什么是JMX](https://zh.wikipedia.org/wiki/JMX)

java.lang.management.*

javax.management.*
#### 泛型
泛型与继承

类型擦除

[泛型中K T V E ？ object等的含义](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/k-t-v-e.md)

泛型各种用法

限定通配符和非限定通配符

上下界限定符extends 和 super

[List和原始类型List之间的区别?](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/genericity-list.md)

[List\<?>和List之间的区别是什么?](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/genericity-list-wildcard.md)

#### 单元测试
junit

mock

mockito

内存数据库（h2）

#### 正则表达式
java.lang.util.regex.*

#### 常用的Java工具库
commons.lang

commons.\*... 

guava-libraries

netty

#### API&SPI
API

[API和SPI的关系和区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/api-vs-spi.md)

[如何定义SPI](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/create-spi.md)

[SPI的实现原理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/spi-principle.md)
#### 异常
异常类型、正确处理异常、自定义异常

Error和Exception

异常链、try-with-resources

finally和return的执行顺序

#### 时间处理
时区、冬令时和夏令时、时间戳、Java中时间API

格林威治时间、CET,UTC,GMT,CST几种常见时间的含义和关系

SimpleDateFormat的线程安全性问题

Java 8中的时间处理

如何在东八区的计算机上获取美国时间

#### 编码方式
Unicode、有了Unicode为啥还需要UTF-8

GBK、GB2312、GB18030之间的区别

UTF8、UTF16、UTF32区别

URL编解码、Big Endian和Little Endian

如何解决乱码问题

#### 语法糖
Java中语法糖原理、解语法糖

语法糖：switch 支持 String 与枚举、泛型、自动装箱与拆箱、方法变长参数、枚举、内部类、条件编译、 断言、数值字面量、
for-each、try-with-resource、Lambda表达式、
## 阅读源代码  



## Java并发编程  